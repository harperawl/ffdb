# FFDB database documentation

### Info

The database is created using Apache Parquet files generated via PyArrow and DuckDB for the database engine. Technically, you can interact with the Parquet files however you'd like; they're just compressed spreadsheets under the hood. However, using DuckDB's SQL query engine is going to be the fastest and easiest for most applications.

The design of the database went through a few iterations. My first ever attempt at something like this was simply keeping the game JSON files in a folder and looping through them. Of course, this was extremely slow. My next attempt used Postgres, but it was again too slow for most queries; I spent most of my time creating indexes instead of actually making queries. It turns out that the transaction-oriented OLTP system that Postgres implements isn't optimal for this use case, but rather a column-oriented OLAP system instead. DuckDB is one of the most widely used implementations of this, and it's worked quite nicely for me.

My design philosophy for the schema (tables, columns, etc.) was that I basically stuck to the format of the original game JSON files as much as possible. This choice was made because I was already familiar with the formatting that the MLB API used, and I figured anyone who wanted to use my database in the future would probably be familiar with it too. Admittedly, this results in some shortcomings and perhaps poor UX design choices, which you'll probably see soon. However, in the interest of backwards compatibility with my own internal code as well as consistency for anyone else using this, I'll be keeping the naming system.

### Structure

The basic structure of the database is the following two namespaces, which contain the following tables:

**Main:**

| Table | Description |
| --- | --- |
| `games` | Game-level information, like teams, venue, game date |
| `plays` | Plate appearance information, like matchup and result of PA | 
| `events` | Pitch-level event information, like pitch tracking and ABS |
| `play_credits` | PA-level at-bat and plate appearance credits |
| `runners` | PA-level runner information, like player ID and start/end base |
| `runner_credits` | PA-level putout credits containing player ID and out type |
| `batting_logs` | Game-level offensive stats, like hits, PAs, ABs, etc. |
| `pitching_logs` | Game-level pitching stats, like strikeouts, pitch count, batters faced, etc. |
| `fielding_logs` | Game-level fielding stats, like putouts, pickoffs, caught stealing, etc. |
| `position_logs` | Game-level logs for each player and what position they played |

**Reference:** (note that this is still very much a work in progress)

| Table | Description |
| --- | --- |
| `ref.players` | Matches player biographical info with player ID |
| `ref.teams` | Matches general team info with team ID |
| `ref.pitch_codes` | Translates pitch result codes into useful booleans |

To see exactly which columns are in each table, you can: 
- Look at the database with the DuckDB UI (or some other DuckDB viewer)
- Look at the PyArrow schemas defined in each script in the [`extractors` folder](/extractors/)

Most columns should be easy to understand; you can find more information for most pitch tracking metrics in the [Statcast CSV documentation](https://baseballsavant.mlb.com/csv-docs).
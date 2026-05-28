### Database Schema Overview
 * **batting_logs:** Aggregated offensive (batting) statistics for individual players on a per-game basis.
 * **events:** Highly granular, pitch-by-pitch data (similar to Statcast). It contains physics metrics (velocity, spin, break) and micro-event details.
 * **fielding_logs:** Aggregated defensive (fielding) statistics for players on a per-game basis.
 * **games:** High-level metadata for individual games, encompassing schedule details, venue, weather, and attendance.
 * **pitching_logs:** Aggregated pitching performance metrics and statistics for players on a per-game basis.
 * **play_credits:** A relational mapping table assigning specific credits (e.g., assists, putouts) to players during a specific at-bat.
 * **player_logs:** Game-level player roster metadata, capturing details like jersey numbers and batting order assignments.
 * **plays:** At-bat level outcome data, summarizing the result of a plate appearance, inning state, and scoring.
 * **position_logs:** A tracking table for the specific defensive positions a player occupied during a game.
 * **runner_credits:** A relational mapping table assigning specific baserunning credits to players during an event.
 * **runners:** Granular tracking of baserunner movements, tracking origin bases, destinations, advancement reasons, and out statuses per play.
### Table Documentation
#### 1. batting_logs
**Table Summary:** This table records the summarized offensive performance of a single player during a specific game. It tracks standard box-score batting metrics.
| Field Name | Data Type | Definition | Source Description | Source Column | Example Value |
|---|---|---|---|---|---|
| game_id | uint32 | Unique identifier for the game. | Unknown | Unknown | 715732 |
| player | uint32 | Unique identifier for the player. | Unknown | Unknown | 605141 |
| summary | string | Text summary of the player's batting performance. | Unknown | Unknown | "2-4, 1 HR, 3 RBI" |
| games_played | uint8 | Indicates if the player played in the game (typically 1 or 0). | Unknown | Unknown | 1 |
| fly_outs | uint8 | Total fly outs hit by the batter. | Unknown | Unknown | 2 |
| ground_outs | uint8 | Total ground outs hit by the batter. | Unknown | Unknown | 1 |
| air_outs | uint8 | Total air outs (fly outs + pop outs). | Unknown | Unknown | 2 |
| runs | uint8 | Total runs scored by the player. | Unknown | Unknown | 1 |
| doubles | uint8 | Total doubles hit. | Unknown | Unknown | 0 |
| triples | uint8 | Total triples hit. | Unknown | Unknown | 0 |
| home_runs | uint8 | Total home runs hit. | Unknown | Unknown | 1 |
| strike_outs | uint8 | Total times the batter struck out. | Unknown | Unknown | 1 |
| base_on_balls | uint8 | Total walks (BB) drawn. | Unknown | Unknown | 1 |
| intentional_walks | uint8 | Total intentional walks (IBB) drawn. | Unknown | Unknown | 0 |
| hits | uint8 | Total base hits. | Unknown | Unknown | 2 |
| hit_by_pitch | uint8 | Total times hit by a pitch (HBP). | Unknown | Unknown | 0 |
| at_bats | uint8 | Total official at-bats (AB). | Unknown | Unknown | 4 |
| caught_stealing | uint8 | Times caught stealing (CS) as a baserunner. | Unknown | Unknown | 0 |
| stolen_bases | uint8 | Total stolen bases (SB). | Unknown | Unknown | 1 |
| ground_into_double_play | uint8 | Times grounded into a double play (GIDP). | Unknown | Unknown | 0 |
| ground_into_triple_play | uint8 | Times grounded into a triple play (GITP). | Unknown | Unknown | 0 |
| plate_appearances | uint8 | Total plate appearances (PA). | Unknown | Unknown | 5 |
| total_bases | uint16 | Total bases accumulated from hits. | Unknown | Unknown | 5 |
| rbi | uint16 | Runs batted in. | Unknown | Unknown | 3 |
| left_on_base | uint16 | Runners left on base at the end of the inning when this batter was up or made the final out. | Unknown | Unknown | 2 |
| sac_bunts | uint16 | Total sacrifice bunts (SH). | Unknown | Unknown | 0 |
| sac_flies | uint16 | Total sacrifice flies (SF). | Unknown | Unknown | 0 |
| catchers_interference | uint16 | Times reaching base via catcher's interference. | Unknown | Unknown | 0 |
| pickoffs | uint16 | Times picked off while on base. | Unknown | Unknown | 0 |
| pop_outs | uint16 | Total pop outs hit by the batter. | Unknown | Unknown | 0 |
| line_outs | uint16 | Total line outs hit by the batter. | Unknown | Unknown | 1 |
#### 2. events
**Table Summary:** Highly granular pitch-by-pitch tracking data. Contains exact physics metrics (velocity, spin, break, launch angle) and detailed state mapping for every pitch or event during an at-bat.
| Field Name | Data Type | Definition | Source Description | Source Column | Example Value |
|---|---|---|---|---|---|
| game_id | uint32 | Unique identifier for the game. | Unknown | Unknown | 715732 |
| at_bat_index | uint16 | Sequential index of the at-bat within the game. | Unknown | Unknown | 45 |
| event_index | uint8 | Sequential index of the pitch/event within the at-bat. | Unknown | Unknown | 3 |
| description | string | Text description of the pitch/event result. | Unknown | Unknown | "Called Strike" |
| event_type | dict(uint8, string) | Categorized type of event (e.g., pitch, pickoff). | Unknown | Unknown | "pitch" |
| code | dict(uint8, string) | Shorthand code for the pitch result (e.g., 'C', 'S', 'B'). | Unknown | Unknown | "C" |
| away_score | uint8 | Current away team score. | Unknown | Unknown | 2 |
| home_score | uint8 | Current home team score. | Unknown | Unknown | 4 |
| is_in_play | boolean | True if the ball was hit into play. | Unknown | Unknown | false |
| is_strike | boolean | True if the pitch was a strike (called or swinging). | Unknown | Unknown | true |
| is_ball | boolean | True if the pitch was a ball. | Unknown | Unknown | false |
| pitch_type | dict(uint8, string) | Classification of the pitch (e.g., FF, SL, CH). | Unknown | Unknown | "FF" |
| is_scoring_play | boolean | True if a run scored on this event. | Unknown | Unknown | false |
| is_out | boolean | True if an out was recorded on this event. | Unknown | Unknown | false |
| has_review | boolean | True if the play was reviewed by umpires. | Unknown | Unknown | false |
| balls | uint8 | Current ball count *after* the event. | Unknown | Unknown | 1 |
| strikes | uint8 | Current strike count *after* the event. | Unknown | Unknown | 2 |
| outs | uint8 | Current out count *after* the event. | Unknown | Unknown | 1 |
| pre_balls | uint8 | Ball count *before* the event. | Unknown | Unknown | 1 |
| pre_strikes | uint8 | Strike count *before* the event. | Unknown | Unknown | 1 |
| pre_outs | uint8 | Out count *before* the event. | Unknown | Unknown | 1 |
| start_speed | float32 | Velocity of pitch at release (mph). | Unknown | Unknown | 95.4 |
| end_speed | float32 | Velocity of pitch crossing the plate (mph). | Unknown | Unknown | 87.1 |
| strike_zone_top | float32 | Top of the batter's specific strike zone (feet). | Unknown | Unknown | 3.45 |
| strike_zone_bottom | float32 | Bottom of the batter's specific strike zone (feet). | Unknown | Unknown | 1.60 |
| a_x | float32 | Acceleration of the pitch on the X-axis. | Unknown | Unknown | -12.5 |
| a_y | float32 | Acceleration of the pitch on the Y-axis. | Unknown | Unknown | 28.2 |
| a_z | float32 | Acceleration of the pitch on the Z-axis. | Unknown | Unknown | -15.3 |
| pfx_x | float32 | Horizontal movement of the pitch (inches). | Unknown | Unknown | -4.2 |
| pfx_z | float32 | Vertical movement of the pitch (inches). | Unknown | Unknown | 8.1 |
| p_x | float32 | X-coordinate of the pitch crossing the plate. | Unknown | Unknown | 0.45 |
| p_z | float32 | Z-coordinate (height) of the pitch crossing the plate. | Unknown | Unknown | 2.15 |
| v_x0 | float32 | Velocity vector X at release. | Unknown | Unknown | 4.1 |
| v_y0 | float32 | Velocity vector Y at release. | Unknown | Unknown | -139.5 |
| v_z0 | float32 | Velocity vector Z at release. | Unknown | Unknown | -5.2 |
| x | float32 | Standardized X-coordinate of the pitch. | Unknown | Unknown | 112.5 |
| y | float32 | Standardized Y-coordinate of the pitch. | Unknown | Unknown | 180.2 |
| x0 | float32 | X-coordinate of release point. | Unknown | Unknown | -2.1 |
| y0 | float32 | Y-coordinate of release point. | Unknown | Unknown | 50.0 |
| z0 | float32 | Z-coordinate of release point. | Unknown | Unknown | 6.2 |
| break_angle | float32 | Angle of the pitch break. | Unknown | Unknown | 25.4 |
| break_length | float32 | Magnitude of the pitch break. | Unknown | Unknown | 5.8 |
| break_y | float32 | Y-coordinate where the pitch break occurs. | Unknown | Unknown | 23.8 |
| break_vertical | float32 | Vertical component of the pitch break. | Unknown | Unknown | 4.2 |
| break_vertical_induced | float32 | Induced vertical break (spin-induced). | Unknown | Unknown | 16.5 |
| break_horizontal | float32 | Horizontal component of the pitch break. | Unknown | Unknown | -3.1 |
| spin_rate | uint16 | Revolutions per minute (RPM) of the pitch. | Unknown | Unknown | 2450 |
| spin_direction | uint16 | Tilt/axis of the spin (degrees). | Unknown | Unknown | 185 |
| launch_speed | float32 | Exit velocity of the batted ball (mph). | Unknown | Unknown | 104.5 |
| launch_angle | float32 | Vertical angle of the batted ball (degrees). | Unknown | Unknown | 22.5 |
| total_distance | float32 | Projected distance of the batted ball (feet). | Unknown | Unknown | 395.2 |
| trajectory | dict(uint8, string) | Batted ball type (e.g., grounder, line drive). | Unknown | Unknown | "line_drive" |
| hardness | dict(uint8, string) | Categorization of hit velocity (e.g., hard, medium). | Unknown | Unknown | "hard" |
| hit_coord_x | float32 | X-coordinate of where the ball was fielded/landed. | Unknown | Unknown | 125.0 |
| hit_coord_y | float32 | Y-coordinate of where the ball was fielded/landed. | Unknown | Unknown | 200.5 |
| zone | uint8 | 1-14 strike zone location grid identifier. | Unknown | Unknown | 5 |
| type_confidence | float32 | System confidence in pitch classification. | Unknown | Unknown | 0.98 |
| plate_time | float32 | Time in seconds for the pitch to reach the plate. | Unknown | Unknown | 0.42 |
| extension | float32 | Distance from pitching rubber to release point (ft). | Unknown | Unknown | 6.5 |
| play_id | string | Unique string identifier for the specific event. | Unknown | Unknown | "abc-123-xyz" |
| pitch_number | uint8 | Pitch count within the at-bat. | Unknown | Unknown | 3 |
| start_time | string | Timestamp of event start. | Unknown | Unknown | "2026-05-28T19:05:00Z" |
| end_time | string | Timestamp of event completion. | Unknown | Unknown | "2026-05-28T19:05:15Z" |
| is_pitch | boolean | True if the event was an actual pitch. | Unknown | Unknown | true |
| type | dict(uint8, string) | Event categorization. | Unknown | Unknown | "pitch" |
| is_overturned | boolean | True if a review overturned the call. | Unknown | Unknown | false |
| review_type | dict(uint16, string) | Type of replay review requested. | Unknown | Unknown | "force_play" |
| challenge_team_id | uint32 | Team ID initiating the challenge. | Unknown | Unknown | 119 |
| challenge_player | uint32 | Player ID involved in the challenge. | Unknown | Unknown | 545361 |
| pitcher | uint32 | Unique ID of the pitcher. | Unknown | Unknown | 608331 |
| pitch_hand | dict(uint8, string) | Pitcher handedness ('R' or 'L'). | Unknown | Unknown | "R" |
| catcher | uint32 | Unique ID of the catcher. | Unknown | Unknown | 669221 |
| first_baseman | uint32 | Unique ID of the first baseman. | Unknown | Unknown | 663656 |
| second_baseman | uint32 | Unique ID of the second baseman. | Unknown | Unknown | 642731 |
| third_baseman | uint32 | Unique ID of the third baseman. | Unknown | Unknown | 669256 |
| shortstop | uint32 | Unique ID of the shortstop. | Unknown | Unknown | 608369 |
| left_fielder | uint32 | Unique ID of the left fielder. | Unknown | Unknown | 660271 |
| center_fielder | uint32 | Unique ID of the center fielder. | Unknown | Unknown | 665742 |
| right_fielder | uint32 | Unique ID of the right fielder. | Unknown | Unknown | 605141 |
| batter | uint32 | Unique ID of the batter. | Unknown | Unknown | 660271 |
| bat_side | dict(uint8, string) | Batter stance ('R', 'L', or 'S'). | Unknown | Unknown | "L" |
| runner_on_first | uint32 | Unique ID of runner on first (if any). | Unknown | Unknown | 641355 |
| runner_on_second | uint32 | Unique ID of runner on second (if any). | Unknown | Unknown | Unknown |
| runner_on_third | uint32 | Unique ID of runner on third (if any). | Unknown | Unknown | Unknown |
| official_home_plate | uint32 | Unique ID of home plate umpire. | Unknown | Unknown | 427011 |
| official_first_base | uint32 | Unique ID of first base umpire. | Unknown | Unknown | 427012 |
| official_second_base | uint32 | Unique ID of second base umpire. | Unknown | Unknown | 427013 |
| official_third_base | uint32 | Unique ID of third base umpire. | Unknown | Unknown | 427014 |
| official_left_field | uint32 | Unique ID of left field umpire (if any). | Unknown | Unknown | Unknown |
| official_right_field | uint32 | Unique ID of right field umpire (if any). | Unknown | Unknown | Unknown |
#### 3. fielding_logs
**Table Summary:** Records the aggregated defensive statistics for a single player during a specific game.
| Field Name | Data Type | Definition | Source Description | Source Column | Example Value |
|---|---|---|---|---|---|
| game_id | uint32 | Unique identifier for the game. | Unknown | Unknown | 715732 |
| player | uint32 | Unique identifier for the player. | Unknown | Unknown | 608369 |
| caught_stealing | uint8 | Number of times catching a runner stealing (Catchers). | Unknown | Unknown | 1 |
| stolen_bases | uint8 | Number of stolen bases allowed (Pitchers/Catchers). | Unknown | Unknown | 0 |
| assists | uint8 | Total defensive assists made. | Unknown | Unknown | 4 |
| put_outs | uint8 | Total putouts recorded. | Unknown | Unknown | 2 |
| errors | uint8 | Total fielding errors committed. | Unknown | Unknown | 0 |
| chances | uint8 | Total fielding chances (putouts + assists + errors). | Unknown | Unknown | 6 |
| passed_ball | uint8 | Total passed balls allowed (Catchers). | Unknown | Unknown | 0 |
| pickoffs | uint8 | Total pickoffs executed. | Unknown | Unknown | 0 |
#### 4. games
**Table Summary:** Contains top-level metadata about a game, such as schedule information, team matchups, conditions, and attendance.
| Field Name | Data Type | Definition | Source Description | Source Column | Example Value |
|---|---|---|---|---|---|
| game_id | uint32 | Unique identifier for the game. | Unknown | Unknown | 715732 |
| game_type | dict(uint8, string) | Game classification (e.g., Regular Season, Playoffs). | Unknown | Unknown | "R" |
| double_header | dict(uint8, string) | Indicates if the game is part of a doubleheader (Y/N). | Unknown | Unknown | "N" |
| gameday_type | dict(uint8, string) | Specific classification of the gameday event. | Unknown | Unknown | "E" |
| tiebreaker | dict(uint8, string) | Indicates if the game is a tiebreaker (Y/N). | Unknown | Unknown | "N" |
| season | uint16 | The year of the baseball season. | Unknown | Unknown | 2026 |
| date_time | string | Scheduled start date and time of the game. | Unknown | Unknown | "2026-05-28T18:40:00Z" |
| tz_offset | decimal(3,2) | Timezone offset from UTC. | Unknown | Unknown | -7.00 |
| status_code | string | Code representing game status (e.g., F for Final). | Unknown | Unknown | "F" |
| away_team_id | uint32 | Unique identifier for the away team. | Unknown | Unknown | 119 |
| home_team_id | uint32 | Unique identifier for the home team. | Unknown | Unknown | 137 |
| venue_id | uint32 | Unique identifier for the stadium/venue. | Unknown | Unknown | 32 |
| weather_condition | dict(uint8, string) | General weather description (e.g., Sunny, Dome). | Unknown | Unknown | "Clear" |
| temperature | int16 | Temperature at first pitch (Fahrenheit). | Unknown | Unknown | 72 |
| wind_speed | uint8 | Wind speed at first pitch (mph). | Unknown | Unknown | 8 |
| wind_direction | dict(uint8, string) | Wind direction (e.g., Out to Center). | Unknown | Unknown | "In from RF" |
| attendance | uint32 | Paid attendance for the game. | Unknown | Unknown | 41235 |
#### 5. pitching_logs
**Table Summary:** Aggregated pitching metrics and statistics for a single pitcher during a specific game.
| Field Name | Data Type | Definition | Source Description | Source Column | Example Value |
|---|---|---|---|---|---|
| game_id | uint32 | Unique identifier for the game. | Unknown | Unknown | 715732 |
| player | uint32 | Unique identifier for the pitcher. | Unknown | Unknown | 608331 |
| note | string | Text note regarding the pitcher's performance. | Unknown | Unknown | "W (5-1)" |
| summary | string | Short string summary of pitching line (IP, H, ER). | Unknown | Unknown | "6.0 IP, 4 H, 1 ER, 7 K" |
| games_played | uint8 | Indicates if the pitcher pitched in the game. | Unknown | Unknown | 1 |
| games_started | uint8 | Indicates if the pitcher started the game. | Unknown | Unknown | 1 |
| fly_outs | uint8 | Total fly outs induced. | Unknown | Unknown | 4 |
| ground_outs | uint8 | Total ground outs induced. | Unknown | Unknown | 6 |
| air_outs | uint8 | Total air outs induced. | Unknown | Unknown | 5 |
| runs | uint8 | Total runs allowed. | Unknown | Unknown | 1 |
| doubles | uint8 | Total doubles allowed. | Unknown | Unknown | 1 |
| triples | uint8 | Total triples allowed. | Unknown | Unknown | 0 |
| home_runs | uint8 | Total home runs allowed. | Unknown | Unknown | 0 |
| strike_outs | uint8 | Total batters struck out. | Unknown | Unknown | 7 |
| base_on_balls | uint8 | Total walks allowed. | Unknown | Unknown | 2 |
| intentional_walks | uint8 | Total intentional walks allowed. | Unknown | Unknown | 0 |
| hits | uint8 | Total hits allowed. | Unknown | Unknown | 4 |
| hit_by_pitch | uint8 | Total batters hit by pitch. | Unknown | Unknown | 0 |
| at_bats | uint8 | Total official at-bats against the pitcher. | Unknown | Unknown | 22 |
| caught_stealing | uint8 | Total runners caught stealing while pitching. | Unknown | Unknown | 0 |
| stolen_bases | uint8 | Total stolen bases allowed while pitching. | Unknown | Unknown | 1 |
| wins | uint8 | Win credited to the pitcher (1 or 0). | Unknown | Unknown | 1 |
| losses | uint8 | Loss credited to the pitcher (1 or 0). | Unknown | Unknown | 0 |
| saves | uint8 | Save credited to the pitcher (1 or 0). | Unknown | Unknown | 0 |
| save_opportunities | uint8 | Indicates if the pitcher had a save opportunity. | Unknown | Unknown | 0 |
| holds | uint8 | Hold credited to the pitcher (1 or 0). | Unknown | Unknown | 0 |
| blown_saves | uint8 | Blown save credited to the pitcher (1 or 0). | Unknown | Unknown | 0 |
| earned_runs | uint8 | Total earned runs allowed. | Unknown | Unknown | 1 |
| batters_faced | uint8 | Total batters faced (TBF). | Unknown | Unknown | 24 |
| outs | uint8 | Total outs recorded by the pitcher (div by 3 for IP). | Unknown | Unknown | 18 |
| games_pitched | uint8 | Indicates appearance as a pitcher. | Unknown | Unknown | 1 |
| complete_games | uint8 | Indicates if pitcher threw a complete game. | Unknown | Unknown | 0 |
| shutouts | uint8 | Indicates if pitcher threw a shutout. | Unknown | Unknown | 0 |
| pitches_thrown | uint16 | Total pitch count. | Unknown | Unknown | 98 |
| balls | uint16 | Total pitches called as balls. | Unknown | Unknown | 35 |
| strikes | uint16 | Total pitches called as strikes or hit. | Unknown | Unknown | 63 |
| hit_batsmen | uint8 | Redundant to hit_by_pitch; batters hit by pitch. | Unknown | Unknown | 0 |
| balks | uint8 | Total balks committed. | Unknown | Unknown | 0 |
| wild_pitches | uint8 | Total wild pitches thrown. | Unknown | Unknown | 1 |
| pickoffs | uint8 | Total runners picked off by pitcher. | Unknown | Unknown | 0 |
| rbi | uint8 | Runs batted in allowed by pitcher. | Unknown | Unknown | 1 |
| games_finished | uint8 | Indicates if pitcher finished the game. | Unknown | Unknown | 0 |
| inherited_runners | uint8 | Number of runners on base when entering game. | Unknown | Unknown | 0 |
| inherited_runners_scored | uint8 | Number of inherited runners that scored. | Unknown | Unknown | 0 |
| catchers_interference | uint8 | Times catcher interference happened while pitching. | Unknown | Unknown | 0 |
| sac_bunts | uint8 | Sacrifice bunts allowed. | Unknown | Unknown | 0 |
| sac_flies | uint8 | Sacrifice flies allowed. | Unknown | Unknown | 0 |
| passed_ball | uint8 | Passed balls allowed while pitching. | Unknown | Unknown | 0 |
| pop_outs | uint8 | Total pop outs induced. | Unknown | Unknown | 1 |
| line_outs | uint8 | Total line outs induced. | Unknown | Unknown | 2 |
#### 6. play_credits
**Table Summary:** A linking table assigning specific defensive or offensive credits (like putouts, assists) to individual players during a specific at-bat/play.
| Field Name | Data Type | Definition | Source Description | Source Column | Example Value |
|---|---|---|---|---|---|
| game_id | uint32 | Unique identifier for the game. | Unknown | Unknown | 715732 |
| at_bat_index | uint16 | Index of the at-bat where the credit occurred. | Unknown | Unknown | 45 |
| player | uint32 | Unique identifier for the credited player. | Unknown | Unknown | 608369 |
| credit | dict(uint8, string) | Type of credit assigned (e.g., 'assist', 'putout'). | Unknown | Unknown | "assist" |
#### 7. player_logs
**Table Summary:** Contains metadata about a player's roster status and game setup, such as their jersey number and place in the batting order.
| Field Name | Data Type | Definition | Source Description | Source Column | Example Value |
|---|---|---|---|---|---|
| game_id | uint32 | Unique identifier for the game. | Unknown | Unknown | 715732 |
| player | uint32 | Unique identifier for the player. | Unknown | Unknown | 605141 |
| jersey_number | string | The player's uniform number for the game. | Unknown | Unknown | "27" |
| position | dict(uint8, string) | Standard positional abbreviation (e.g., 'CF', 'P'). | Unknown | Unknown | "RF" |
| status_code | dict(uint8, string) | Player roster status (e.g., active, bench). | Unknown | Unknown | "A" |
| parent_team_id | uint32 | Unique identifier for the player's team. | Unknown | Unknown | 119 |
| batting_order | string | The player's assigned spot in the batting order. | Unknown | Unknown | "300" *(Often formatted as spot + substitution index)* |
#### 8. plays
**Table Summary:** Summarizes the final outcome and state of an at-bat, abstracting away the individual pitches into a single summarized event record.
| Field Name | Data Type | Definition | Source Description | Source Column | Example Value |
|---|---|---|---|---|---|
| game_id | uint32 | Unique identifier for the game. | Unknown | Unknown | 715732 |
| at_bat_index | uint16 | Sequential index of the at-bat. | Unknown | Unknown | 45 |
| event_type | dict(uint8, string) | Outcome classification (e.g., Home Run, Strikeout). | Unknown | Unknown | "single" |
| description | string | Text description of the play outcome. | Unknown | Unknown | "Player singles on a ground ball to center field." |
| rbi | uint8 | Runs batted in on the play. | Unknown | Unknown | 1 |
| away_score | uint8 | Away team score post-play. | Unknown | Unknown | 2 |
| home_score | uint8 | Home team score post-play. | Unknown | Unknown | 5 |
| is_out | boolean | True if the batter was put out. | Unknown | Unknown | false |
| half_inning | dict(uint8, string) | Top or Bottom of the inning. | Unknown | Unknown | "bottom" |
| inning | uint8 | Inning number (1-9+). | Unknown | Unknown | 6 |
| start_time | string | Timestamp of the first pitch in the at-bat. | Unknown | Unknown | "2026-05-28T19:02:00Z" |
| end_time | string | Timestamp of the play resolution. | Unknown | Unknown | "2026-05-28T19:05:15Z" |
| is_scoring_play | boolean | True if a run scored during this play. | Unknown | Unknown | true |
| has_review | boolean | True if the play was reviewed. | Unknown | Unknown | false |
| has_out | boolean | True if any out was recorded on the play. | Unknown | Unknown | false |
| balls | uint8 | Final ball count of the at-bat. | Unknown | Unknown | 3 |
| strikes | uint8 | Final strike count of the at-bat. | Unknown | Unknown | 2 |
| outs | uint8 | Out count post-play. | Unknown | Unknown | 1 |
| batter_id | uint32 | Unique ID of the batter. | Unknown | Unknown | 660271 |
| bat_side | dict(uint8, string) | Batter handedness. | Unknown | Unknown | "L" |
| pitcher_id | uint32 | Unique ID of the pitcher. | Unknown | Unknown | 608331 |
| pitch_hand | dict(uint8, string) | Pitcher handedness. | Unknown | Unknown | "R" |
| post_on_first | uint32 | ID of the runner on first after the play. | Unknown | Unknown | 660271 |
| post_on_second | uint32 | ID of the runner on second after the play. | Unknown | Unknown | Unknown |
| post_on_third | uint32 | ID of the runner on third after the play. | Unknown | Unknown | 641355 |
#### 9. position_logs
**Table Summary:** A simple tracking table documenting what defensive position(s) a player played during a game.
| Field Name | Data Type | Definition | Source Description | Source Column | Example Value |
|---|---|---|---|---|---|
| game_id | uint32 | Unique identifier for the game. | Unknown | Unknown | 715732 |
| player | uint32 | Unique identifier for the player. | Unknown | Unknown | 608369 |
| position | dict(uint8, string) | Position played (e.g., 'SS', '2B'). | Unknown | Unknown | "SS" |
#### 10. runner_credits
**Table Summary:** Assigns specific baserunning credits to players (e.g., getting a stolen base credit, or being caught stealing) linked to a specific moment in an at-bat.
| Field Name | Data Type | Definition | Source Description | Source Column | Example Value |
|---|---|---|---|---|---|
| game_id | uint32 | Unique identifier for the game. | Unknown | Unknown | 715732 |
| at_bat_index | uint16 | Index of the at-bat during which credit occurred. | Unknown | Unknown | 45 |
| runner_index | uint8 | Sequence index of the runner event. | Unknown | Unknown | 1 |
| player | uint32 | Unique ID of the runner receiving credit. | Unknown | Unknown | 641355 |
| position | dict(uint8, string) | Base position of the runner (e.g., '1B', '2B'). | Unknown | Unknown | "1B" |
| credit | dict(uint8, string) | Type of credit (e.g., 'stolen_base'). | Unknown | Unknown | "stolen_base" |
#### 11. runners
**Table Summary:** Details the micro-movements of baserunners during a play. It tracks where a runner started, where they ended up, why they moved, and if they scored or were thrown out.
| Field Name | Data Type | Definition | Source Description | Source Column | Example Value |
|---|---|---|---|---|---|
| game_id | uint32 | Unique identifier for the game. | Unknown | Unknown | 715732 |
| at_bat_index | uint16 | Index of the at-bat where movement occurred. | Unknown | Unknown | 45 |
| runner_index | uint8 | Sequential index of the runner's movement. | Unknown | Unknown | 1 |
| origin_base | dict(uint8, string) | The base the runner started on prior to the pitch. | Unknown | Unknown | "1B" |
| start | dict(uint8, string) | Base at the exact start of movement. | Unknown | Unknown | "1B" |
| end | dict(uint8, string) | The base the runner safely reached. | Unknown | Unknown | "3B" |
| out_base | dict(uint8, string) | The base the runner was trying to reach if thrown out. | Unknown | Unknown | Unknown |
| is_out | boolean | True if the runner was put out during advancement. | Unknown | Unknown | false |
| out_number | uint8 | If out, whether they were the 1st, 2nd, or 3rd out. | Unknown | Unknown | Unknown |
| event_type | dict(uint8, string) | The type of event that caused movement. | Unknown | Unknown | "single" |
| movement_reason | dict(uint8, string) | Why the runner advanced (e.g., 'hit', 'wild_pitch'). | Unknown | Unknown | "hit" |
| runner | uint32 | Unique identifier for the running player. | Unknown | Unknown | 641355 |
| responsible_pitcher | uint32 | Pitcher accountable for this runner if they score. | Unknown | Unknown | 608331 |
| is_scoring_event | boolean | True if this specific runner crossed home plate. | Unknown | Unknown | false |
| rbi | boolean | True if this runner scoring resulted in an RBI. | Unknown | Unknown | false |
| earned | boolean | True if the run (if scored) was earned. | Unknown | Unknown | false |
| team_unearned | boolean | True if run was team unearned but pitcher unearned. | Unknown | Unknown | false |
| play_index | uint8 | Micro-index ordering multiple runners on one play. | Unknown | Unknown | 1 |

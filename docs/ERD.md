wip generated ERD 

```
========================================================================================================
                                     CORE GAME METADATA & CONTEXT
========================================================================================================

    +----------------------------------+                   +----------------------------------+
    |              games               |                   |           player_logs            |
    +----------------------------------+                   +----------------------------------+
    | PK | game_id           (uint32)  |--+                | PK | game_id           (uint32)  |
    +----+-----------------------------+  |                | PK | player            (uint32)  |
    |    | game_type         (dict)    |  |                +----+-----------------------------+
    |    | season            (uint16)  |  |                |    | position          (dict)    |
    |    | date_time         (string)  |  |                |    | batting_order     (string)  |
    |    | away_team_id      (uint32)  |  |                +----------------------------------+
    |    | home_team_id      (uint32)  |  |                                 |
    +----------------------------------+  |                                 |
                                          |                                 |
==========================================|=================================|===========================
                                     PLAY-BY-PLAY GRANULARITY       |
==========================================|=================================|===========================
                                          |                                 |
                                          | (1:N)                           |
                                          v                                 |
    +----------------------------------+                                    |
    |              plays               |                                    |
    +----------------------------------+                                    |
    | PK | game_id           (uint32)  |<-----------------------------+     |
    | PK | at_bat_index      (uint16)  |--+                           |     |
    +----+-----------------------------+  |                           |     |
    | FK | batter_id         (uint32)  |  | (1:N)                     |     |
    | FK | pitcher_id        (uint32)  |  |                           |     |
    |    | event_type        (dict)    |  |                           |     |
    |    | description       (string)  |  |                           |     |
    +----------------------------------+  |                           |     |
                                          |                           |     |
            +-----------------------------+                           |     |
            |                                                         |     |
            | (1:N)                                                   |     |
            v                                                         |     |
    +----------------------------------+     +------------------------|-----|-----------------+
    |              events              |     |      play_credits      |     |                 |
    +----------------------------------+     +------------------------|-----|-----------------+
    | PK | game_id           (uint32)  |     | PK | game_id           |(u32)|                 |
    | PK | at_bat_index      (uint16)  |     | PK | at_bat_index      |(u16)|                 |
    | PK | event_index       (uint8)   |     | PK | player            |(u32)|<----------------+
    +----+-----------------------------+     +----+-------------------------+                 |
    |    | description       (string)  |     |    | credit            (dict) |                 |
    |    | pitch_type        (dict)    |     +------------------------------+                 |
    |    | launch_speed      (float32) |                                                      |
    |    | launch_angle      (float32) |                                                      |
    +----------------------------------+                                                      |
                                                                                              |
            +---------------------------------------------------------+                       |
            |                                                         |                       |
            | (1:N)                                                   |                       |
            v                                                         |                       |
    +----------------------------------+     +------------------------|-----|-----------------+
    |             runners              |     |     runner_credits     |     |                 |
    +----------------------------------+     +------------------------|-----|-----------------+
    | PK | game_id           (uint32)  |     | PK | game_id           |(u32)|                 |
    | PK | at_bat_index      (uint16)  |--+  | PK | at_bat_index      |(u16)|                 |
    | PK | runner_index      (uint8)   |  |  | PK | runner_index      (uint8)|                |
    +----+-----------------------------+  |  | PK | player            |(u32)|<---------------+
    |    | origin_base       (dict)    |  |  +----+-------------------------+
    |    | start             (dict)    |  +->|    | credit            (dict) |
    |    | end               (dict)    | (1:N)+------------------------------+
    +----------------------------------+

========================================================================================================
                                   CUMULATIVE PERFORMANCE LOGS (BOX SCORES)
========================================================================================================

    +----------------------------------+     +----------------------------------+
    |           batting_logs           |     |          pitching_logs           |
    +----------------------------------+     +----------------------------------+
    | PK | game_id           (uint32)  |     | PK | game_id           (uint32)  |
    | PK | player            (uint32)  |     | PK | player            (uint32)  |
    +----+-----------------------------+     +----+-----------------------------+
    |    | hits              (uint8)   |     |    | pitches_thrown    (uint16)  |
    |    | plate_appearances (uint8)   |     |    | strike_outs       (uint8)   |
    +----------------------------------+     +----------------------------------+

    +----------------------------------+     +----------------------------------+
    |          fielding_logs           |     |          position_logs           |
    +----------------------------------+     +----------------------------------+
    | PK | game_id           (uint32)  |     | PK | game_id           (uint32)  |
    | PK | player            (uint32)  |     | PK | player            (uint32)  |
    +----+-----------------------------+     +----+-----------------------------+
    |    | put_outs          (uint8)   |     |    | position          (dict)    |
    |    | errors            (uint8)   |     +----------------------------------+
    +----------------------------------+

```
### Architectural Notes for Engineers
 * **Primary Key Invariance:** game_id cascading acts as the foundational partition or clustering key if deploying on columnar or distributed datastores (e.g., DuckDB, ClickHouse, Postgres).
 * **The _credits Optimization:** play_credits and runner_credits map player identities (player) asynchronously to high-resolution data streams without forcing massive null-filled wide columns on plays or runners.
 * **State Mapping:** The runners table maintains structural state changes. The composite array configuration of origin_base, start, and end allows for easy reconstruction of a baseball diamond base-map vector at any point in time.

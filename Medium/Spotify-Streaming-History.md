# [Spotify Streaming History](https://datalemur.com/questions/spotify-streaming-history)

## Problem Statement

You're given two tables containing data on Spotify users' streaming activity:
- `songs_history` - which has historical streaming data
- `songs_weekly` - which has data from the current week

Write a query that outputs the user ID, song ID, and cumulative count of song plays up to August 4th, 2022, sorted in descending order.

Assume that there may be new users or songs in the `songs_weekly` table that are not present in the `songs_history` table.

**Definitions:**
- `song_weekly` table only contains data for the week of August 1st to August 7th, 2022.
- `songs_history` table contains data up to July 31st, 2022. The query should include historical data from this table.

## Tables

### `songs_history` Table:

| Column Name | Type | Description |
|---|---|---|
| history_id | integer | |
| user_id | integer | |
| song_id | integer | |
| song_plays | integer | |

### `songs_weekly` Table:

| Column Name | Type | Description |
|---|---|---|
| user_id | integer | |
| song_id | integer | |
| listen_time | datetime | |

## Example Input

**songs_history Table:**

| history_id | user_id | song_id | song_plays |
|---|---|---|---|
| 10011 | 777 | 1238 | 11 |
| 12452 | 695 | 4520 | 1 |

**songs_weekly Table:**

| user_id | song_id | listen_time |
|---|---|---|
| 777 | 1238 | 08/01/2022 12:00:00 |
| 695 | 4520 | 08/04/2022 08:00:00 |
| 125 | 9630 | 08/04/2022 16:00:00 |

## Example Output

| user_id | song_id | song_plays |
|---|---|---|
| 777 | 1238 | 12 |
| 695 | 4520 | 2 |
| 125 | 9630 | 1 |

## Solution

```sql
WITH history AS (
  SELECT 
    user_id,
    song_id,
    song_plays
  FROM songs_history
  
  UNION ALL
  
  SELECT 
    user_id,
    song_id,
    COUNT(*) AS song_plays
  FROM songs_weekly
  WHERE listen_time <= '2022-08-04 23:59:59'
  GROUP BY user_id, song_id
)
SELECT 
  user_id,
  song_id,
  SUM(song_plays) AS song_plays
FROM history
GROUP BY user_id, song_id
ORDER BY song_plays DESC;
```

## Explanation

### Step 1: Extract the count of song plays from the weekly table

The weekly table contains streaming data from August 1st to August 7th, 2022. To get the data up to August 4th, 2022, we filter to the specified date.

```sql
SELECT 
  user_id,
  song_id,
  COUNT(*) AS song_plays
FROM songs_weekly
WHERE listen_time <= '2022-08-04 23:59:59'
GROUP BY user_id, song_id;
```

**Note:** In the following step, the SELECT fields should be arranged in the order `user_id`, `song_id`, and `song_plays`.

### Step 2: Combine the weekly and historical streaming data

We combine the output with the `songs_history` table using a `UNION ALL` clause.

For the `UNION ALL` to work, the number and order of the fields in the SELECT statement for both queries should be the same, and the data types should be compatible.

### Step 3: Find the cumulative count of song plays

Now that we have a table containing the historical streaming data up to August 4th, 2022, we need to find the cumulative count of song plays and order them in descending order.

# App Click-through Rate (CTR)

**Company**: Facebook

**Difficulty**: Easy

**Source**: [DataLemur](https://datalemur.com/questions/click-through-rate)

---

## Problem Statement

Assume you have an events table on Facebook app analytics. Write a query to calculate the click-through rate (CTR) for the app in 2022 and round the results to 2 decimal places.

**Definition and note:**
- Percentage of click-through rate (CTR) = 100.0 * Number of clicks / Number of impressions
- To avoid integer division, multiply the CTR by 100.0, not 100.

## Table Schema

### `events` Table:

| Column Name | Type |
|-------------|----------|
| app_id | integer |
| event_type | string |
| timestamp | datetime |

### `events` Example Input:

| app_id | event_type | timestamp |
|--------|------------|---------------------|
| 123 | impression | 07/18/2022 11:36:12 |
| 123 | impression | 07/18/2022 11:37:12 |
| 123 | click | 07/18/2022 11:37:42 |
| 234 | impression | 07/18/2022 14:15:12 |
| 234 | click | 07/18/2022 14:16:12 |

### Example Output:

| app_id | ctr |
|--------|--------|
| 123 | 50.00 |
| 234 | 100.00 |

### Explanation:

App 123 has a CTR of 50.00% because out of 2 impressions, it got 1 click. The calculation is: (1 / 2) * 100.0 = 50.00%.

---

## Solution

```sql
WITH impressions_and_clicks AS (
  SELECT 
    app_id,
    SUM(
      CASE 
        WHEN event_type = 'impression' THEN 1 ELSE 0
      END
    ) AS impressions,
    SUM(
      CASE 
        WHEN event_type = 'click' THEN 1 ELSE 0
      END
    ) AS clicks
  FROM events
  WHERE EXTRACT(YEAR FROM timestamp) = 2022
  GROUP BY app_id
)
SELECT 
  app_id,
  ROUND(((100.0 * clicks) / impressions), 2) ctr
FROM impressions_and_clicks;
```

### Explanation:

1. **Step 1: Filter for 2022 events**
   - Use `WHERE EXTRACT(YEAR FROM timestamp) = 2022` to get only 2022 data

2. **Step 2: Count clicks and impressions**
   - Use CASE statements with SUM to count each event type
   - Group by app_id to get counts per app

3. **Step 3: Calculate CTR**
   - Divide clicks by impressions and multiply by 100.0
   - Use ROUND() to get 2 decimal places

### Alternative Solution using COUNT(CASE...)

```sql
SELECT 
  app_id,
  ROUND((100.0 * 
    COUNT(CASE WHEN event_type = 'click' THEN 1 ELSE NULL END) / 
    COUNT(CASE WHEN event_type = 'impression' THEN 1 ELSE NULL END)), 2) AS ctr_rate
FROM events
WHERE timestamp >= '2022-01-01' AND timestamp < '2023-01-01'
GROUP BY app_id;
```

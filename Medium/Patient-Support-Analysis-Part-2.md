# Patient Support Analysis (Part 2)

**Company:** UnitedHealth Group
**Difficulty:** Medium
**SQL Dialect:** PostgreSQL 14

## Problem Statement

UnitedHealth Group (UHG) has a program called Advocate4Me, which allows policy holders (or, members) to call an advocate and receive support for their health care needs – whether that's claims and benefits support, drug coverage, pre- and post-authorisation, medical records, emergency assistance, or member portal services.

Calls to the Advocate4Me call centre are classified into various categories, but some calls cannot be neatly categorised. These uncategorised calls are labeled as "n/a", or are left empty when the support agent does not enter anything into the call category field.

Write a query to calculate the percentage of calls that cannot be categorised. Round your answer to 1 decimal place. For example, 45.0, 48.5, 57.7.

## Tables

### `callers` Schema:

| Column Name | Type | Description |
|-------------|------|-------------|
| policy_holder_id | integer | Policy holder ID |
| case_id | varchar | Case ID |
| call_category | varchar | Call category |
| call_date | timestamp | Call date |
| call_duration_secs | integer | Call duration in seconds |

### `callers` Example Input:

| policy_holder_id | case_id | call_category | call_date | call_duration_secs |
|------------------|---------|---------------|-----------|--------------------|
| 1 | f1d012f9-9d02-4966-a968-bf6c5bc9a9fe | emergency assistance | 2023-04-13T19:16:53Z | 144 |
| 1 | 41ce8fb6-1ddd-4f50-ac31-07bfcce6aaab | authorisation | 2023-05-25T09:30Z | 815 |
| 2 | 9b1af84b-eedb-4c21-9730-6f099cc2c65e | n/a | 2023-01-26T01:21:27Z | 992 |
| 2 | 8471a3d4-6fc7-4bb2-9fc7-4503e3638a9e | emergency assistance | 2023-03-09T10:58:54Z | 128 |
| 2 | 38208fae-bad0-49bf-99aa-7842ba2e37bc | benefits | 2023-06-05T07:35:43Z | 619 |

## Example Output:

| uncategorised_call_pct |
|------------------------|
| 20.0 |

**Explanation:** Out of the total of 5 calls registered, one call was not categorised. Therefore, the percentage of uncategorised calls is calculated as 20.0% (1 out of 5 multiplied by 100 and rounded to one decimal place).

## Solution

```sql
WITH uncategorised_callers AS (
  SELECT COUNT(case_id) AS uncategorised_calls
  FROM callers
  WHERE call_category IS NULL
    OR call_category = 'n/a')

SELECT
  ROUND(
    100.0 * uncategorised_calls / (SELECT COUNT(*) FROM callers), 1) AS uncategorised_call_pct
FROM uncategorised_callers;
```

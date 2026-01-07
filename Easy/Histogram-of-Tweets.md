# Histogram of Tweets

**Company:** Twitter  
**Difficulty:** Easy  
**Category:** SQL  
**Date Solved:** January 7, 2026

## Problem Description

Assume you're given a table Twitter tweet data, write a query to obtain a histogram of tweets posted per user in 2022. Output the tweet count per user as the bucket and the number of Twitter users who fall into that bucket.

In other words, group the users by the number of tweets they posted in 2022 and count the number of users in each group.

### tweets Table:

| Column Name | Type |
|-------------|------|
| tweet_id | integer |
| user_id | integer |
| msg | string |
| tweet_date | timestamp |

### Example Input:

| tweet_id | user_id | msg | tweet_date |
|----------|---------|-----|------------|
| 214252 | 111 | Am considering taking Tesla private at $420. Funding secured. | 12/30/2021 00:00:00 |
| 739252 | 111 | Despite the constant negative press covfefe | 01/01/2022 00:00:00 |
| 846402 | 111 | Following @NickSinghTech on Twitter changed my life! | 02/14/2022 00:00:00 |
| 241425 | 254 | If the salary is so competitive why won't you tell me what it is? | 03/01/2022 00:00:00 |
| 231574 | 148 | I no longer have a manager. I can't be managed | 03/23/2022 00:00:00 |

### Example Output:

| tweet_bucket | users_num |
|--------------|----------|
| 1 | 2 |
| 2 | 1 |

### Explanation:

Based on the example output, there are two users who posted only one tweet in 2022, and one user who posted two tweets in 2022. The query groups the users by the number of tweets they posted and displays the number of users in each group.

## Solution

```sql
WITH tweets_per_user AS (
  SELECT
    user_id,
    COUNT(msg) AS msg_count
  FROM tweets
  WHERE tweet_date BETWEEN "2022-01-01" AND "2022-12-31"
  GROUP BY user_id
)
SELECT
  msg_count,
  COUNT(msg_count) AS users_num
FROM tweets_per_user
GROUP BY msg_count;
```

## Approach

1. Use a CTE (Common Table Expression) called `tweets_per_user` to first calculate the number of tweets per user in 2022
2. Filter tweets by date range (2022-01-01 to 2022-12-31)
3. Group by user_id and count the messages for each user
4. In the main query, group by the message count and count how many users fall into each bucket
5. This creates a histogram showing the distribution of tweet counts across users

## Link

[DataLemur - Histogram of Tweets](https://datalemur.com/questions/sql-histogram-tweets)

# Second Day Confirmation

**Company**: TikTok

**Difficulty**: Easy

**Source**: [DataLemur](https://datalemur.com/questions/second-day-confirmation)

---

## Problem Statement

Assume you're given tables with information about TikTok user sign-ups and confirmations through email and text. New users on TikTok sign up using their email addresses, and upon sign-up, each user receives a text message confirmation to activate their account.

Write a query to display the user IDs of those who did not confirm their sign-up on the first day, but confirmed on the second day.

**Definition:**
- `action_date` refers to the date when users activated their accounts and confirmed their sign-up through text messages.

## Table Schema

### `emails` Table:

| Column Name | Type |
|-------------|----------|
| email_id | integer |
| user_id | integer |
| signup_date | datetime |

### `emails` Example Input:

| email_id | user_id | signup_date |
|----------|---------|---------------------|
| 125 | 7771 | 06/14/2022 00:00:00 |
| 433 | 1052 | 07/09/2022 00:00:00 |

### `texts` Table:

| Column Name | Type |
|-------------|----------|
| text_id | integer |
| email_id | integer |
| signup_action | string ('Confirmed', 'Not confirmed') |
| action_date | datetime |

### `texts` Example Input:

| text_id | email_id | signup_action | action_date |
|---------|----------|---------------|---------------------|
| 6878 | 125 | Confirmed | 06/14/2022 00:00:00 |
| 6997 | 433 | Not Confirmed | 07/09/2022 00:00:00 |
| 7000 | 433 | Confirmed | 07/10/2022 00:00:00 |

### Example Output:

| user_id |
|---------|
| 1052 |

### Explanation:

Only User 1052 confirmed their sign-up on the second day. User 7771 confirmed on the same day they signed up, so they are not included.

---

## Solution

```sql
SELECT user_id
FROM emails
JOIN texts
  ON emails.email_id = texts.email_id
WHERE DATEDIFF(action_date, signup_date) = 1
  AND signup_action = 'Confirmed';
```

### Explanation:

1. **JOIN the tables**
   - Connect emails and texts tables on email_id to link user sign-ups with their confirmations

2. **Filter for second day confirmations**
   - Use `DATEDIFF(action_date, signup_date) = 1` to find users who confirmed exactly 1 day after signup
   - MySQL's DATEDIFF returns the difference in days between two dates

3. **Filter for confirmed actions**
   - Add `signup_action = 'Confirmed'` to ensure we only get actual confirmations

### PostgreSQL Alternative:

```sql
SELECT user_id
FROM emails
JOIN texts
  ON emails.email_id = texts.email_id
WHERE action_date::DATE - signup_date::DATE = 1
  AND signup_action = 'Confirmed';
```

### Using EXTRACT for date comparison:

```sql
SELECT user_id
FROM emails
INNER JOIN texts
  ON emails.email_id = texts.email_id
WHERE EXTRACT(DAY FROM (action_date - signup_date)) = 1
  AND signup_action = 'Confirmed';
```

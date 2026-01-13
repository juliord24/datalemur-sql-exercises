# [Signup Activation Rate](https://datalemur.com/questions/signup-confirmation-rate)

## Problem Statement

New TikTok users sign up with their emails. They confirmed their signup by replying to the text confirmation to activate their accounts. Users may receive multiple text messages for account confirmation until they have confirmed their new account.

A senior analyst is interested to know the activation rate of specified users in the `emails` table. Write a query to find the activation rate. Round the percentage to 2 decimal places.

**Definitions:**
- `emails` table contain the information of user signup details.
- `texts` table contains the users' activation information.

**Assumptions:**
- The analyst is interested in the activation rate of specific users in the `emails` table, which may not include all users that could potentially be found in the `texts` table.
- For example, user 123 in the `emails` table may not be in the `texts` table and vice versa.

## Tables

### `emails` Table:

| Column Name | Type | Description |
|---|---|---|
| email_id | integer | |
| user_id | integer | |
| signup_date | datetime | |

### `texts` Table:

| Column Name | Type | Description |
|---|---|---|
| text_id | integer | |
| email_id | integer | |
| signup_action | string | |

## Example Input

**emails Table:**

| email_id | user_id | signup_date |
|---|---|---|
| 125 | 7771 | 06/14/2022 00:00:00 |
| 236 | 6950 | 07/01/2022 00:00:00 |
| 433 | 1052 | 07/09/2022 00:00:00 |

**texts Table:**

| text_id | email_id | signup_action |
|---|---|---|
| 6878 | 125 | Confirmed |
| 6920 | 236 | Not Confirmed |
| 6994 | 236 | Confirmed |

## Example Output

| activation_rate |
|---|
| 0.67 |

## Solution

```sql
SELECT 
  ROUND(
    COUNT(texts.email_id)::DECIMAL
    / COUNT(DISTINCT emails.email_id), 2
  ) AS activation_rate
FROM emails
LEFT JOIN texts
  ON emails.email_id = texts.email_id
  AND texts.signup_action = 'Confirmed';
```

## Explanation

### Step 1: Join tables for records with successful confirmations

To start, we use a `LEFT JOIN` to connect the `emails` and `texts` tables. It's important to note that an `INNER JOIN` won't work for this question.

We keep only the users who successfully signed up and received a 'Confirmed' text confirmation in the `signup_action` column located in the `texts` table.

**Why LEFT JOIN is Necessary in this Query?**

Not every `email_id` in the `emails` table will have a matching value in the `texts` table, and this is where the `LEFT JOIN` comes into play.

When we perform a `LEFT JOIN`, all the rows from the left table (`emails`) are returned along with matching rows from the right table (`texts`). If there is no match for a particular row in the right table, then the columns from the right table will be NULL.

If we were to use an `INNER JOIN`, only the matching rows between the two tables would be returned, effectively filtering out any `email_id` values from the `emails` table that do not have a corresponding match in the `texts` table. This could result in relevant users being excluded from the calculation completely.

### Step 2: Calculate the signup activation rate

**Signup Activation Rate = Number of users who confirmed their accounts / Number of users in the emails table**

To avoid getting an activation rate of '0' (which happens when dividing an integer by another integer), we cast either the denominator or the numerator to DECIMAL type.

Finally, the `ROUND()` function is used to truncate the result to 2 decimal places, as specified in the instructions.

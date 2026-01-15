# FAANG Stock Min-Max (Part 1)

**Company:** Bloomberg  
**Difficulty:** Medium  
**SQL Dialect:** PostgreSQL 14

## Problem Statement

The Bloomberg terminal is the go-to resource for financial professionals. As a Data Analyst at Bloomberg, you have access to historical data on stock performance.

You're analyzing the highest and lowest open prices for each FAANG stock by month over the years.

For each FAANG stock, display the ticker symbol, the month and year ('Mon-YYYY') with the corresponding highest and lowest open prices.

## Tables

### `stock_prices` Schema:

| Column Name | Type | Description |
|-------------|------|-------------|
| date | datetime | The specified date |
| ticker | varchar | The stock ticker symbol |
| open | decimal | The opening price |
| high | decimal | The highest price |
| low | decimal | The lowest price |
| close | decimal | The closing price |

## Solution

```sql
WITH highest AS(
  SELECT
    ticker,
    TO_CHAR(date, 'FMMon-YYYY') highest_mth,
    MAX(open) highest_open,
    RANK() OVER(PARTITION BY ticker ORDER BY open DESC) row_num
  FROM stock_prices
  GROUP BY ticker, TO_CHAR(date, 'FMMon-YYYY'), open
),
lowest AS (
  SELECT
    ticker,
    TO_CHAR(date, 'FMMon-YYYY') lowest_mth,
    MIN(open) lowest_open,
    RANK() OVER(PARTITION BY ticker ORDER BY open) row_num
  FROM stock_prices
  GROUP BY ticker, TO_CHAR(date, 'FMMon-YYYY'), open
)
SELECT
  highest.ticker,
  highest.highest_mth,
  highest.highest_open,
  lowest.lowest_mth,
  lowest.lowest_open
FROM highest
INNER JOIN lowest
  ON highest.ticker = lowest.ticker
  AND highest.row_num = 1
  AND lowest.row_num = 1
ORDER BY highest.ticker;
```

# Supercloud Customer

**Company:** Microsoft  
**Difficulty:** Medium  
**SQL Dialect:** PostgreSQL 14

## Problem Statement

A Microsoft Azure Supercloud customer is defined as a customer who has purchased at least one product from every product category listed in the `products` table.

Write a query that identifies the customer IDs of these Supercloud customers.

## Tables

### `customer_contracts` Table:

| Column Name | Type |
|-------------|--------|
| customer_id | integer |
| product_id | integer |
| amount | integer |

### `customer_contracts` Example Input:

| customer_id | product_id | amount |
|-------------|------------|--------|
| 1 | 1 | 1000 |
| 1 | 3 | 2000 |
| 1 | 5 | 1500 |
| 2 | 2 | 3000 |
| 2 | 6 | 2000 |

### `products` Table:

| Column Name | Type |
|----------------|--------|
| product_id | integer |
| product_category | string |
| product_name | string |

### `products` Example Input:

| product_id | product_category | product_name |
|------------|------------------|-------------------------|
| 1 | Analytics | Azure Databricks |
| 2 | Analytics | Azure Stream Analytics |
| 4 | Containers | Azure Kubernetes Service |
| 5 | Containers | Azure Service Fabric |
| 6 | Compute | Virtual Machines |
| 7 | Compute | Azure Functions |

## Example Output:

| customer_id |
|-------------|
| 1 |

## Explanation:

Customer 1 bought from Analytics, Containers, and Compute categories of Azure, and thus is a Supercloud customer. Customer 2 isn't a Supercloud customer, since they don't buy any container services from Azure.

## Solution

```sql
WITH customers_category AS (
  SELECT
    customer_id,
    product_category
  FROM customer_contracts c
  JOIN products p
    ON c.product_id = p.product_id
  GROUP BY customer_id, product_category
  ORDER BY customer_id
)
SELECT customer_id
FROM customers_category
GROUP BY customer_id
HAVING COUNT(customer_id) >= (
  SELECT COUNT(DISTINCT product_category)
  FROM products
);
```

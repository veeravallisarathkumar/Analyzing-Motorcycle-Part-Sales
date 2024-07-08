# README

## Overview

This Jupyter Notebook is designed to analyze sales data from a company that has provided access to their database. The analysis focuses on understanding the net revenue from wholesale orders, categorized by product line, month, and warehouse.

## File Contents

The notebook contains the following sections:

1. **Introduction and Data Description**:
    - Describes the sales data, including the table structure and columns.
    
2. **SQL Query**:
    - Contains an SQL query to extract the required information.
    
3. **Query Execution**:
    - Executes the SQL query and displays the results.

## Data Description

The `sales` table in the database contains the following columns:

| Column       | Data type | Description                                          |
|--------------|-----------|------------------------------------------------------|
| `order_number` | `VARCHAR` | Unique order number.                                 |
| `date`         | `DATE`    | Date of the order, from June to August 2021.         |
| `warehouse`    | `VARCHAR` | The warehouse that the order was made from; `North`, `Central`, or `West`. |
| `client_type`  | `VARCHAR` | Whether the order was `Retail` or `Wholesale`.      |
| `product_line` | `VARCHAR` | Type of product ordered.                            |
| `quantity`     | `INT`     | Number of products ordered.                         |
| `unit_price`   | `FLOAT`   | Price per product (dollars).                        |
| `total`        | `FLOAT`   | Total price of the order (dollars).                 |
| `payment`      | `VARCHAR` | Payment method; `Credit card`, `Transfer`, or `Cash`. |
| `payment_fee`  | `FLOAT`   | Percentage of `total` charged as a result of the `payment` method. |

## SQL Query

The notebook includes an SQL query to calculate the net revenue for each product line, month, and warehouse. The query filters the data to include only wholesale orders. Here is the SQL query:

```sql
SELECT product_line,
    CASE WHEN EXTRACT('month' from date) = 6 THEN 'June'
        WHEN EXTRACT('month' from date) = 7 THEN 'July'
        WHEN EXTRACT('month' from date) = 8 THEN 'August'
    END as month,
    warehouse,
    SUM(total) - SUM(payment_fee) AS net_revenue
FROM sales
WHERE client_type = 'Wholesale'
GROUP BY product_line, warehouse, month
ORDER BY product_line, month, net_revenue DESC

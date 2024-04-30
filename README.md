# Data Driven Businesses with SQL & Tableau
## Case Study: Help an e-commerce company make a critical business decision by exploring data in SQL and visualizing it interactively in a Tableau dashboard.
## Tasks
  1. use SQL queries
  2. prepare data visualisations in Tableau
  3. identify, analyse, interpret and present relevant data
  4. prepare conclusions and recommendations

## Examples
### Schema

<img src="https://raw.githubusercontent.com/Markus-DS-29/Data-Driven-Businesses-with-SQL-Tableau/main/schema case study.png" alt="Schema of Case Study" style="width: 400px; height: auto;">

### Example SQL query

Usage of a SQL query to analyze orders of products in the "computers_accessories" category, categorizing them by weight intervals, and counting the number of delayed orders within each interval for each month and year.

```
SELECT 
    YEAR(order_purchase_timestamp), 
    MONTH(order_purchase_timestamp),
    CASE
        WHEN product_weight_g BETWEEN 0 AND 100 THEN '[1: 0g-100g['
        WHEN product_weight_g BETWEEN 100 AND 500 THEN '[2: 100g-500g['
        WHEN product_weight_g BETWEEN 500 AND 1000 THEN '[3: 500g-1000g['
        WHEN product_weight_g BETWEEN 1000 AND 2000 THEN '[4: 1.000g-2.000g['
        WHEN product_weight_g BETWEEN 2000 AND 10000 THEN '[5: 2.000g-10.000g['
        WHEN product_weight_g BETWEEN 10000 AND 20000 THEN '[6: 10.000g-20.000g['
        WHEN product_weight_g BETWEEN 20000 AND 50000 THEN '[7: 20.000g-50.000g['
    END AS interval_group,
    COUNT(order_id) AS "number of delayed orders"
FROM 
    orders 
    JOIN order_items USING(order_id)
    JOIN products USING(product_id)
    LEFT JOIN product_category_name_translation USING(product_category_name)
WHERE 	
    product_category_name_english = "computers_accessories"
    AND
    TIMESTAMPDIFF(second, order_purchase_timestamp, order_delivered_customer_date)/3600 
    >
    TIMESTAMPDIFF(second, order_purchase_timestamp, order_estimated_delivery_date)/3600 
    AND
    order_status = 'delivered'  
    AND order_approved_at IS NOT NULL
GROUP BY 
    YEAR(order_purchase_timestamp), 
    MONTH(order_purchase_timestamp), 
    interval_group
ORDER BY 
    YEAR(order_purchase_timestamp), 
    MONTH(order_purchase_timestamp), 
    interval_group;
```

### Examples Tableau
  1. Delayed Orders

<img src="https://raw.githubusercontent.com/Markus-DS-29/Data-Driven-Businesses-with-SQL-Tableau/main/visual delayed orders.png" alt="Example Visualisation: Delayed Orders" style="width: 400px; height: auto;">

  2. Regional Distribution of Reviews

<img src="https://raw.githubusercontent.com/Markus-DS-29/Data-Driven-Businesses-with-SQL-Tableau/main/regional distribution of reviews.png" alt="Example Visualisation: Regional Distribution of Reviews" style="width: 400px; height: auto;">




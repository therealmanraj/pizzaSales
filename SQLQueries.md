# Pizza Sales Data Analysis SQL Queries

## KPIs

1. **Total Revenue**
    ```sql
    SELECT SUM(total_price) AS Total_Revenue FROM pizza_sales;
    ```

2. **Average Order Value**
    ```sql
    SELECT (SUM(total_price) / COUNT(DISTINCT order_id)) AS Avg_order_Value FROM pizza_sales;
    ```

3. **Total Pizzas Sold**
    ```sql
    SELECT SUM(quantity) AS Total_pizza_sold FROM pizza_sales;
    ```

4. **Total Orders**
    ```sql
    SELECT COUNT(DISTINCT order_id) AS Total_Orders FROM pizza_sales;
    ```

5. **Average Pizzas Per Order**
    ```sql
    SELECT CAST(CAST(SUM(quantity) AS DECIMAL(10,2)) / 
    CAST(COUNT(DISTINCT order_id) AS DECIMAL(10,2)) AS DECIMAL(10,2))
    AS Avg_Pizzas_per_order
    FROM pizza_sales;
    ```

## Hourly Trend for Total Pizzas Sold

```sql
SELECT HOUR(order_time) as order_hours, 
       SUM(quantity) as total_pizzas_sold
FROM pizza_sales
GROUP BY order_hours
ORDER BY order_hours;
```

## Weekly Trend for Orders

```sql
SELECT 
    WEEK(STR_TO_DATE(order_date, '%d-%m-%Y'), 3) AS WeekNumber,
    YEAR(STR_TO_DATE(order_date, '%d-%m-%Y')) AS Year,
    COUNT(DISTINCT order_id) AS Total_orders
FROM 
    pizza_sales
GROUP BY 
    WeekNumber,
    Year
ORDER BY 
    Year, WeekNumber;
```

## % of Sales by Pizza Category

```sql
SELECT pizza_category, 
       CAST(SUM(total_price) AS DECIMAL(10,2)) as total_revenue,
       CAST(SUM(total_price) * 100 / (SELECT SUM(total_price) from pizza_sales) AS DECIMAL(10,2)) AS PCT
FROM pizza_sales
GROUP BY pizza_category;
```

## % of Sales by Pizza Size

```sql
SELECT pizza_size, 
       CAST(SUM(total_price) AS DECIMAL(10,2)) as total_revenue,
       CAST(SUM(total_price) * 100 / (SELECT SUM(total_price) from pizza_sales) AS DECIMAL(10,2)) AS PCT
FROM pizza_sales
GROUP BY pizza_size
ORDER BY pizza_size;
```

## Total Pizzas Sold by Pizza Category

```sql
SELECT pizza_category, SUM(quantity) as Total_Quantity_Sold
FROM pizza_sales
GROUP BY pizza_category
ORDER BY Total_Quantity_Sold DESC;
```

## Top 5 Pizzas by Revenue

```sql
SELECT pizza_name, SUM(total_price) AS Total_Revenue
FROM pizza_sales
GROUP BY pizza_name
ORDER BY Total_Revenue DESC
LIMIT 5;
```

## Bottom 5 Pizzas by Revenue

```sql
SELECT pizza_name, SUM(total_price) AS Total_Revenue
FROM pizza_sales
GROUP BY pizza_name
ORDER BY Total_Revenue ASC
LIMIT 5;
```

## Top 5 Pizzas by Quantity

```sql
SELECT pizza_name, SUM(quantity) AS Total_Pizza_Sold
FROM pizza_sales
GROUP BY pizza_name
ORDER BY Total_Pizza_Sold DESC
LIMIT 5;
```

## Bottom 5 Pizzas by Quantity

```sql
SELECT pizza_name, SUM(quantity) AS Total_Pizza_Sold
FROM pizza_sales
GROUP BY pizza_name
ORDER BY Total_Pizza_Sold ASC
LIMIT 5;
```

## Top 5 Pizzas by Total Orders

```sql
SELECT pizza_name, COUNT(DISTINCT order_id) AS Total_Orders
FROM pizza_sales
GROUP BY pizza_name
ORDER BY Total_Orders DESC
LIMIT 5;
```

## Bottom 5 Pizzas by Total Orders

```sql
SELECT pizza_name, COUNT(DISTINCT order_id) AS Total_Orders
FROM pizza_sales
GROUP BY pizza_name
ORDER BY Total_Orders ASC
LIMIT 5;
```

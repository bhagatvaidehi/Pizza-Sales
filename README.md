# Pizza-Sales Analysis
Pizza Data Insights : Pizza Sales Analytics

## Table of Contents

- [Project Overview](#project-overview)
- [Data Source](#data-source)
- [Tools](#tools)
- [Data Cleaning/Preparation](#data-cleaning)
- [Problem Statement](#problem-statement)
- [Data Analysis](#data-analysis)
- [Results](#results)
- [Recommendation](#recommendation)

## Project Overview
This project revolves around the thorough analysis and visualization of key performance indicators (KPIs) derived from pizza sales data to gain valuable insights into
business performance. By examining metrics such as total revenue, average order value, total pizzas sold, total orders, and average pizzas per order, the project aims 
to comprehensively evaluate sales performance. Through the visualization of daily and monthly order trends, peak hours of activity can be identified to optimize resource 
allocation. Additionally, analyzing sales by pizza category and size allows for a better understanding of customer preferences. By identifying top-selling and 
underperforming pizza options, informed decisions can be made to drive sales and improve overall business outcomes. Ultimately, the project seeks to uncover actionable
insights that propel business growth and success.

### Data Source

Sales Data : The primary dataset used for this analysis is the "pizza_sales.csv" file.

### Tools

- Excel - Data Cleaning
- SQL Server - Data Analysis
- PowerBI - Creating Report

 ### Data Cleaning

 In the initial data prepation phase, I have perfromed the following tasks:
 1. Data loading and inspection.
 2. Handling missing values.
 3. Data Cleaning and formatting.

### Problem Statement 

#### KPI's REQUIREMENTS:

1. Total Revenue: The sum of the total price of all pizza orders.

2. Average Order Value: The average amount spent per order, calculated by dividing the total revenue by the total number of orders.

3. Total Pizzas Sold: The sum of the quantities of all pizzas sold.

4. Total Orders: The total number of orders placed.

5. Average Pizzas Per Order: The average number of pizzas sold per order, calculated by dividing the total number of pizzas sold by the total number of orders.

#### CHARTS REQUIREMENTS:

1. Daily Trend for Total Orders: A bar chart that displays the daily trend of total orders over a specific time period.

2. Monthly Trend for Total Orders: A line chart that illustrates the hourly trend of total orders throughout the day.

3. Percentage of Sales by Pizza Category: A pie chart that shows the distribution of sales across different pizza categories.

4. Percentage of Sales by Pizza Size: A pie chart that represents the percentage of sales attributed to different pizza sizes. 

5. Total Pizzas Sold by Pizza Category: A funnel chart that presents the total number of pizzas sold for each pizza category.

6. Top 5 Best Sellers by Revenue, Total Quantity and Total Orders : A bar chart highlighting the top 5 best-selling pizzas based on the Revenue, Total Quantity and Total Orders.

7. Bottom 5 Best Sellers by Revenue, Total Quantity and Total Orders: A bar chart showcasing the bottom 5 worst-selling pizzas based on the Revenue, Total Quantity and Total Orders.



<img width="500" alt="0001" src="https://github.com/bhagatvaidehi/Pizza-Sales/assets/31363646/7e980279-2da5-48dd-b02d-89fb1f4b9c3d">  <img width="500" alt="0002" src="https://github.com/bhagatvaidehi/Pizza-Sales/assets/31363646/a157aba0-269f-4f38-a06f-8042b09fd95d">






### Data Analysis

#### KPI's REQUIREMENTS:

1. Total Revenue:
   
```sql
SELECT SUM(total_price) AS Total_Revenue FROM pizza_sales
```

2. Average Order Value
```sql
SELECT (SUM(total_price) / COUNT(DISTINCT order_id)) AS Avg_order_Value FROM pizza_sales
```

3. Total Pizzas Sold
```sql
SELECT SUM(quantity) AS Total_pizza_sold FROM pizza_sales
```

4. Total Orders
```sql
SELECT COUNT(DISTINCT order_id) AS Total_Orders FROM pizza_sales
```

5. Average Pizzas Per Order
```sql
SELECT CAST(CAST(SUM(quantity) AS DECIMAL(10,2)) / 
CAST(COUNT(DISTINCT order_id) AS DECIMAL(10,2)) AS DECIMAL(10,2))
AS Avg_Pizzas_per_order
FROM pizza_sales
```

#### CHARTS REQUIREMENTS:

1. Daily Trend for Total Orders
```sql
SELECT DATENAME(DW, order_date) AS order_day, COUNT(DISTINCT order_id) AS total_orders 
FROM pizza_sales
GROUP BY DATENAME(DW, order_date)
```

2. Monthly Trend for Orders
```sql
select DATENAME(MONTH, order_date) as Month_Name, COUNT(DISTINCT order_id) as Total_Orders
from pizza_sales
GROUP BY DATENAME(MONTH, order_date)
```

3. % of Sales by Pizza Category
```sql
SELECT pizza_category, CAST(SUM(total_price) AS DECIMAL(10,2)) as total_revenue,
CAST(SUM(total_price) * 100 / (SELECT SUM(total_price) from pizza_sales) AS DECIMAL(10,2)) AS PCT
FROM pizza_sales
GROUP BY pizza_category
```

4. % of Sales by Pizza Size
```sql
SELECT pizza_size, CAST(SUM(total_price) AS DECIMAL(10,2)) as total_revenue,
CAST(SUM(total_price) * 100 / (SELECT SUM(total_price) from pizza_sales) AS DECIMAL(10,2)) AS PCT
FROM pizza_sales
GROUP BY pizza_size
ORDER BY pizza_size
```

5. Total Pizzas Sold by Pizza Category
```sql
SELECT pizza_category, SUM(quantity) as Total_Quantity_Sold
FROM pizza_sales
WHERE MONTH(order_date) = 2
GROUP BY pizza_category
ORDER BY Total_Quantity_Sold DESC
```

6.Top 5 Pizzas by Revenue
```sql
SELECT Top 5 pizza_name, SUM(total_price) AS Total_Revenue
FROM pizza_sales
GROUP BY pizza_name
ORDER BY Total_Revenue DESC
```

7.Bottom 5 Pizzas by Revenue
```sql
SELECT Top 5 pizza_name, SUM(total_price) AS Total_Revenue
FROM pizza_sales
GROUP BY pizza_name
ORDER BY Total_Revenue ASC
```

8. Top 5 Pizzas by Quantity
```sql
SELECT Top 5 pizza_name, SUM(quantity) AS Total_Pizza_Sold
FROM pizza_sales
GROUP BY pizza_name
ORDER BY Total_Pizza_Sold DESC
```

9. Bottom 5 Pizzas by Quantity
```sql
SELECT TOP 5 pizza_name, SUM(quantity) AS Total_Pizza_Sold
FROM pizza_sales
GROUP BY pizza_name
ORDER BY Total_Pizza_Sold ASC
```

10.Top 5 Pizzas by Total Orders
```sql
SELECT Top 5 pizza_name, COUNT(DISTINCT order_id) AS Total_Orders
FROM pizza_sales
GROUP BY pizza_name
ORDER BY Total_Orders DESC
```

11.Bottom 5 Pizzas by Total Orders
```sql
SELECT Top 5 pizza_name, COUNT(DISTINCT order_id) AS Total_Orders
FROM pizza_sales
GROUP BY pizza_name
ORDER BY Total_Orders ASC
```




### Results:


##### - Busiest Days and Times : 
- Orders are higest on weekends, Friday/Saturday evening.
- There are maximum orders for month of July and January.

##### - Sales Performance :
- Classic category contributes to maximum sales and total orders.
- Large size pizza contributes to maximum sales.

##### - Best Sellers :
- The Thai Chicken pizza contributes to maximum Revenue.
- The Classic Deluxe Pizza contributes to maximum Total quantities.
- The Classic Deluxe Pizza contributes to maximum Total orders.

##### - Worst Sellers :
- The Brie Carre contributes to minimum total quantities.
- The Brie Carre contributes to minimum total orders.


#### Recommendation:
- Offer weekday-specific promotions.
- Ensure fast and efficient delivery services.
- Gather customer feedback for improvements (For Specific the bottom ranked pizza)
- Collaborate with local businesses and schools.


#### References
- Stack Overflow
- Coursera
 

## RESTAURANT ORDER ANALYSIS: MENU PERFORMANCE & CUSTOMER INSIGHTS (SQL)

## Restaurant Menu & Customer Order Analysis (SQL Portfolio Project)

## Project Overview
This project analyzes customer order data for Taste of the World Café, a restaurant that recently rolled out a new menu. As the data analyst on this project, I explored the restaurant's menu_items and order_details tables using SQL to evaluate how the new menu is performing — identifying which items are popular, which are underperforming, and what patterns exist in customer ordering behavior.

## Project Objectives
Explore the menu_items table to understand the new menu's structure (categories, prices, item types).
Explore the order_details table to understand what order data has been collected (order volume, dates, item frequency).
Join both tables to analyze how customers are responding to the new menu — best/worst-selling items, revenue by category, and ordering trends among top customers.

## KEY QUESTIONS ANSWERED
 Here are the key questions this analysis answers, organized by section:
### Menu Exploration
1.How many items are on the menu?

2.What are the least and most expensive items?

3.How many Italian dishes are on the menu, and what are their price extremes?

4.How many dishes fall into each category?

5.What is the average dish price within each category?
### Order Exploration
6. What is the date range covered by the order data?
  
7. How many total orders were placed in that range?

8. How many individual items were ordered in that range?

9. Which orders contained the most items?
   
10. How many orders had more than 12 items?
### Combined Menu + Order Analysis
11. What are the least and most ordered items, and which categories do they belong to?
   
12. What were the top 5 highest-spending orders?
    
13. What items/categories made up the highest-spending order — and what insight does that reveal?
    
14. What items/categories made up the top 5 highest-spending orders — and what pattern emerges across them?

## DATA DESCRIPTION 
A MySQL script that builds a restaurant_db schema with two tables:
order_details — 12,234 rows of individual order line items, each with an order_details_id, order_id, order_date, order_time, and item_id (linking to a menu item).
Multiple rows share the same order_id when a customer ordered several items at once. Data spans Jan 1 – Mar 31, 2023.
menu_items — 32 rows, a lookup table of menu_item_id, item_name, category (American, Asian, Mexican, Italian), and price.
## DATA SOURCE 
https://mavenanalytics.io/data-playground?pageSize=10
## TOOLS USED 
SQL
## SQL ANALYSIS AND QUERIES
## exploring the item table 
### 1. view the menu_items table.
```sql
select * from menu_items;
```
### 2. find the number of items on the menu
```sql
select count(*)  from menu_items;
```
###  3. what are the least and most expensive items on the menu? 
```sql
select * from menu_items
order by price;
select * from menu_items
order by price DESC;
```
### 4. how many italian dishes are on the menu?
```sql
select count(*) FROM menu_items 
WHERE category= 'italian';
```

### 5. what are the least and most expensive italian dishes on the meanu?
```sql
select* 
from menu_items 
where category='italian'
order by price;

select* 
from menu_items 
where category='italian'
order by price desc;
```
### 6. how many dishes are in each cotegory?
```sql
SELECT category, count(menu_item_id) as num_dishes
from menu_items 
group by category;
```
### 7. waht is the average dish price within each category?
```sql
SELECT category, avg(price) as avg_price
from menu_items 
group by category;
```
## EXPLORING THE ORDER TABLE

### 1. VIEW THE ORDER TABLE 
```sql
SELECT * FROM order_details;
```
### 2. WHAT IS THE DATE RANGE OF THE TABLE?
```sql
SELECT MIN(order_details) , MAX (order_date) from order_details;
```
### 3. HOW MANY ORDERS WERE MADE WITHIN THIS DATE RANGE?
```SQL
select count(distinct order_id) FROM order_details;
```
### 4. HOW MANY ITEMS WERE OREDERED WITHIN THIS DATE RANGE?
```sql
SELECT COUNT(*) FROM order_details;
```
### 5. WHICH ORDERS HAD THE MOST NUMBER OF ITEMS?
```sql
select order_id , count(item_id) as num_items 
from order_details
group by order_id
order by num_items desc;
```
### 6. HOW MANY ORDERS HAD  MORE THAN 12 ITEMS?
```sql
(select order_id , count(item_id) as num_items 
from order_details
group by order_id;
```

## Combined Menu + Order Analysis 
### 1. COMBINE THE MENU_ITEMS AND ORDER_DETAILS TABLES INTO A SINGLE TABLE 
```sql
select * FROM menu_items;
select * FROM order_details;
select * 
FROM order_details OD LEFT JOIN menu_items MI 
       ON OD.item_id = MI.menu_item_id; 

```
### 2. WHAT ARE THE LEAST AND MOST OREDERED ITEMS? WHAT CATEGORIES WERE THEY IN ?
```sql
select item_name , count(order_details_id) AS NUM_PURCHASES
FROM order_details OD LEFT JOIN menu_items MI 
       ON OD.item_id = MI.menu_item_id
group by item_name
order by NUM_PURCHASES desc;
```
### 3.  WHAT WERE THE TOP 5 ORDERS THAT SPENT THE MOST MONEY?
```sql
select order_id, sum(price) as  total_spend
FROM order_details OD LEFT JOIN menu_items MI 
       ON OD.item_id = MI.menu_item_id
       group by order_id
       order by total_spend desc
       limit 5;
```

### 4. VIEW THE DETAILS OF THE HIGHEST SPEND ORDER.  WHAT INSIGHTS CAN YOU GATHER FROM THE data
```sql
select category, count( item_id) as numP_items 
FROM order_details OD LEFT JOIN menu_items MI 
       ON OD.item_id = MI.menu_item_id
       where order_id = 440
       group by category;
```

###  5. VIEW THE DETAILS OF THE TOP 5 HIGHEST SPEND ORDERS. WHAT INSIGHTS CAN YOU GATHER FROM 
```sql
select category, count( item_id) as num_items 
FROM order_details OD LEFT JOIN menu_items MI 
       ON OD.item_id = MI.menu_item_id
       where order_id in (440,2075,1957,330,2675)
       group by order_id,category;
```
     
## Recommendations

1. Promote the most ordered dishes
   The restaurant should give greater promotional attention to the most frequently purchased dishes. These items can be highlighted on the menu, recommended to customers, or included in special offers.

2. Review the least ordered items
   Items with very low purchase volumes should be reviewed. Management should consider improving their presentation, adjusting their prices, promoting them more effectively, or removing them if they consistently perform poorly.

3. Focus on high-value orders
   The restaurant should study the characteristics of the highest-spending orders and encourage customers to purchase more items through meal combinations, add-ons, and premium options.

4. Develop strategic menu pricing
   The average prices across categories should be compared with their sales performance. Categories with strong demand and reasonable prices could receive more attention, while expensive dishes with low demand may require price adjustments.

5. Create meal combinations and bundles
   Since some orders contain multiple items, the restaurant could introduce combinations such as main dish + side + drink. This could increase the average amount spent per order.

6. Improve inventory management
   The most ordered dishes should receive priority when planning stock levels. Maintaining sufficient ingredients for popular dishes can reduce the risk of stockouts and lost sales.

7. Use customer ordering patterns for marketing
   The restaurant should use its order data to create targeted promotions around popular categories and dishes. For example, frequently purchased items could be promoted during periods of lower sales.

8. Monitor sales performance regularly
   Management should continue using SQL or business intelligence tools such as Power BI to monitor orders, revenue, popular dishes, average order value, and category performance over time.

9. Investigate large orders
   Orders with unusually high numbers of items should be analyzed further to determine whether they are individual customer orders, group orders, or special events. This could reveal opportunities for catering and bulk-order services.

10. Make data-driven menu decisions
    Future decisions about adding, removing, pricing, or promoting menu items should be based on actual sales and customer ordering data rather than assumptions.

## Conclusion

The SQL analysis of the restaurant database provided useful insights into the restaurant's menu and order patterns. By combining the "menu_items" and "order_details" tables, it was possible to examine customer purchasing behavior, identify the most and least ordered menu items, analyze the highest-spending orders, and understand the distribution of dishes across different food categories.

The analysis showed that some menu items were ordered significantly more frequently than others, indicating that customers have clear preferences for certain dishes. The analysis of the top five highest-spending orders also showed that some customers placed larger orders containing multiple items and categories. This information can help the restaurant understand which categories and dishes contribute most to sales.

The menu analysis further revealed the number of dishes available in each category, the average price of dishes within each category, and the differences between the least and most expensive items. These findings can help management evaluate the restaurant's menu structure and pricing strategy.

Overall, SQL provided an effective way to transform the restaurant's raw transactional data into meaningful business insights. The findings can be used to improve menu planning, pricing, inventory management, marketing, and customer satisfaction.

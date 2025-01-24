# Pizza-Sales-Data-Analysis-Using-SQL
Project Objective
The main objective of this project was to analyze pizza sales data and derive actionable insights. By utilizing SQL queries, critical aspects such as sales trends, customer preferences, and revenue distribution were explored and understood.

# Key Queries and Insights
1 Total Number of Orders Placed

SELECT COUNT(order_id) AS total_orders FROM orders;

Insight: Total number of orders placed in the dataset.
Use Case: Business ko pata chalega kitne orders process hue.

2. Total Revenue Generated from Pizza Sales

SELECT ROUND(SUM(order_details.quantity * pizzas.price), 2) AS total_revenue
FROM order_details 
JOIN pizzas ON pizzas.pizza_id = order_details.pizza_id;

Insight: Total revenue generated from all pizza sales.
Use Case: Financial performance evaluate karna.

3. Orders Placed on a Specific Date

SELECT * FROM orders WHERE date = '2015-01-01';
Insight: Retrieve all orders placed on January 1, 2015.
Use Case: Date-specific sales trends analyze karna.
4. Total Quantity of Pizzas Ordered for Each Pizza Type

SELECT pizza_id, SUM(quantity) AS total_quantity 
FROM order_details 
GROUP BY pizza_id 
ORDER BY total_quantity DESC;
Insight: Sabse zyada aur sabse kam order hone wale pizza types ka pata chala.

5. Highest Price Pizza

SELECT pizza_types.name, pizzas.price 
FROM pizza_types 
JOIN pizzas ON pizza_types.pizza_type_id = pizzas.pizza_type_id
ORDER BY pizzas.price DESC LIMIT 1;

Insight: Sabse mehnga pizza aur uska naam.
Use Case: Premium pricing strategy ke liye useful.

6. Most Common Pizza Size Ordered

SELECT pizzas.size, COUNT(order_details.order_details_id) AS most_order 
FROM pizzas 
JOIN order_details ON pizzas.pizza_id = order_details.pizza_id
GROUP BY pizzas.size 
ORDER BY most_order DESC LIMIT 1;
Insight: Popular pizza size (Small, Medium, Large, etc.).
Use Case: Inventory planning ke liye.
7. Top 5 Most Ordered Pizza Types

SELECT pizza_types.name, SUM(order_details.quantity) AS quantity 
FROM pizza_types 
JOIN pizzas ON pizza_types.pizza_type_id = pizzas.pizza_type_id
JOIN order_details ON order_details.pizza_id = pizzas.pizza_id
GROUP BY pizza_types.name 
ORDER BY quantity DESC LIMIT 5;
Insight: Top 5 pizza types jo sabse zyada order hue.
8. Hourly Order Distribution

SELECT HOUR(time), COUNT(order_id) 
FROM orders 
GROUP BY HOUR(time);
Insight: Orders kis time slot me zyada aate hain.
Use Case: Peak hours identify karne ke liye.
9. Pizza Category Distribution

SELECT COUNT(name), category 
FROM pizza_types 
GROUP BY category;
Insight: Kaunse category (e.g., Veg, Non-Veg) ki pizzas ka demand zyada hai.
10. Average Number of Pizzas Ordered Per Day

SELECT ROUND(AVG(quantity), 0) AS avg_pizzas_per_day 
FROM (
    SELECT orders.date, SUM(order_details.quantity) AS quantity 
    FROM orders 
    JOIN order_details ON orders.order_id = order_details.order_id
    GROUP BY orders.date
) AS daily_data;
Insight: Ek din me average kitni pizzas order hoti hain.
11. Top 3 Most Ordered Pizza Types Based on Revenue

SELECT pizza_types.name, SUM(order_details.quantity * pizzas.price) AS revenue 
FROM pizza_types 
JOIN pizzas ON pizzas.pizza_type_id = pizza_types.pizza_type_id
JOIN order_details ON order_details.pizza_id = pizzas.pizza_id
GROUP BY pizza_types.name 
ORDER BY revenue DESC LIMIT 3;
Insight: Top 3 pizza types jo revenue generate karte hain.
12. Percentage Contribution of Each Pizza Category to Total Revenue

SELECT pizza_types.category, 
ROUND(SUM(order_details.quantity * pizzas.price) / 
(SELECT SUM(order_details.quantity * pizzas.price) 
 FROM order_details 
 JOIN pizzas ON pizzas.pizza_id = order_details.pizza_id) * 100, 2) AS revenue_percentage 
FROM pizza_types 
JOIN pizzas ON pizzas.pizza_type_id = pizza_types.pizza_type_id
JOIN order_details ON order_details.pizza_id = pizzas.pizza_id
GROUP BY pizza_types.category 
ORDER BY revenue_percentage DESC;
Insight: Har pizza category ka revenue me contribution.
13. Cumulative Revenue Over Time

SELECT date, SUM(revenue) OVER (ORDER BY date) AS cumulative_revenue 
FROM (
    SELECT orders.date, ROUND(SUM(order_details.quantity * pizzas.price), 2) AS revenue 
    FROM orders 
    JOIN order_details ON orders.order_id = order_details.order_id
    JOIN pizzas ON pizzas.pizza_id = order_details.pizza_id
    GROUP BY orders.date
) AS daily_revenue;
Insight: Time ke saath revenue ka growth pattern.
14. Top 3 Most Ordered Pizza Types in Each Category

SELECT name, revenue 
FROM (
    SELECT category, name, revenue, RANK() OVER (PARTITION BY category ORDER BY revenue DESC) AS rank 
    FROM (
        SELECT pizza_types.category, pizza_types.name, SUM(order_details.quantity * pizzas.price) AS revenue 
        FROM pizza_types 
        JOIN pizzas ON pizza_types.pizza_type_id = pizzas.pizza_type_id
        JOIN order_details ON order_details.pizza_id = pizzas.pizza_id
        GROUP BY pizza_types.category, pizza_types.name
    ) AS category_revenue
) AS ranked_revenue 
WHERE rank <= 3;
Insight: Har category ke top 3 pizza types.
15. Ingredients of Each Pizza Ordered

SELECT order_details.order_id, pizza_types.name AS pizza_name, pizza_types.ingredients, order_details.quantity 
FROM order_details 
JOIN pizza_types 
ON REPLACE(REPLACE(REPLACE(REPLACE(order_details.pizza_id, '_s', ''), '_m', ''), '_l', ''), '_xl', '') = pizza_types.pizza_type_id;
Insight: Har order ke saath pizza ka naam aur uske ingredients.
Project Outcome
Identified customer preferences and high-performing pizza categories.
Derived actionable insights for inventory planning and revenue optimization.
Created time-based sales trends and cumulative revenue patterns.

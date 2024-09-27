# Pizza-Sales-SQL-Queries
### Hi there 👋, I'm Hafsa Ali
#### SQL Developer | Data Science & Analytics Enthusiast | Solving Problems with Data 
![SQL Developer | Data Science & Analytics Enthusiast | Solving Problems with Data ](https://github.com/Hafsa-Ali/Pizza-Sales-SQL-Queries/blob/main/Queries/Banner.webp)

This project showcases SQL queries for analyzing a fictional pizza sales dataset, named as the Pizza Sales Schema. It includes a series of SQL challenges ranging from basic to advanced, along with the corresponding solutions. The project is designed to demonstrate SQL skills, including data retrieval, joining tables, aggregation, and more.

What's Included:

- Pizza Sales Schema (Tables): The database structure for analyzing pizza sales.
- Questions (.txt): The SQL problem statements categorized as Basic, Intermediate, and Advanced.
- Queries (Screenshots): Visual proof of successful query executions.

Skills: SQL
## SQL Challenges and Solutions

### Basic Questions
1. **Question 1: [Retrieve the total number of orders placed.]**
   - **Snapshot:**  
     ![Query 1 Snapshot](https://github.com/Hafsa-Ali/Pizza-Sales-SQL-Queries/blob/main/Queries/Basic%201.PNG)

   - **SQL Query:**
     ```sql
      select count(order_id) from orders;
     ```
2. **Question 2: [Calculate the total revenue generated from pizza sales.]**
   - **Snapshot:**  
     ![Query 2 Snapshot](https://github.com/Hafsa-Ali/Pizza-Sales-SQL-Queries/blob/main/Queries/Basic%202.PNG)
   - **SQL Query:**
     ```sql
           SELECT 
          SUM(order_details.quantity * pizzas.price) AS total_revenue
      FROM
          order_details
              JOIN
          pizzas ON pizzas.pizza_id = order_details.pizza_id;
     ```
3. **Question 3: [Identify the highest-priced pizza.]**
   - **Snapshot:**  
     ![Query 3 Snapshot](https://github.com/Hafsa-Ali/Pizza-Sales-SQL-Queries/blob/main/Queries/Basic%203.PNG)

   - **SQL Query:**
     ```sql
     SELECT 
        pizza_types.name, pizzas.price
     FROM
        pizza_types
           JOIN
        pizzas ON pizza_types.pizza_type_id = pizzas.pizza_type_id
     ORDER BY price DESC
     LIMIT 1;
     ```
4. **Question 4: [Identify the most common pizza size ordered.]**
   - **Snapshot:**  
     ![Query 4 Snapshot](https://github.com/Hafsa-Ali/Pizza-Sales-SQL-Queries/blob/main/Queries/Basic%204.PNG)

   - **SQL Query:**
     ```sql
     SELECT 
        pizzas.size, COUNT(order_details.quantity)
     FROM
        pizzas
           JOIN
        order_details ON pizzas.pizza_id = order_details.pizza_id
     GROUP BY pizzas.size;
     ```
5. **Question 5: [List the top 5 most ordered pizza types along with their quantities.]**
   - **Snapshot:**  
     ![Query 5 Snapshot](https://github.com/Hafsa-Ali/Pizza-Sales-SQL-Queries/blob/main/Queries/Basic%205.PNG)

   - **SQL Query:**
     ```sql
      SELECT 
          pizza_types.name, SUM(order_details.quantity) AS quantity
      FROM
          pizza_types
              JOIN
          pizzas ON pizza_types.pizza_type_id = pizzas.pizza_type_id
              JOIN
          order_details ON order_details.pizza_id = pizzas.pizza_id
      GROUP BY pizza_types.name
      ORDER BY quantity DESC
      LIMIT 5;
     ```
   
### Intermediate Questions
1. **Question 1: [Join the necessary tables to find the total quantity of each pizza category ordered.]**
   - **Snapshot:**  
     ![Query 1 Snapshot](https://github.com/Hafsa-Ali/Pizza-Sales-SQL-Queries/blob/main/Queries/Inter%201.PNG)
   - **SQL Query:**
     ```sql
      SELECT 
       SUM(order_details.quantity) AS quan,
       pizza_types.category AS cat
      FROM
        order_details
           JOIN
        pizzas ON order_details.pizza_id = pizzas.pizza_id
           JOIN
        pizza_types ON pizza_types.pizza_type_id = pizzas.pizza_type_id
      GROUP BY category
      ORDER BY quan DESC;
     ```
2. **Question 2: [Determine the distribution of orders by hour of the day.]**
   - **Snapshot:**  
     ![Query 2 Snapshot](https://github.com/Hafsa-Ali/Pizza-Sales-SQL-Queries/blob/main/Queries/Inter%202.PNG)
   - **SQL Query:**
     ```sql
      SELECT 
          HOUR(order_time) AS tm, COUNT(order_id)
      FROM
          orders
      GROUP BY tm;
     ```
3. **Question 3: [Join relevant tables to find the category-wise distribution of pizzas.]**
   - **Snapshot:**  
     ![Query 3 Snapshot](https://github.com/Hafsa-Ali/Pizza-Sales-SQL-Queries/blob/main/Queries/Inter%203.PNG)
   - **SQL Query:**
     ```sql
      SELECT 
       category, COUNT(name)
      FROM
          pizza_types
      GROUP BY category
     ```
4. **Question 4: [Group the orders by date and calculate the average number of pizzas ordered per day.]**
   - **Snapshot:**  
     ![Query 4 Snapshot](https://github.com/Hafsa-Ali/Pizza-Sales-SQL-Queries/blob/main/Queries/Inter%204.PNG)
   - **SQL Query:**
     ```sql
     select round(avg(quan),0) from (
     SELECT 
        orders.order_date, sum(order_details.quantity) as quan
     FROM
        orders
           JOIN
        order_details ON orders.order_id = order_details.order_id
     GROUP BY orders.order_date) 
     as order_quan;
     ```
5. **Question 5: [Determine the top 3 most ordered pizza types based on revenue.]**
   - **Snapshot:**  
     ![Query 5 Snapshot](https://github.com/Hafsa-Ali/Pizza-Sales-SQL-Queries/blob/main/Queries/Inter%205.PNG)
     ![Query 5 Snapshot](https://github.com/Hafsa-Ali/Pizza-Sales-SQL-Queries/blob/main/Queries/Inter%205a.PNG)
   - **SQL Query:**
     ```sql
      SELECT 
          pizza_types.pizza_type_id,
          SUM(pizzas.price * order_details.quantity) AS revenue,
          pizza_types.name
      FROM
          pizza_types
              JOIN
          pizzas ON pizza_types.pizza_type_id = pizzas.pizza_type_id
              JOIN
          order_details ON order_details.pizza_id = pizzas.pizza_id
      GROUP BY pizza_types.pizza_type_id , pizza_types.name
      ORDER BY revenue DESC
      LIMIT 3
     ```
### Advanced Questions
1. **Question 1: [Calculate the percentage contribution of each pizza type to total revenue.]**
   - **Snapshot:**  
     ![Query 1 Snapshot](https://github.com/Hafsa-Ali/Pizza-Sales-SQL-Queries/blob/main/Queries/Adv%201.PNG)
     ![Query 1 Snapshot](https://github.com/Hafsa-Ali/Pizza-Sales-SQL-Queries/blob/main/Queries/Adv%201a.PNG)
   - **SQL Query:**
     ```sql
     SELECT 
       pizza_types.category,
       ROUND(SUM(pizzas.price * order_details.quantity) / (SELECT 
                       ROUND(SUM(order_details.quantity * pizzas.price),
                                   2) AS total_revenue
                   FROM
                       order_details
                           JOIN
                       pizzas ON pizzas.pizza_id = order_details.pizza_id) * 100,
               2) AS revenue
     FROM
          pizza_types
              JOIN
          pizzas ON pizza_types.pizza_type_id = pizzas.pizza_type_id
              JOIN
          order_details ON order_details.pizza_id = pizzas.pizza_id
      GROUP BY pizza_types.category
      ORDER BY revenue
     ```
2. **Question 2: [Analyze the cumulative revenue generated over time.]**
   - **Snapshot:**
     ![Query 2 Snapshot](https://github.com/Hafsa-Ali/Pizza-Sales-SQL-Queries/blob/main/Queries/Adv%202.PNG)
     ![Query 2 Snapshot](https://github.com/Hafsa-Ali/Pizza-Sales-SQL-Queries/blob/main/Queries/Adv%202a.PNG)
   - **SQL Query:**
     ```sql
     SELECT 
          sales.order_date, 
          SUM(sales.revenue) OVER (ORDER BY sales.order_date) AS cumulative_rev
     FROM 
          (SELECT 
              orders.order_date, 
              SUM(order_details.quantity * pizzas.price) AS revenue
           FROM 
              order_details
           JOIN 
              pizzas ON order_details.pizza_id = pizzas.pizza_id
           JOIN 
              orders ON orders.order_id = order_details.order_id
           GROUP BY 
              orders.order_date
          ) AS sales;
     ```
3. **Question 3: [Determine the top 3 most ordered pizza types based on revenue for each pizza category.]**
   - **Snapshot:**  
     ![Query 3 Snapshot](https://github.com/Hafsa-Ali/Pizza-Sales-SQL-Queries/blob/main/Queries/Adv%203.PNG)
     ![Query 3 Snapshot](https://github.com/Hafsa-Ali/Pizza-Sales-SQL-Queries/blob/main/Queries/Adv%203q.PNG)
     ![Query 3 Snapshot](https://github.com/Hafsa-Ali/Pizza-Sales-SQL-Queries/blob/main/Queries/Adv%203a.PNG)
   - **SQL Query:**
     ```sql
     SELECT 
          name, 
          revenue 
      FROM 
          (SELECT 
              category, 
              name, 
              revenue,
              RANK() OVER (PARTITION BY category ORDER BY revenue DESC) AS rn
          FROM 
              (SELECT 
                  pizza_types.category, 
                  pizza_types.name, 
                  SUM(order_details.quantity * pizzas.price) AS revenue
              FROM 
                  pizza_types
              JOIN 
                  pizzas ON pizza_types.pizza_type_id = pizzas.pizza_type_id 
              JOIN 
                  order_details ON order_details.pizza_id = pizzas.pizza_id
              GROUP BY 
                  pizza_types.category, pizza_types.name
              ) AS rev
          ) AS rev_rank
      WHERE 
          rn <= 3;
     ```
- 🔭 I’m currently working on this page. 
- 🌱 I’m currently learning Data Science & Data Analytics 
- 📫 How to reach me: LinkedIn: https://www.linkedin.com/in/-hafsa-ali/ 
- ⚡ Fun fact: When I'm not analyzing data, you can find me exploring new pizza recipes 🍕 


[<img src='https://cdn.jsdelivr.net/npm/simple-icons@3.0.1/icons/github.svg' alt='github' height='40'>](https://github.com/Hafsa-Ali)  [<img src='https://cdn.jsdelivr.net/npm/simple-icons@3.0.1/icons/linkedin.svg' alt='linkedin' height='40'>](https://www.linkedin.com/in/https://www.linkedin.com/in/-hafsa-ali//)  




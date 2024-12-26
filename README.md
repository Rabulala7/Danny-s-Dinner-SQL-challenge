# Danny's Diner - Danny Ma SQL challenge (Case Study #1)

![Sticker - Danny's Diner](https://github.com/user-attachments/assets/e2aae3d5-199d-4ec5-bc37-e7629aedaea2)

This is a challenge to hone and prove my SQL capabilities.

# Introduction
Danny wants to use this data to answer a few simple questions about his customers. 

He plans on using answers from these questions to help him decide whether he should expand the existing customer loyalty program - additionally he needs help to generate some basic datasets so his team can easily inspect the data without needing to use SQL.

Danny has provided you with a sample of his overall customer data due to privacy issues - but he hopes that these examples are enough for you to write fully functioning SQL queries to help him answer his questions!

Below are the key datasets for this study 

* [Sales](https://github.com/Rabulala7/Danny-s-Dinner-SQL-challenge/blob/main/datasets/Sales.csv)
* [Menu](https://github.com/Rabulala7/Danny-s-Dinner-SQL-challenge/blob/main/datasets/Menu.csv)
* [Members](https://github.com/Rabulala7/Danny-s-Dinner-SQL-challenge/blob/main/datasets/Members.csv)


# 1. What is the total amount each customer spent at the restaurant?


    SELECT customer_id, 
           SUM(price) AS money_spent
    FROM sales AS s
    JOIN menu AS m
    ON m.product_id = s.product_id
    GROUP BY customer_id





# 2. How many days has each customer visited the restaurant?


    SELECT customer_id, 
           COUNT(DISTINCT order_date) AS days_visited
    FROM sales
    GROUP BY customer_id



# 3. What was the first item from the menu purchased by each customer?


    SELECT customer_id, 
        order_date, 
        product_name 
    FROM sales AS S
    JOIN menu AS m
    ON s.product_id = m.product_id
    ORDER BY order_date



# 4. What is the most purchased item on the menu and how many times was it purchased by all customers?



    SELECT  product_name,
            COUNT(product_name) AS purchase_count
    FROM sales AS s
    JOIN menu AS m
    ON m.product_id = s.product_id
    GROUP BY product_name
    ORDER BY purchase_count DESC
    LIMIT 1


# 5. Which item was the most popular for each customer?



    SELECT  customer_id,
            product_name,
            COUNT(product_name) AS purchase_count
    FROM sales AS s
    JOIN menu AS m
    ON m.product_id = s.product_id
    GROUP BY customer_id, product_name
    ORDER BY purchase_count DESC, customer_id DESC
    LIMIT 3



# 6. Which item was purchased first by the customer after they became a member?



    SELECT s.customer_id,
        product_name,
        order_date
    FROM sales AS s
    JOIN menu AS m
    ON s.product_id = m.product_id
    JOIN members AS mem
    ON s.order_date > mem.join_date 
        AND s.customer_id = mem.customer_id
    ORDER BY order_date, s.customer_id DESC


# 7. Which item was purchased just before the customer became a member?



    SELECT s.customer_id,
        product_name,
        order_date
    FROM sales AS s
    JOIN menu AS m
    ON s.product_id = m.product_id
    JOIN members AS mem
    ON s.order_date <= mem.join_date 
        AND s.customer_id = mem.customer_id
    ORDER BY order_date DESC, s.customer_id DESC



# 8. What is the total items and amount spent for each member before they became a member?


    SELECT s.customer_id,
        COUNT(product_name) AS total_items,
        SUM(price) AS total_amount
    FROM sales AS s
    JOIN menu AS m
    ON s.product_id = m.product_id
    JOIN members AS mem
    ON s.order_date <= mem.join_date 
        AND s.customer_id = mem.customer_id
    GROUP BY s.customer_id
    ORDER BY s.customer_id DESC





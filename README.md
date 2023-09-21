# Balanced Tree SQL case study

![balanced_tree.png](balanced_tree.png)

This is a case study developed by Danny Ma. It can be found on his website [here.](https://8weeksqlchallenge.com/case-study-7/)

## Introduction

Balanced Tree Clothing Company prides themselves on providing an optimised range of clothing and lifestyle wear for the modern adventurer!

Danny, the CEO of this trendy fashion company has asked you to assist the team’s merchandising teams analyse their sales performance and generate a basic financial report to share with the wider business.

## Available data

### Product Details

balanced_tree.product_details includes all information about the entire range that Balanced Clothing sells in their store.

| product_id | price | product_name                     | category_id | segment_id | style_id | category_name | segment_name | style_name          |
| ---------- | ----- | -------------------------------- | ----------- | ---------- | -------- | ------------- | ------------ | ------------------- |
| c4a632     | 13    | Navy Oversized Jeans - Womens    | 1           | 3          | 7        | Womens        | Jeans        | Navy Oversized      |
| e83aa3     | 32    | Black Straight Jeans - Womens    | 1           | 3          | 8        | Womens        | Jeans        | Black Straight      |
| e31d39     | 10    | Cream Relaxed Jeans - Womens     | 1           | 3          | 9        | Womens        | Jeans        | Cream Relaxed       |
| d5e9a6     | 23    | Khaki Suit Jacket - Womens       | 1           | 4          | 10       | Womens        | Jacket       | Khaki Suit          |
| 72f5d4     | 19    | Indigo Rain Jacket - Womens      | 1           | 4          | 11       | Womens        | Jacket       | Indigo Rain         |
| 9ec847     | 54    | Grey Fashion Jacket - Womens     | 1           | 4          | 12       | Womens        | Jacket       | Grey Fashion        |
| 5d267b     | 40    | White Tee Shirt - Mens	          | 2           | 5          | 13       | Mens          | Shirt        | White Tee           |
| c8d436     | 10    | Teal Button Up Shirt - Mens      | 2           | 5          | 14       | Mens          | Shirt        | Teal Button Up      |
| 2a2353     | 57    | Blue Polo Shirt - Mens           | 2           | 5          | 15       | Mens          | Shirt        | Blue Polo           |
| f084eb     | 36    | Navy Solid Socks - Mens          | 2           | 6          | 16       | Mens          | Socks        | Navy Solid          |
| b9a74d     | 17    | White Striped Socks - Mens       | 2           | 6          | 17       | Mens          | Socks        | White Striped       |
| 2feb6b     | 29    | Pink Fluro Polkadot Socks - Mens | 2           | 6          | 18       | Mens          | Socks        | Pink Fluro Polkadot |

### Product Sales

balanced_tree.sales contains product level information for all the transactions made for Balanced Tree including
quantity, price, percentage discount, member status, a transaction ID and also the transaction timestamp.

| prod_id | qty | price | discount | member | txn_id | start_txn_time           |
| ------- | --- | ----- | -------- | ------ | ------ | ------------------------ |
| c4a632  | 4   | 13    | 17       | t      | 54f307 | 2021-02-13 01:59:43.296  |
| 5d267b  | 4	  | 40    | 17       | t	    | 54f307 | 2021-02-13 01:59:43.296  |
| b9a74d  | 4	  | 17    | 17       | t      | 54f307 | 2021-02-13 01:59:43.296  |
| 2feb6b  | 2   | 29    | 17       | t      | 54f307 | 2021-02-13 01:59:43.296  |
| c4a632  | 5   | 13    | 21       | t      | 26cc98 | 2021-01-19 01:39:00.3456 |
| e31d39  | 2   | 10    | 21       | t      | 26cc98 | 2021-01-19 01:39:00.3456 |
| 72f5d4  | 3   | 19    | 21       | t      | 26cc98 | 2021-01-19 01:39:00.3456 |
| 2a2353  | 3   | 57    | 21       | t      | 26cc98 | 2021-01-19 01:39:00.3456 |
| f084eb  | 3   | 36    | 21       | t      | 26cc98 | 2021-01-19 01:39:00.3456 |
| c4a632  | 1   | 13    | 21       | f      | ef648d | 2021-01-27 02:18:17.1648 |

## A. High Level Sales Analysis
1. What was the total quantity sold for all products?
2. What is the total generated revenue for all products before discounts?
3. What was the total discount amount for all products?
   
## B. Transaction Analysis
1. How many unique transactions were there?
2. What is the average unique products purchased in each transaction?
3. What are the 25th, 50th and 75th percentile values for the revenue per transaction?
4. What is the average discount value per transaction?
5. What is the percentage split of all transactions for members vs non-members?
6. What is the average revenue for member transactions and non-member transactions?
   
## C. Product Analysis
1. What are the top 3 products by total revenue before discount?
2. What is the total quantity, revenue and discount for each segment?
3. What is the top selling product for each segment?
4. What is the total quantity, revenue and discount for each category?
5. What is the top selling product for each category?
6. What is the percentage split of revenue by product for each segment?
7. What is the percentage split of revenue by segment for each category?
8. What is the percentage split of total revenue by category?
9. What is the total transaction “penetration” for each product? (hint: penetration = number of transactions where at least 1 quantity of a product was purchased divided by total number of transactions)

## A. High Level Sales Analysis

### Question 1. What was the total quantity sold for all products?
   
<pre>
SELECT SUM(qty) AS total_qty
FROM balanced_tree.sales;
</pre>

Output: 
| total_qty |
| --------- |
| 45216     |

### Question 2. What is the total generated revenue for all products before discounts?

<pre>
SELECT SUM(price * qty) AS total_sales
FROM balanced_tree.sales;
</pre>

Output:
| total_sales |
| ----------- |
| 1289453     |

### Question 3. What was the total discount amount for all products?

<pre>
SELECT ROUND(SUM(qty * price * discount::numeric/100), 2) AS total_discount
FROM balanced_tree.sales;
</pre>

Output:
| total_discount |
| -------------- |
| 156229.14      |

## B. Transaction Analysis

### Question 1. How many unique transactions were there?

<pre>
SELECT COUNT(DISTINCT txn_id) AS total_txn
FROM balanced_tree.sales;
</pre>

Output:
| total_txn |
| --------- |
| 2500      |

### Question 2. What is the average unique products purchased in each transaction?

<pre>
WITH unique_products AS (
	SELECT txn_id, COUNT (DISTINCT prod_id) AS product_count
	FROM balanced_tree.sales
	GROUP BY txn_id
  	)
SELECT ROUND(AVG(product_count)) AS avg_uniq_prod
FROM unique_products;
</pre>

Output:
| avg_uniq_prod |
| ------------- |
| 6             |

### Question 3. What are the 25th, 50th and 75th percentile values for the revenue per transaction?

<pre>
WITH transaction_revenue AS (
 	SELECT ROUND(((qty * price) - ((qty * price * discount)::Numeric / 100)), 2) AS revenue
  	FROM balanced_tree.sales
	)
SELECT
   	PERCENTILE_DISC(0.25) WITHIN GROUP(ORDER BY revenue) AS pct_25,
   	PERCENTILE_DISC(0.5) WITHIN GROUP(ORDER BY revenue) AS pct_50,
   	PERCENTILE_DISC(0.75) WITHIN GROUP(ORDER BY revenue) AS pct_75
FROM transaction_revenue;
</pre>

Output:
| pct_25 | pct_50 | pct_75 |
| ------ | ------ | ------ |
| 31.54  | 55.76  | 103.74 |

### Question 4. What is the average discount value per transaction?

<pre>
WITH transaction_discounts AS (
	SELECT txn_id, SUM(price * qty * ((100 - discount) / 100)) AS total_discount
	FROM balanced_tree.sales
	GROUP BY txn_id
	)
SELECT ROUND(AVG(total_discount), 2) AS avg_discount_value
FROM transaction_discounts;
</pre>

Output:
| avg_discount_value |
| ------------------ |
| 16.33              |

### Question 5. What is the percentage split of all transactions for members vs non-members?

<pre>
SELECT member, ROUND(COUNT(DISTINCT txn_id)/(SUM(COUNT(DISTINCT txn_id)) OVER()) * 100) AS percent
FROM balanced_tree.sales
GROUP BY member;
</pre>

Output:
| member | percent |
| ------ | ------- |
| false  | 40      |
| true   | 60      |

### Question 6. What is the average revenue for member transactions and non-member transactions?

<pre>
WITH member_revenue AS (
	SELECT member, txn_id, SUM(price * qty * ((100 - discount) / 100)) AS revenue
  	FROM balanced_tree.sales
  	GROUP BY member, txn_id
	)
SELECT member, ROUND(AVG(revenue), 2) AS avg_revenue
FROM member_revenue
GROUP BY member;
</pre>

Output:
| member | avg_revenue |
| ------ | ----------- |
| false  | 14.23       |
| true   | 17.72       |

## C. Product Analysis

### Question 1. What are the top 3 products by total revenue before discount?

<pre>
SELECT details.product_name, SUM(sales.price * sales.qty) AS total_revenue
FROM balanced_tree.sales AS sales
	JOIN balanced_tree.product_details AS details
	ON sales.prod_id = details.product_id
GROUP BY details.product_name
ORDER BY total_revenue DESC
LIMIT 3;
</pre>

Output:
| product_name                 | total_ revenue |
| ---------------------------- | -------------- |
| Blue Polo Shirt - Mens       | 217683         |
| Grey Fashion Jacket - Womens | 209304         |
| White Tee Shirt - Mens       | 152000         |

### Question 2. What is the total quantity, revenue and discount for each segment?

<pre>
SELECT details.segment_name,
	SUM(sales.qty) AS total_qty, SUM(sales.price * sales.qty) AS total_revenue,
    SUM(sales.price * sales.qty * ((100 - discount) / 100)) AS total_discount
FROM balanced_tree.product_details AS details
	JOIN balanced_tree.sales AS sales ON
    details.product_id = sales.prod_id
GROUP BY details.segment_name
ORDER BY details.segment_name;
</pre>

Output:
| segment_name | total_qty | total_revenue | total_discount |
| ------------ | --------- | ------------- | -------------- |
| Jacket       | 11385     | 366983	       | 11290          |
| Jeans	       | 11349     | 208350        | 6332           |
| Shirt        | 11265     | 406143        | 12523          |
| Socks        | 11217     | 307977        | 10690          |

### Question 3. What is the top selling product for each segment?

<pre>
WITH top_selling AS ( 
	SELECT details.segment_id, details.segment_name, details.product_id, details.product_name,
    SUM(sales.qty) AS total_qty,
    RANK() OVER (
    	PARTITION BY segment_id 
    	ORDER BY SUM(sales.qty) DESC) AS ranking
  	FROM balanced_tree.sales
  		JOIN balanced_tree.product_details AS details
    	ON sales.prod_id = details.product_id
	GROUP BY details.segment_id, details.segment_name, details.product_id, details.product_name
	)
SELECT segment_id, segment_name, product_id, product_name, total_qty
FROM top_selling
WHERE ranking = 1
ORDER BY segment_name;
</pre>

Output:
| segment_id | segment_name | product_id | product_name                  | total_qty |
| ---------- | ------------ | ---------- | ----------------------------- | --------- |
| 4          | Jacket       | 9ec84      | Grey Fashion Jacket - Womens  | 3876      |
| 3          | Jeans        | c4a632     | Navy Oversized Jeans - Womens | 3856      |
| 5          | Shirt        | 2a2353     | Blue Polo Shirt - Mens        | 3819      |
| 6          | Socks        | f084eb     | Navy Solid Socks - Mens       | 3792      |

### Question 4. What is the total quantity, revenue and discount for each category?

<pre>
SELECT details.category_name,
	SUM(sales.qty) AS total_qty, SUM(sales.price * sales.qty) AS total_revenue,
    SUM(sales.price * sales.qty * ((100 - discount) / 100)) AS total_discount
FROM balanced_tree.product_details AS details
	JOIN balanced_tree.sales AS sales
    ON details.product_id = sales.prod_id
GROUP BY details.category_name
ORDER BY details.category_name;
</pre>

Output:
| category_name | total_qty | total_revenue | total_discount |
| ------------- | --------- | ------------- | -------------- |
| Mens          | 22482     | 714120        | 23213          |
| Womens        | 22734     | 575333        | 17622          |
  
### Question 5. What is the top selling product for each category?

<pre>
WITH top_selling AS ( 
	SELECT details.category_id, details.category_name, details.product_id, details.product_name,
    SUM(sales.qty) AS total_qty,
    RANK() OVER (
    	PARTITION BY category_id 
    	ORDER BY SUM(sales.qty) DESC) AS ranking
  	FROM balanced_tree.sales
  		JOIN balanced_tree.product_details AS details
    	ON sales.prod_id = details.product_id
	GROUP BY details.category_id, details.category_name, details.product_id, details.product_name
	)
SELECT category_id, category_name, product_id, product_name, total_qty
FROM top_selling
WHERE ranking = 1
ORDER BY category_id;
</pre>

Output:
| category_id | category_name	| product_id | product_name                 | total_qty |
| ----------- | ------------- | ---------- | ---------------------------- | --------- |
| 1           | Womens        | 9ec847     | Grey Fashion Jacket - Womens | 3876      |
| 2           | Mens          | 2a2353     | Blue Polo Shirt - Mens       | 3819      |

### Question 6. What is the percentage split of revenue by product for each segment?

<pre>
WITH product_revenue AS (
	SELECT details.segment_name, details.product_name,
    SUM(sales.qty * sales.price) AS product_revenue
  	FROM balanced_tree.sales AS sales
  		JOIN balanced_tree.product_details AS details
    	ON sales.prod_id = details.product_id
  	GROUP BY details.segment_name, details.product_name
	)
SELECT segment_name, product_name,
	ROUND(100 * product_revenue / SUM(product_revenue) OVER
         (PARTITION BY segment_name), 2) AS product_percent
FROM product_revenue
ORDER BY segment_name, product_percent DESC;
</pre>

Output:
| segment_name | product_name                     | product_percent |
| ------------ | -------------------------------- | --------------- |
| Jacket       | Grey Fashion Jacket - Womens     | 57.03           |
| Jacket       | Khaki Suit Jacket - Womens       | 23.51           |
| Jacket       | Indigo Rain Jacket - Womens      | 19.45           |
| Jeans        | Black Straight Jeans - Womens    | 58.15           |
| Jeans        | Navy Oversized Jeans - Womens    | 24.06           |
| Jeans        | Cream Relaxed Jeans - Womens     | 17.79           |
| Shirt        | Blue Polo Shirt - Mens           | 53.60           |
| Shirt        | White Tee Shirt - Mens           | 37.43           |
| Shirt        | Teal Button Up Shirt - Mens      | 8.98            |
| Socks        | Navy Solid Socks - Mens          | 44.33           |
| Socks        | Pink Fluro Polkadot Socks - Mens | 35.50           |
| Socks        | White Striped Socks - Mens       | 20.18           |

### Question 7. What is the percentage split of revenue by segment for each category?

<pre>
WITH segment_revenue AS (
	SELECT details.category_name, details.segment_name,
    SUM(sales.qty * sales.price) AS segment_revenue
  	FROM balanced_tree.sales AS sales
  		JOIN balanced_tree.product_details AS details
    	ON sales.prod_id = details.product_id
  	GROUP BY details.category_name, details.segment_name
	)
SELECT category_name, segment_name,
	ROUND(100 * segment_revenue / SUM(segment_revenue) OVER
         (PARTITION BY category_name), 2) AS segment_percent
FROM segment_revenue
GROUP BY segment_revenue, category_name, segment_name
ORDER BY category_name, segment_percent DESC;
</pre>

Output:
| category_name | segment_name | segment_percent |
| ------------- | ------------ | --------------- |
| Mens          | Shirt        | 56.87           |
| Mens          | Socks        | 43.13           |
| Womens        | Jacket       | 63.79           |
| Womens        | Jeans        | 36.21           |

### Question 8. What is the percentage split of total revenue by category?

<pre>
SELECT ROUND(100 * SUM(CASE WHEN details.category_id = 1 THEN (sales.qty * sales.price) END) / 
	SUM(qty * sales.price)) AS womens,
    (100 - ROUND(100 * SUM(CASE WHEN details.category_id = 1 THEN (sales.qty * sales.price) END) / 
		 SUM(sales.qty * sales.price))) AS mens
FROM balanced_tree.sales AS sales
JOIN balanced_tree.product_details AS details
	ON sales.prod_id = details.product_id;
</pre>

Output:
| womens | mens |
| ------ | ---- |
| 44     | 56   |
  
### Question 9. What is the total transaction “penetration” for each product?

<pre>
WITH product_txn AS (
    SELECT product_name, COUNT(DISTINCT txn_id) AS num_of_txn
    FROM balanced_tree.sales AS sales
    	JOIN balanced_tree.product_details AS details
  		ON details.product_id = sales.prod_id
    GROUP BY 1
	),
total_txn AS (
    SELECT COUNT(DISTINCT txn_id) AS total_of_txn
    FROM balanced_tree.sales
	)
SELECT product_name, ROUND((num_of_txn::NUMERIC/total_of_txn) *100,2) AS penetration
FROM product_txn, total_txn
ORDER BY 2 DESC;
</pre>

Output:
| product_name                     | penetration |
| -------------------------------- | ----------- |
| Navy Solid Socks - Mens          | 51.24       |
| Grey Fashion Jacket - Womens     | 51.00       |
| Navy Oversized Jeans - Womens    | 50.96       |
| White Tee Shirt - Mens           | 50.72       |
| Blue Polo Shirt - Mens           | 50.72       |
| Pink Fluro Polkadot Socks - Mens | 50.32       |
| Indigo Rain Jacket - Womens      | 50.00       |
| Khaki Suit Jacket - Womens       | 49.88       |
| Black Straight Jeans - Womens    | 49.84       |
| Cream Relaxed Jeans - Womens     | 49.72       |
| White Striped Socks - Mens       | 49.72       |
| Teal Button Up Shirt - Mens      | 49.68       |





Answer the following questions and provide the SQL queries used to find the answer.

    
**Question 1: Which cities and countries have the highest level of transaction revenues on the site?**


SQL Queries:
```SQL
SELECT city FROM (SELECT city, RANK() OVER(ORDER BY transactionRevenue) revenueRank FROM all_sessions
	WHERE city != 'not available in demo dataset' AND city != '(not set)') tbl
WHERE tbl.revenueRank = 1

SELECT country FROM (SELECT country, RANK() OVER(ORDER BY transactionRevenue) revenueRank FROM all_sessions
	WHERE country != '(not set)') tbl
WHERE tbl.revenueRank = 1
```


Answer:
City: Sunnyvale
Country: United States



**Question 2: What is the average number of products ordered from visitors in each city and country?**


SQL Queries:

```SQL
SELECT city, AVG(productQuantity)
FROM all_sessions
WHERE productQuantity IS NOT NULL AND city != '(not set)' AND city != 'not available in demo dataset'
GROUP BY city
ORDER BY city

SELECT country, AVG(productQuantity)
FROM all_sessions
WHERE productQuantity IS NOT NULL AND country != '(not set)'
GROUP BY country
ORDER BY country
```


Answer:
Cities: 1 to 8
Countries: 1 to 10




**Question 3: Is there any pattern in the types (product categories) of products ordered from visitors in each city and country?**


SQL Queries:

```SQL
SELECT city, v2productcategory AS productCategory, SUM(productprice) totalSale, RANK() OVER(PARTITION BY city ORDER BY SUM(productprice) DESC) salesRank FROM all_sessions 
WHERE city != '(not set)' AND city != 'not available in demo dataset' AND v2productcategory != '(not set)'
GROUP BY city, v2productcategory

SELECT country, v2productcategory AS productCategory, SUM(productprice) totalSale, RANK() OVER(PARTITION BY country ORDER BY SUM(productprice) DESC) salesRank FROM all_sessions 
WHERE country != '(not set)' AND v2productcategory != '(not set)'
GROUP BY country, v2productcategory
```


Answer:
It appears men's apparel is popular among all cities and countries




**Question 4: What is the top-selling product from each city/country? Can we find any pattern worthy of noting in the products sold?**


SQL Queries:
```SQL
SELECT tbl.city, (SELECT v2ProductName FROM all_sessions WHERE productsku = tbl.productsku LIMIT 1) FROM 
(SELECT city, productsku, COUNT(*) prodCnt, RANK() OVER(PARTITION BY city ORDER BY COUNT(*) DESC) salesRank FROM all_sessions
WHERE city != '(not set)' AND city != 'not available in demo dataset'
GROUP BY city, productsku) tbl
WHERE tbl.salesRank = 1
ORDER BY tbl.city

SELECT tbl.country, (SELECT v2ProductName FROM all_sessions WHERE productsku = tbl.productsku LIMIT 1) FROM 
(SELECT country, productsku, COUNT(*) prodCnt, RANK() OVER(PARTITION BY country ORDER BY COUNT(*) DESC) salesRank FROM all_sessions
WHERE country != '(not set)'
GROUP BY country, productsku) tbl
WHERE tbl.salesRank = 1
ORDER BY tbl.country
```


Answer:
There are many products that are tied as the top selling product in each city/country.




**Question 5: Can we summarize the impact of revenue generated from each city/country?**

SQL Queries:
```SQL
SELECT city, SUM(totaltransactionrevenue) FROM all_sessions
WHERE totaltransactionrevenue IS NOT NULL AND city != '(not set)' AND city != 'not available in demo dataset'
GROUP BY city

SELECT country, SUM(totaltransactionrevenue) FROM all_sessions
WHERE totaltransactionrevenue IS NOT NULL AND country != '(not set)'
GROUP BY country
```


Answer:
Not enough data to make reasonable conclusions







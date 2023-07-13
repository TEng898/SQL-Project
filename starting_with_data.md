Question 1: Find the best month of sales

SQL Queries:
```SQL
SELECT EXTRACT(MONTH FROM date) as Month, SUM(units_sold * unit_price) totalSales
FROM analytics
WHERE units_sold IS NOT NULL AND unit_price IS NOT NULL
GROUP BY EXTRACT(MONTH FROM date)
ORDER BY Month DESC
LIMIT 1
```
Answer: 
August


Question 2: Find the channel(s) that generates the most sales

SQL Queries:
```SQL
SELECT channelGrouping
FROM
(SELECT channelGrouping, SUM(units_sold * unit_price) totalSales, 
 RANK() OVER(ORDER BY SUM(units_sold * unit_price) DESC) salesRank 
FROM analytics
WHERE channelGrouping != '(Other)'
GROUP BY channelGrouping) tbl
WHERE salesRank = 1
```
Answer:
Referral


Question 3: Find the average time spent on the site before making a purchase

SQL Queries:

```SQL
SELECT AVG(timeonsite)/60
FROM analytics
WHERE units_sold > 0
```
Answer:
20 minutes


Question 4: 

SQL Queries:

Answer:



Question 5: 

SQL Queries:

Answer:

What are your risk areas? Identify and describe them.

Thare are many missing values in the data. Therefore, accuracy of results from queries must take this limitation into consideration. There's also lack of organization of the tables. For example, it's not obvious if there can be any primary key and foreign key relationship between the tables.

In addition, there's a 'time' field in the all_sessions table that is ambiguous. That field will need to be explored further in the future.


QA Process:
Describe your QA process and include the SQL queries used to execute it.

First, make a backup copy of all the tables.
```SQL
SELECT * INTO analytics_bck FROM analytics
```
Second, check if there are any duplicates across the entire table and the percent of distinct values (distinct/total) if there are any (100 = no duplicates).
```SQL
SELECT COUNT(*)::real/(SELECT COUNT(*)::real FROM all_sessions ) total
	FROM (SELECT DISTINCT * FROM all_sessions) noDups
```
Third, check for percentage of null values in individual columns
```SQL
SELECT (SELECT COUNT(*)::real FROM all_sessions WHERE country IS null)/ COUNT(*)::real * 100 AS percentNulls FROM all_sessions 
```
Fourth, get the minimum and maximum values to make sure timestamps, prices, units sold, etc. are not negative, dates are not in the future, any outliers or unexpected values.
```SQL
SELECT MAX(units_sold) as Max FROM analytics;
SELECT MIN(units_sold) as Min FROM analytics;
```
Fifth, get the distribution of distinct and duplicated values. Order the description by ascending will show the non alpha-numeric characters first (after nulls if any) if there are any so they can be easily spotted.
```SQL
SELECT v2ProductCategory, COUNT(*) FROM all_sessions
GROUP BY 1
ORDER BY 1 NULLS FIRST
```
Sixth, specifically check for duplicates.
```SQL
SELECT productSKU, COUNT(*) FROM all_sessions
GROUP BY 1
HAVING COUNT(*) > 1
ORDER BY 1 NULLS FIRST
```

The product sku in the products table appears to be an appropriate column for a primary key since it has no duplicates. Other tables except analytics also has a product sku column so it makes sense for these columns to be foreign keys. However, as the queries below show, there are skus in these columns that do not exit in the sku of the products table. Therefore, there cannot be a relationship between these columns because a PK/FK constraint cannot be applied except for the sales_report table for which there can be a PK/FK relationship, but it does not meaningfully contribute to the analysis of the data, e.g. no dates.
```SQL
SELECT productsku FROM all_sessions WHERE TRIM(productsku) NOT IN (SELECT TRIM(sku) FROM products) --2033
SELECT productsku FROM sales_by_sku WHERE TRIM(productsku) NOT IN (SELECT TRIM(sku) FROM products) --8
SELECT productsku FROM sales_report WHERE TRIM(productsku) NOT IN (SELECT TRIM(sku) FROM products) --0
```
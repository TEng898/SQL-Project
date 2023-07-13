What issues will you address by cleaning the data?

Bad or messay data can result in inaccurate conclusions or worse misleading conclusions which can lead to financial or even catastrophic losses. Data cleaning helps to prevent these side affects and will make the dataset more understandable and easier to work with.

Bad data is data that's incomplete, inaccurate, irrelevant, duplicated or missing. In addition, structual errors like naming conventions, typos, or incompatible data types also contributes to the loss of integrity of the data. Data cleaning addresses these issue.



Queries:
Below, provide the SQL queries you used to clean your data.
```SQL
ALTER TABLE analytics ALTER COLUMN unit_price TYPE real USING CAST(unit_price AS real)
UPDATE analytics SET unit_price = unit_price/1000000
ALTER TABLE analytics ALTER COLUMN date TYPE date USING CAST(date AS date)
ALTER TABLE analytics ALTER COLUMN visitStartTime TYPE timestamp USING to_timestamp(visitStartTime)

ALTER TABLE all_sessions ALTER COLUMN date TYPE date USING CAST(date AS date)
UPDATE all_sessions SET productprice = productprice/1000000
UPDATE all_sessions SET productvariant = TRIM(productvariant)
UPDATE all_sessions SET currencycode = 'USD'
```
# Final-Project-Transforming-and-Analyzing-Data-with-SQL

## Project/Goals
A dataset consisting of tables in csv format is provided and the goal is to import it into a Postgresql database to clean the data, analyze the data to gain some insights, and conduct quality assurance on the data.

## Process
### Analyzed headings and data of csv source files
### Created appropriate data types for columns in the tables
### Imported the csv files to Postgresql database
### Performed casting of data types, corrected erroneous values, imputed missing values
### Created queries to detect duplicated/missing data
### Queried maximum and minimum values of columns to determine outliers
### Created queries to spot check columns for data inconsistencies

## Results
Discovered the analytics table with 4.3 million rows only contains 40% distinct rows. The rest are duplicates. The large amount of duplication would skew the results of the queries. In addition, some rows contains over 99.9% null values, yielding some query results with limited trustworthiness. 

## Challenges 
Lack of meaningful relationships between the data sets. Many missing values in quite a few columns in the tables.

## Future Goals
The data clearning process was quite manual and tedious which introduces the possibility of human errors. In the future, it would be more efficient to use tools that can automate the data cleaning process.

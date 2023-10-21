# Analyzing-American-Baby-Name-Trends
![Hello_My_Name_](https://github.com/aliabdulelah/Analyzing-American-Baby-Name-Trends/assets/129835709/40af9d0d-4851-4eec-baa7-fa1d347600f4)

## Table of contents 



## Project Overview
By using the United States Social Security Administration data from 1920-to 2020, we analyze American baby name shifts. Explore trends, and changes in popular names over time, and compare century-old favorites with recent top names, offering insights relevant to both parents and businesses.




### Data Source
We'll use data published by the U.S. Social Security Administration spanning over a hundred years to understand American baby name tastes, 
[Download here AS CSV file](https://docs.google.com/spreadsheets/d/1cJ636nsZxeEMJ93nnjaLSo1u8LEdcbf0kfFjHB1tvYc/edit#gid=2018594801)




### Tools 
- PostgreSQL - Data Cleaning
- PostgreSQL - Data Analysis





### Data Cleaning / Preparation 

 In the initial data preparation phase, we performed the following tasks:

- Data loading and inspection.
- Handling missing values.
- Data cleaning and formatting.


 


### Exploratory Data Analysis
 EDA involves exploring the American baby name data to answer key questions, such as:

1. Classic American names
2. Timeless or trendy?
3. Top-ranked female names since 1920
4. Picking a baby name
5. The Olivia expansion
6. Many males with the same name
7. Top male names over the years
8. The most years at number one


### Data Analysis
Include some interesting code/features worked with

```sql
SELECT first_name ,
       SUM(num),
    RANK() OVER(ORDER BY SUM(num) DESC) AS name_rank
FROM baby_names
WHERE year >= 1920 AND sex = 'F'
GROUP BY first_name
ORDER BY name_rank ASC
LIMIT 10;
```

```sql
SELECT b.year,
      b.first_name,
      b.num
FROM baby_names AS b
INNER JOIN (SELECT year ,
      MAX(num) AS max_num
FROM baby_names
WHERE sex = 'M'
GROUP BY year) AS subquery
ON b.year = subquery.year AND b.num = subquery.max_num
ORDER BY year DESC
```
```sql
WITH count_top_name AS (SELECT b.year,
      b.first_name,
      b.num
FROM baby_names AS b
INNER JOIN (SELECT year ,
      MAX(num) AS max_num
FROM baby_names
WHERE sex = 'M'
GROUP BY year) AS subquery
ON b.year = subquery.year AND b.num = subquery.max_num
ORDER BY year DESC)

SELECT first_name,
       COUNT(first_name) AS count_top_name
FROM count_top_name
GROUP BY first_name
ORDER BY COUNT(first_name) DESC
```

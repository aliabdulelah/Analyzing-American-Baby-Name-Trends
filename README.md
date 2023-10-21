# Analyzing-American-Baby-Name-Trends
![Hello_My_Name_](https://github.com/aliabdulelah/Analyzing-American-Baby-Name-Trends/assets/129835709/40af9d0d-4851-4eec-baa7-fa1d347600f4)

## Table of contents 
- [Project Overview](#project-Overview)
- [Data Source](#Data-Source)
- [Tools](#Tools)
- [ Data Cleaning / Preparation](#Data-Cleaning-/-Preparation)
- [Exploratory Data Analysis](#Exploratory-Data-Analysis)

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

1. [Classic American names](#Classic-American-names)
2. [Timeless or trendy?](#Timeless-or-trendy?)
3. [Top-ranked female names since 1920](#Top-ranked-female-names-since-1920)
4. [Picking a baby name](#Picking-a-baby-name)
5. [The Olivia expansion](#The-Olivia-expansion)
6. [Many males with the same name](#Many-males-with-the-same-name)
7. [Top male names over the years](#Top-male-names-over-the-years)
8. [The most years at number one](#The-most-years-at-number-one)


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

### Results/Findings

1- Classic American names
After conducting an analysis of classic American names, the dataset reveals the frequency of occurrences for each name, indicating that 'James' is the most common, followed closely by 'John' and 'William'

| first_name | sum      |
| ---------- | -------- |
| James      | 4748138  |
| John       | 4510721  |
| William    | 3614424  |
| David      | 3571498  |
| Joseph     | 2361382  |
| Thomas     | 2166802  |
| Charles    | 2112352  |
| Elizabeth  | 1436286  |


2- Timeless or trendy?

Upon analyzing the dataset regarding the timelessness and trendiness of American names, it was found that there are 547 records in total. The table below showcases a sample of these records and their corresponding popularity types.

| first_name | sum    | popularity_type |
| ---------- | ------ | --------------- |
| Aaliyah    | 15870  | Trendy          |
| Aaron      | 530592 | Semi-classic    |
| Abigail    | 338485 | Semi-trendy     |
| Adam       | 497293 | Semi-trendy     |
| Addison    | 107433 | Trendy          |
| Adrian     | 147741 | Semi-trendy     |
| Aidan      | 68566  | Trendy          |
| Aiden      | 216194 | Trendy          |


3- Top-ranked female names since 1920

Through an analysis spanning from 1920 to the present, the following table represents the top-ranked female names, based on their frequency of occurrence within the specified period.

| first_name | sum     | name_rank |
| ---------- | ------- | --------- |
| Mary       | 3215850 | 1         |
| Patricia   | 1479802 | 2         |
| Elizabeth  | 1436286 | 3         |
| Jennifer   | 1404743 | 4         |
| Linda      | 1361021 | 5         |
| Barbara    | 1343901 | 6         |
| Susan      | 1025728 | 7         |
| Jessica    | 994210  | 8         |
| Lisa       | 920119  | 9         |
| Betty      | 893396  | 10        |


4- Picking a baby name 
Perhaps a friend has heard of our work analyzing baby names and would like help choosing a name for her baby, a girl. She doesn't like any of the top-ranked names we found in the previous task.

She's set on a traditionally female name ending in the letter 'a' since she's heard that vowels in baby names are trendy. She's also looking for a name that has been popular in the years since 2015.

Let's see what we can do to find some options for this friend!

| first_name |
| ---------- |
| Olivia     |
| Emma       |
| Ava        |
| Sophia     |
| Isabella   |
| Mia        |
| Amelia     |
| Ella       |
| Sofia      |
| Camila     |
| Aria       |
| Victoria   |
| Layla      |
| Nora       |
| Mila       |
| Luna       |
| Stella     |
| Gianna     |
| Aurora     |


5- The Olivia expansion
Based on the results in the previous task, we can see that Olivia is the most popular female name ending in 'A' since 2015. When did the name Olivia become so popular?

| year | first_name | num  | cumulative_olivias |
| ---- | ---------- | ---- | ------------------ |
| 1991 | Olivia     | 5601 | 5601               |
| 1992 | Olivia     | 5809 | 11410              |
| 1993 | Olivia     | 6340 | 17750              |
| 1994 | Olivia     | 6434 | 24184              |
| 1995 | Olivia     | 7624 | 31808              |
| 1996 | Olivia     | 8124 | 39932              |
| 1997 | Olivia     | 9477 | 49409              |
| 1998 | Olivia     | 10610| 60019              |
| 1999 | Olivia     | 11255| 71274              |
| 2000 | Olivia     | 12852| 84126              |
| 2001 | Olivia     | 13977| 98103              |
| 2002 | Olivia     | 14630| 112733             |
| 2003 | Olivia     | 16152| 128885             |
| 2004 | Olivia     | 16106| 144991             |
| 2005 | Olivia     | 15694| 160685             |
| 2006 | Olivia     | 15501| 176186             |
| 2007 | Olivia     | 16584| 192770             |
| 2008 | Olivia     | 17084| 209854             |
| 2009 | Olivia     | 17438| 227292             |
| 2010 | Olivia     | 17029| 244321             |
| 2011 | Olivia     | 17327| 261648             |
| 2012 | Olivia     | 17320| 278968             |
| 2013 | Olivia     | 18439| 297407             |
| 2014 | Olivia     | 19823| 317230             |
| 2015 | Olivia     | 19710| 336940             |
| 2016 | Olivia     | 19380| 356320             |
| 2017 | Olivia     | 18744| 375064             |
| 2018 | Olivia     | 18011| 393075             |
| 2019 | Olivia     | 18508| 411583             |
| 2020 | Olivia     | 17535| 429118             |


6- Many males with the same name

Wow, Olivia has had a meteoric rise! Let's take a look at traditionally male names now. We saw in the first task that there are nine traditionally male names given to at least 5,000 babies every single year in our 101-year dataset! Those names are classics, but showing up in the dataset every year doesn't necessarily mean that the timeless names were the most popular. Let's explore popular male names a little further.

In the next two tasks, we will build up to listing every year along with the most popular male name in that year. This presents a common problem: how do we find the greatest X in a group? Or, in the context of this problem, how do we find the male name given to the highest number of babies in a year?

| year | max_num |
| ---- | ------- |
| 1970 | 85291   |
| 2000 | 34483   |
| 1947 | 94764   |
| 1962 | 85041   |
| 1975 | 68451   |
| 1980 | 68704   |
| 1931 | 60518   |
| 1981 | 68776   |
| 2013 | 18266   |
| 1972 | 71401   |
| 1956 | 90665   |
| 2007 | 24292   |
| 1948 | 88589   |
| 1984 | 67745   |
| 1957 | 92718   |
| 1961 | 86917   |
| 2002 | 30579   |
| 1925 | 60897   |
| 1992 | 54397   |
| 2008 | 22603   |
| 1958 | 90564   |
| 1971 | 77599   |
| 1985 | 64924   |
| 1926 | 61130   |
| 1988 | 64150   |
| 1929 | 59804   |
| 1963 | 83778   |
| 1928 | 60703   |
| 2003 | 29643   |
| 1930 | 62149   |
| 1951 | 87261   |
| 1940 | 62476   |
| 1982 | 68244   |
| 1920 | 56914   |
| 1999 | 35367   |
| 1952 | 87063   |
| 2020 | 19659   |
| 1946 | 87439   |
| 1968 | 81995   |
| 1996 | 38365   |
| 2005 | 25837   |
| 1923 | 57469   |
| 2009 | 21184   |
| 1924 | 60801   |
| 1954 | 88576   |
| 2004 | 27886   |
| 1938 | 62269   |
| 1942 | 77174   |
| 1966 | 79990   |
| 1998 | 36616   |
| 1974 | 67580   |
| 1949 | 86865   |
| 1990 | 65302   |
| 1995 | 41399   |
| 1973 | 67842   |
| 1927 | 61671   |
| 1941 | 66743   |
| 1977 | 67609   |
| 2001 | 32554   |
| 1997 | 37549   |
| 2014 | 19319   |
| 1965 | 81021   |
| 1935 | 56522   |
| 1944 | 76954   |
| 1994 | 44472   |
| 2016 | 19154   |
| 1960 | 85933   |
| 1987 | 63654   |
| 1978 | 67157   |
| 2018 | 19924   |
| 2006 | 24850   |
| 1921 | 58215   |
| 1993 | 49554   |
| 1964 | 82642   |
| 1943 | 80274   |
| 1937 | 61842   |
| 1986 | 64224   |
| 1953 | 86247   |
| 1959 | 85224   |
| 1976 | 66947   |
| 1989 | 65399   |
| 2012 | 19088   |
| 2011 | 20378   |
| 2019 | 20555   |
| 1955 | 88372   |
| 1939 | 59653   |
| 1979 | 67742   |
| 1991 | 60793   |
| 2017 | 18824   |
| 1969 | 85201   |
| 2015 | 19650   |
| 2010 | 22139   |
| 1932 | 59265   |
| 1967 | 82440   |
| 1922 | 57280   |
| 1933 | 54223   |
| 1934 | 55834   |
| 1936 | 58499   |
| 1945 | 74460   |
| 1950 | 86229   |
| 1983 | 68010   |



7- Top male names over the years

In the previous task, we found the maximum number of babies given any one male name in each year. Incredibly, the most popular name each year varied from being given to less than 20,000 babies to being given to more than 90,000!

| year | first_name | num   |
| ---- | ---------- | ----- |
| 2020 | Liam       | 19659 |
| 2019 | Liam       | 20555 |
| 2018 | Liam       | 19924 |
| 2017 | Liam       | 18824 |
| 2016 | Noah       | 19154 |
| 2015 | Noah       | 19650 |
| 2014 | Noah       | 19319 |
| 2013 | Noah       | 18266 |
| 2012 | Jacob      | 19088 |
| 2011 | Jacob      | 20378 |
| 2010 | Jacob      | 22139 |
| 2009 | Jacob      | 21184 |
| 2008 | Jacob      | 22603 |
| 2007 | Jacob      | 24292 |
| 2006 | Jacob      | 24850 |
| 2005 | Jacob      | 25837 |
| 2004 | Jacob      | 27886 |
| 2003 | Jacob      | 29643 |
| 2002 | Jacob      | 30579 |
| 2001 | Jacob      | 32554 |
| 2000 | Jacob      | 34483 |
| 1999 | Jacob      | 35367 |
| 1998 | Michael    | 36616 |
| 1997 | Michael    | 37549 |
| 1996 | Michael    | 38365 |
| 1995 | Michael    | 41399 |
| 1994 | Michael    | 44472 |
| 1993 | Michael    | 49554 |
| 1992 | Michael    | 54397 |
| 1991 | Michael    | 60793 |
| 1990 | Michael    | 65302 |
| 1989 | Michael    | 65399 |
| 1988 | Michael    | 64150 |
| 1987 | Michael    | 63654 |
| 1986 | Michael    | 64224 |
| 1985 | Michael    | 64924 |
| 1984 | Michael    | 67745 |
| 1983 | Michael    | 68010 |
| 1982 | Michael    | 68244 |
| 1981 | Michael    | 68776 |
| 1980 | Michael    | 68704 |
| 1979 | Michael    | 67742 |
| 1978 | Michael    | 67157 |
| 1977 | Michael    | 67609 |
| 1976 | Michael    | 66947 |
| 1975 | Michael    | 68451 |
| 1974 | Michael    | 67580 |
| 1973 | Michael    | 67842 |
| 1972 | Michael    | 71401 |
| 1971 | Michael    | 77599 |
| 1970 | Michael    | 85291 |
| 1969 | Michael    | 85201 |
| 1968 | Michael    | 81995 |
| 1967 | Michael    | 82440 |
| 1966 | Michael    | 79990 |
| 1965 | Michael    | 81021 |
| 1964 | Michael    | 82642 |
| 1963 | Michael    | 83778 |
| 1962 | Michael    | 85041 |
| 1961 | Michael    | 86917 |
| 1960 | David      | 85933 |
| 1959 | Michael    | 85224 |
| 1958 | Michael    | 90564 |
| 1957 | Michael    | 92718 |
| 1956 | Michael    | 90665 |
| 1955 | Michael    | 88372 |
| 1954 | Michael    | 88576 |
| 1953 | Robert     | 86247 |
| 1952 | James      | 87063 |
| 1951 | James      | 87261 |
| 1950 | James      | 86229 |
| 1949 | James      | 86865 |
| 1948 | James      | 88589 |
| 1947 | James      | 94764 |
| 1946 | James      | 87439 |
| 1945 | James      | 74460 |
| 1944 | James      | 76954 |
| 1943 | James      | 80274 |
| 1942 | James      | 77174 |
| 1941 | James      | 66743 |
| 1940 | James      | 62476 |
| 1939 | Robert     | 59653 |
| 1938 | Robert     | 62269 |
| 1937 | Robert     | 61842 |
| 1936 | Robert     | 58499 |
| 1935 | Robert     | 56522 |
| 1934 | Robert     | 55834 |
| 1933 | Robert     | 54223 |
| 1932 | Robert     | 59265 |
| 1931 | Robert     | 60518 |
| 1930 | Robert     | 62149 |
| 1929 | Robert     | 59804 |
| 1928 | Robert     | 60703 |
| 1927 | Robert     | 61671 |
| 1926 | Robert     | 61130 |
| 1925 | Robert     | 60897 |
| 1924 | Robert     | 60801 |
| 1923 | John       | 57469 |
| 1922 | John       | 57280 |
| 1921 | John       | 58215 |
| 1920 | John       | 56914 |


8- The most years at number one

Noah and Liam have ruled the roost in the last few years, but if we scroll down in the results, it looks like Michael and Jacob have also spent a good number of years as the top name! Which name has been number one for the largest number of years? Let's use a common table expression to find out.

| first_name | count_top_name |
| ---------- | -------------- |
| Michael    | 44             |
| Robert     | 17             |
| Jacob      | 14             |
| James      | 13             |
| Noah       | 4              |
| John       | 4              |
| Liam       | 4              |
| David      | 1              |



# <p align="center">SUPER MARKET SALES ANALYSIS!</p>
# <p align="center">![image](https://github.com/AhamedSahil/Project-1/assets/164605797/25231f93-6fdf-40c6-a2c0-3d49cdcb7241)</p>

This repository contains code and resources for analyzing sales data from a supermarket. The analysis is performed using Sql on  mysqlworkbech and Excel. The purpose of this project is to gain insights into sales trends, customer behavior.

**Tools:-** Excel,Mysql

[Datasets Used](https://www.kaggle.com/datasets/aungpyaeap/supermarket-sales)

[SQL Analysis (Code)](supermarket_sales_projects.sql)

[Ppt presentation](sql_prjct.pptx)

### Data preparation

- changing the data types of date column 

```mysql 
update supermarket_sales.supermarket_sales set _date = str_to_date(_date,"%m/%d/%Y");
```

- adding a new colunm as sentiment fro rating purpose

```mysql
alter table supermarket_sales add column sentiment varchar(25) ;select count(sentiment) from supermarket_sales where sentiment is null ;
```

- we have fill some categorical data like 'negetive' if rating between 0 to 4,'netural' when rating between 4 to 6,'Good' when rating between 6 to 8 and 'Very Good' when rating betweeen 8 to 11 in sentiment column with the help of  rating column
```mysql 
start transaction;
savepoint sentiment_insertion;update supermarket_sales set rating = round(rating);
update supermarket_sales set sentiment= case when rating between 0 and 4 then "negetive"
when rating between 4 and 6 then "neutral"                                        
when rating between 6 and 8 then "Good"                                        
when rating between 8 and 11 then "very Good"
end ;
```
Result:

![Img_1](https://github.com/Aathimuthu25/SUPER-MARKET-SALES-ANALYSIS/assets/158067286/29f17ccd-372b-45df-a83a-9d0a5e71295f)

- coverted  invoice column into primary key
```mysql
ALTER TABLE supermarket_sales.supermarket_sales
CHANGE COLUMN `Invoice ID` `Invoice ID` VARCHAR(100) NOT NULL,
ADD PRIMARY KEY (`Invoice ID`);
```

### OBJECTIVE OF THE STUDY

- To visualize how explanatory variable i.e., Branch, Customer type, Gender, Product line and Payment type affect to study variable sales.

- To check Main and Interaction effect of explanatory Variable on sales.
 
- Fitting appropriate Time Series Model and analyzing study variable sales.

### Contents

- Sales Performance Analysis

- Average Profit Analysis

- Sentiment Analysis

- Quantity of products sold in each city /branch

- Top selling product

### The questions we have apply on our dataset

- Finding the total sales with respect to product line and the top selling product 
```mysql
select product_line,sum(quantity) as total_sell from supermarket_sales group by product_line order by total_sell desc;
```
Result:

![Img_2](https://github.com/Aathimuthu25/SUPER-MARKET-SALES-ANALYSIS/assets/158067286/ecfe5f9f-1353-40f7-b748-4f2402e7f922)

### <p align="center">Top Selling Product!</p>
![Img_3](https://github.com/Aathimuthu25/SUPER-MARKET-SALES-ANALYSIS/assets/158067286/e7c17ebb-3ac3-4191-a90b-7ed2d3ea98ae)



- finding the most profitable productline.
```mysql
select product_line,sum(gross_income) as total_profit from supermarket_sales group by product_line order by total_profit desc;#top profitable product is food and beverages.
```
Result:

![Img_4](https://github.com/Aathimuthu25/SUPER-MARKET-SALES-ANALYSIS/assets/158067286/6097a0b7-c8ef-49ae-802e-8b7b608812d8)


- finding out the change in sells by their month respective to proudctline
```mysql
select _month,product_line,sum(total_sales) over(partition by product_line order by _month ) as profit_data from (select extract(month from _date) as _month,product_line,sum(gross_income) total_sales from supermarket_sales  Group by _month,product_line order by total_sales ) as data_;
```
Result:

![Img_5](https://github.com/Aathimuthu25/SUPER-MARKET-SALES-ANALYSIS/assets/158067286/b598f4c1-4238-4d43-933e-c13ff596d362)


- Finding the avg profit  with respective to proudctline

```mysql
select product_line, avg(gross_income) as Avg_profit from supermarket_sales group by product_line;
```
Result:

![Img_6](https://github.com/Aathimuthu25/SUPER-MARKET-SALES-ANALYSIS/assets/158067286/9d8caed7-8f1a-41c1-8ba6-0485c72dcbac)


- Finding out sum of quantity by respictive City and branch irrespictive to productline
```mysql
select City,branch,sum(quantity) from supermarket_sales group by city,branch;
```
Result:

![Img_7](https://github.com/Aathimuthu25/SUPER-MARKET-SALES-ANALYSIS/assets/158067286/bff1a0a0-e54f-4856-973d-68f78ac9b16a)


- Finding out the top most ratings with respect to Sentiment
```mysql
select sentiment,count(sentiment) as total_sentiment  from supermarket_sales group by sentiment order by total_sentiment desc;
```
Result:

![Img_8](https://github.com/Aathimuthu25/SUPER-MARKET-SALES-ANALYSIS/assets/158067286/b22008b1-8276-4da4-a2d8-f6eb9ca52c6f)


 ![Img_9](https://github.com/Aathimuthu25/SUPER-MARKET-SALES-ANALYSIS/assets/158067286/7175dcb7-85f0-4069-9820-aac9a78e6e37)



- Finding out the Top 5  very good rating products
```mysql
SELECT city,product_line,sentiment, MAX(rating) AS top_rating
FROM supermarket_sales
GROUP BY city,product_line,sentiment
ORDER BY top_rating DESC
LIMIT 5;
```
Result:

![Img_10](https://github.com/Aathimuthu25/SUPER-MARKET-SALES-ANALYSIS/assets/158067286/0df0e00c-3913-4962-ac89-ef1d8f44f2a2)

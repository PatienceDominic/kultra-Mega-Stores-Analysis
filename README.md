# PROJECCT TOPIC
kultra-Mega-Stores-Analysis
 
## PROJEC OVERVIEW
This project involves analyzing historical sales data for Kultra Mega Stores (KMS), a Lagos-based supplier of office supplies and furniture. The analysis focuses on the Abuja division, using order data from the year 2009 to 2012 to uncover trends, performance metrics, and strategic insight.

## DATA SOURCE
Internal order dataset shared by KMS Business Manager (Excel file containing orders from 2009–2012 for Abuja division)

## TOOL USED
SQL for querrying the data to uncover trends and performance metrics

## DATA CLEANING AND PREPARATION
In this aspect the data was loaded and inspected, handling missing data and formating.

## Exploratory Data Analysis 
 This involves exploring the data to answer questions like
  - Which product category had the highest sales?
  - What are the top 3 and bottom 3 regions in terms of sales?
  -  What were the total sales of appliance in ontario?
  -  KMS incured the most shipping cost using which shipping method?
  -   Who are the most valuable customers, and what products or services do they typically purchase?
  -   Which corporate customer placed the most number of orders in 2009-2012?
  -    Which customers returned items and what segment do they belong to?


## DATA ANALYSIS	

---- QUESTION 1
----- Which product category had the highest sales?

``` SQL
select max(Product_Category) AS highest_sales
from [dbo].[KMS Sql Case Study]
```
------QUESTION 2
------ What are the top 3 and bottom 3 regions in terms of sales?
	
 ```SQL
 select 
	  top 3 Region,
	  sum(sales) AS total_sales
	from [dbo].[KMS Sql Case Study]
	group by Region
	order by sum(sales) desc
	select top 3 Region,
	   sum(sales) AS total_sales
	from [dbo].[KMS Sql Case Study]
	group by Region
	order by sum(sales) asc
 ```

------QUESTION 3
------- What were the total sales of appliance in ontario
 
 ```SQL
select Region, round(sum(sales),2) AS total_sales_of_appliances
 from [dbo].[KMS Sql Case Study]
 where Product_Sub_Category = 'Appliances'
 and Province = 'Ontario'
 group by Region ```

 -----QUESTION 4----
 ------Advice the managament of KMS on what to do to increase the revenue from the bottom 10 customers

``` SQL
select top 10
	Customer_Name,
	sum(Sales) AS total_sales
from
[dbo].[KMS Sql Case Study]
group by Customer_Name
order by total_sales asc```

--------QUESTION 5
--------KMS incured the most shipping cost using which shipping method?

``` SQL
select top 1
  Ship_Mode,
  sum(Shipping_Cost) AS
  total_shipping_cost
from 
[dbo].[KMS Sql Case Study]
group by Ship_Mode
order by total_shipping_cost desc
```

------QUESTION 6
----- Who are the most valuable customers, and what products or services do they typically purchase?

``` SQL
with customer_totals AS(
select 
Customer_Name,
sum(Order_Quantity) AS total_quantity
from 
[dbo].[KMS Sql Case Study]
group by Customer_Name
),
top_customers AS(
select
Customer_Name
from customer_totals
)
select
KMS.Customer_Name,
KMS.Product_Name,
KMS.Product_Category,
sum(KMS.Order_Quantity) AS
total_purchased
 from  [dbo].[KMS Sql Case Study] As KMS
 join top_customers tc on
 KMS.Customer_Name = tc.Customer_Name
 group by KMS.Customer_Name,
 KMS.Product_Name, KMS.Product_Category
 order by total_purchased desc
```

 ------QUESTION 7
 ------ Which small business customer had the highest sales?
``` SQL
 select top 1
	Customer_Name,
	sum(Sales) AS highest_sales
from
	[dbo].[KMS Sql Case Study]
where
	Customer_Segment = 'Small Business'
group by Customer_Name
order by highest_sales desc
```

-------QUESTION 8
-------Which corporate customer placed the most number of orders in 2009-2012?

``` SQL
select top 1
	Customer_Name,
	count(distinct Order_ID) AS Total_Orders
from
	[dbo].[KMS Sql Case Study]
where
	Customer_Segment = 'Corporate'
	AND Order_Date BETWEEN '2012-01-01' AND '2019-01-01'
group by
	Customer_Name
order by
	Total_Orders desc
```

-------QUESTION 9-----
------Which consumer customer was the most profitable one?

```SQL
select top 1
	Customer_Name,
round(sum(Profit),2) AS total_profit
from
	[dbo].[KMS Sql Case Study]
where
	Customer_Segment = 'Consumer'
group by
	Customer_Name
order by
	total_profit desc
```

------ QUESTION 10
 ------ Which customers returned items and what segment do they belong to?

 ```SQL
select
	KMS.Customer_Name,
	KMS.Customer_Segment
from
	[dbo].[KMS Sql Case Study] AS KMS
join
	Order_Status AS os 
	on KMS.Order_ID = os.Order_ID
where
	os.Status = 'Returned'
```


	------QUESTION 11
	-------If the delivery truck is the most economical but the slowest shipping method and Express Air is the fastest but the most expensive one, do
	----you think the company appropriately spent shipping costs based on the Order Priority? Explain your answer.
 
 ```SQL
select
	Order_Priority,
	Ship_Mode,
	count(Order_ID) AS
Order_Count,
	round(sum(Sales - Profit),2) AS
	Estimated_Shipping_Cost,
	avg(datediff(day, Order_Date, Ship_Date)) AS avg_Ship_days
from [dbo].[KMS Sql Case Study]
  group by Order_Priority,Ship_Mode
  order by Order_Priority,Ship_Mode desc
```

![Screenshot (1)](https://github.com/user-attachments/assets/8c32044f-3768-4776-bc52-1c479a1adfa9)

# kultra-Mega-Stores-Analysis
Sales and other data analysis for Kultra Mega Stores, identifying trends and key metrics.

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

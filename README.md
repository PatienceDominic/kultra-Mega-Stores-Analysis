# PROJECT TOPIC
kultra-Mega-Stores-Analysis
 
### PROJECT OVERVIEW
This project involves analyzing historical sales data for Kultra Mega Stores (KMS), a Lagos-based supplier of office supplies and furniture. The analysis focuses on the Abuja division, using order data from the year 2009 to 2012 to uncover trends, performance metrics, and strategic insight.

### DATA SOURCE
Internal order dataset shared by KMS Business Manager (Excel file containing orders from 2009–2012 for Abuja division)

### TOOL USED
Excel for data cleaning and uploading
SQL Server for querrying the data to uncover trends and performance metrics

### DATA CLEANING AND PREPARATION
In this aspect the data was loaded and inspected, handling missing data and formating.

### Exploratory Data Analysis 
 This involves exploring the data to answer questions like
  - Which product category had the highest sales?
  - What are the top 3 and bottom 3 regions in terms of sales?
  - What were the total sales of appliance in ontario?
  - KMS incured the most shipping cost using which shipping method?
  - Who are the most valuable customers, and what products or services do they typically purchase?
  - Which corporate customer placed the most number of orders in 2009-2012?
  - Which customers returned items and what segment do they belong to?


### DATA ANALYSIS	

---- QUESTION 1
----- Which product category had the highest sales?

``` SQL
select max(Product_Category) AS highest_sales
from [dbo].[KMS Sql Case Study]

**Technology**
```
https://github.com/PatienceDominic/kultra-Mega-Stores-Analysis/blob/main/Screenshot%20(1).png

Insight...... The product_category with the highest sales is technology

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

```sql
Top 3 region in terms of sales
| Region   | Sales (in units)    |
|----------|---------------------|
| West     | 3,597,549.27        |
| Ontario  | 3,063,212.48        |
| Prarie   | 2,837,304.61        |

Botttom 3 regions in terms of sales
| Region                | Sales (in units)    |
|------------------------|---------------------|
| Nunavut               | 116,376.48          |
| Northwest Territories | 800,847.33          |
| Yukon                 | 975,867.38          |

```

------QUESTION 3
------- What were the total sales of appliance in ontario
 
 ```SQL
select Region, round(sum(sales),2) AS total_sales_of_appliances
 from [dbo].[KMS Sql Case Study]
 where Product_Sub_Category = 'Appliances'
 and Province = 'Ontario'
 group by Region

Ontario — $202,346.84

The 'Appliance' category in the 'Ontario' region generated a total of $202,346.84 

```

 -----QUESTION 4----
 ------Advice the managament of KMS on what to do to increase the revenue from the bottom 10 customers

``` SQL
select top 10
	Customer_Name,
	sum(Sales) AS total_sales
from
[dbo].[KMS Sql Case Study]
group by Customer_Name
order by total_sales asc

 Top 10 Customers by Total Sales (Ascending Order)

| Rank | Customer Name        | Total Sales ($)     |
|------|----------------------|----------------------|
| 1    | Jeremy Farry         | 85.72                |
| 2    | Natalie DeCherney    | 125.90               |
| 3    | Nicole Fjeld         | 153.03               |
| 4    | Katrina Edelman      | 180.76               |
| 5    | Dorothy Dickinson    | 198.08               |
| 6    | Christine Kargatis   | 293.22               |
| 7    | Eric Murdock         | 343.33               |
| 8    | Chris McAfee         | 350.18               |
| 9    | Rick Huthwaite       | 415.82               |
| 10   | Mark Hamilton        | 450.99               |

```

My advice to the management: The management has to increase revenue coming from customers like Jeremy Farry, Natahlie Decherney and others considering targeted approach focusing on engagement and value proposition
 The management can send  messages or emails to individuals, offer discount on their next purchase in order to entice them.
 carry out a survey reaching out to these customers with low purchase to know their reasons for low purchase. understanding the 'why' their low spending and offering, then KMS can convert these customers to more vslusble customers.

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

 Shipping Method with the Highest Total Cost

| Shipping Method | Total Shipping Cost ($) |
|------------------|--------------------------|
| Delivery Truck   | 51,971.94                |

Insight  
KMS incurred the most shipping cost using the Delivery Truck shipping method.

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

 Top Small Business Customer by Sales

| Customer Name | Total Sales ($)   |
|----------------|-------------------|
| Dennis Kane    | 75,967.59         |

Insight:  
Among small business customers, Dennis Kane generated the highest total sales.
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

### Top Corporate Customer by Number of Orders (2009–2012)

| Customer Name | Number of Orders |
|----------------|------------------|
| Roy Skaria     | 8                |

Insight:  
Between '2009 and 2012', Roy Skaria placed the highest number of orders among corporate customers, totaling 8 orders.

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

### Most Profitable Consumer Customer

| Customer Name | Total Profit ($) |
|----------------|------------------|
| Emily Phan     | 34,005.44        |

Insight:  
'Emily Phan' was the most profitable customer in the Consumer segment, generating $34,005.44 in profit.

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

Analysis: Was Shipping Cost Appropriately Spent Based on Order Priority?

| Order Priority  | Shipping Method  | Number of Orders | Shipping Cost ($) | Priority Level |
|------------------|------------------|------------------|--------------------|----------------|
| Critical         | Delivery Truck   | 228              | 1,218,333.85       | 1 (Highest)    |
| Critical         | Regular Air      | 1180             | 1,118,847.32       | 1              |
| Critical         | Express Air      | 200              |   197,985.04       | 1              |
| High             | Delivery Truck   | 248              | 1,338,507.99       | 1              |
| High             | Regular Air      | 1308             | 1,306,958.06       | 1              |
| High             | Express Air      | 212              |   201,587.18       | 1              |
| Medium           | Delivery Truck   | 205              |   969,386.53       | 1              |
| Medium           | Regular Air      | 1225             | 1,260,247.42       | 1              |
| Medium           | Express Air      | 201              |   244,815.39       | 1              |
| Low              | Delivery Truck   | 250              | 1,313,686.18       | 3              |
| Low              | Regular Air      | 1280             | 1,360,358.02       | 4 (Lowest)     |
| Low              | Express Air      | 190              |   190,542.17       | 4              |
| Not Specified    | Delivery Truck   | 215              | 1,080,831.59       | 1              |
| Not Specified    | Regular Air      | 1277             | 1,257,789.80       | 1              |
| Not Specified    | Express Air      | 180              |   194,378.26       | 1              |

_I will say that the company did not fully align shipping choices with order priority:

- Critical and High Priority Orders: These received an appropriate mix of faster (Express Air and Regular Air) and economical (Delivery Truck) methods. The presence of Express Air for Critical orders suggests good responsiveness.

- Low Priority Orders: These still had significant spending on Regular Air ($1.36M) and Delivery Truck ($1.31M), and even some Express Air ($190K)—which contradicts the need for economy over speed.

- Medium and Not Specified Priorities: These had consistent spending across all shipping methods, including costly options like Express Air, suggesting a lack of stricter prioritization.


To improve cost-efficiency:
- Restrict Express Air to Critical orders only.
- Use Delivery Truck or Regular Air for Low priority orders to reduce cost.
- Implement automated shipping selection rules based on priority levels.
With this KMS would optimize shipping costs without compromising service quality.

 
```

![Screenshot (1)](https://github.com/user-attachments/assets/8c32044f-3768-4776-bc52-1c479a1adfa9)
![Screenshot (9)](https://github.com/user-attachments/assets/c7cb8557-13b2-489c-966e-f9f45b8c8011)
![Screenshot (12)](https://github.com/user-attachments/assets/77eaed6a-8840-45b8-8ed4-2f5bdeeea1a8)
![Screenshot (11)](https://github.com/user-attachments/assets/dc424304-c288-4651-b003-e7b7d887ab8c)
![Screenshot (4)](https://github.com/user-attachments/assets/897c4a89-9499-426f-a98f-512411c8e362)
![Screenshot (5)](https://github.com/user-attachments/assets/8ad07f09-0c1d-4c28-9d6c-726d8cf77f15)
![Screenshot (14)](https://github.com/user-attachments/assets/b4054710-dd36-48d9-a736-3cc222def7d3)
![Screenshot (7)](https://github.com/user-attachments/assets/d4147288-a5cc-4157-a013-3dc87fb7020e)
![Screenshot (15)](https://github.com/user-attachments/assets/5fbe97a8-a714-4be0-a8d8-071fefeb6bd6)
![Screenshot (6)](https://github.com/user-attachments/assets/470492bc-ac08-49b1-af1a-4e1a7a68175f)
![Screenshot (8)](https://github.com/user-attachments/assets/046ebd67-e731-46eb-9576-d9694c6ea4c6)

Recommendation:

- KMS has to enforce Shipping Policy based on Order Priority

- Automate shipping method selection using business rules

- Reserve Express Air for Critical or High priority orders

- Use Delivery Truck for Low or Medium priority to reduce cost

 Conclusion
- By analyzing customer behavior, product performance, and shipping trends, KMS can:

- Focus on high-performing regions and customers

- Optimize shipping costs through smarter logistics

- Boost revenue via customer segmentation strategies

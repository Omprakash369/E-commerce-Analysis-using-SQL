# E-commerce-Analysis-using-SQL
ABC is a globally renowned brand and a prominent retailer in the United States. ABC makes itself a preferred shopping destination by offering outstanding value, inspiration, innovation and an exceptional guest experience that no other retailer can deliver.

This particular business case focuses on the operations of ABC in Brazil and provides insightful information about 100,000 orders placed between 2016 and 2018. The dataset offers a comprehensive view of various dimensions including the order status, price, payment and freight performance, customer location, product attributes, and customer reviews.

By analyzing this extensive dataset, it becomes possible to gain valuable insights into ABC's operations in Brazil. The information can shed light on various aspects of the business, such as order processing, pricing strategies, payment and shipping efficiency, customer demographics, product characteristics, and customer satisfaction levels.

---------------------------------------------------------------------------------

The data is available in 8 csv files:

<img width="506" alt="image" src="https://github.com/user-attachments/assets/08d442ea-5ab4-41cc-9b24-d96cfd1fd39b">

The data is available in 8 csv files:

1.customers.csv <br />
2.sellers.csv <br />
3.order_items.csv <br />
4.geolocation.csv <br />
5.payments.csv <br />
6.reviews.csv <br />
7.orders.csv <br />
8.products.csv <br />

##  Import the dataset and do usual exploratory analysis steps like checking the structure & characteristics of the dataset: <br />
Data type of all columns in the "customers" table. <br />
Get the time range between which the orders were placed. <br />
Count the Cities & States of customers who ordered during the given period. <br />
## In-depth Exploration:
Is there a growing trend in the no. of orders placed over the past years?<br />
Can we see some kind of monthly seasonality in terms of the no. of orders being placed?<br />
During what time of the day, do the Brazilian customers mostly place their orders? (Dawn, Morning, Afternoon or Night)<br />
0-6 hrs : Dawn <br />
7-12 hrs : Mornings <br />
13-18 hrs : Afternoon <br />
19-23 hrs : Night <br />
## Evolution of E-commerce orders in the Brazil region:
Get the month on month no. of orders placed in each state. <br />
How are the customers distributed across all the states? <br />
## Impact on Economy: Analyze the money movement by e-commerce by looking at order prices, freight and others.
Get the % increase in the cost of orders from year 2017 to 2018 (include months between Jan to Aug only). <br />
You can use the "payment_value" column in the payments table to get the cost of orders. <br />
Calculate the Total & Average value of order price for each state. <br />
Calculate the Total & Average value of order freight for each state. <br />
## Analysis based on sales, freight and delivery time. <br />
Find the no. of days taken to deliver each order from the order’s purchase date as delivery time. <br />
Also, calculate the difference (in days) between the estimated & actual delivery date of an order. <br />
Find out the top 5 states with the highest & lowest average freight value.<br />
Find out the top 5 states with the highest & lowest average delivery time.<br />
Find out the top 5 states where the order delivery is really fast as compared to the estimated date of delivery.<br />
You can use the difference between the averages of actual & estimated delivery date to figure out how fast the delivery was for each state.
Analysis based on the payments:<br />
Find the month on month no. of orders placed using different payment types.<br />
Find the no. of orders placed on the basis of the payment installments that have been paid.<br />


------------------------------------------------------------------------------------------------------------------------------------------------------------

```SQL
select column_name,data_type
from `target_customer.INFORMATION_SCHEMA.COLUMNS`
where table_name = 'customers'
````
```SQL
select min(order_purchase_timestamp) as start_date,
max(order_purchase_timestamp) as end_date
from `target_customer.orders `
```
```SQL
select count(distinct customer_city) as count_city, count(distinct customer_state) as count_state
from `target_customer.customers`
```
```SQL
select pre_year, count(order_id) as count_of_orders
from
(select *, extract (year from order_purchase_timestamp) as pre_year
from `target_customer.orders `) tb
group by pre_year
```
```SQL
select year, month,  count(order_id) as count_of_orders 
from
(select *, extract (year from order_purchase_timestamp) as year,extract(month from order_purchase_timestamp ) as month
from `target_customer.orders `) tb
group by year,month
order by year,month
```
```SQL
select time_of_order,count(*) as no_of_orders 
from(
select 
case 
when time_ between 0 and 6 then 'Dawn'
when time_ between 7 and 12 then 'Mornings'
when time_ between 13 and 18 then 'Afternoon'
when time_ between 19 and 23 then 'Night'
end as time_of_order,time_
from (
select extract(hour from order_purchase_timestamp) as time_
from `target_customer.orders ` ) A ) B
group by time_of_order
```
```SQL
select tb.customer_state,tb.month, count(tb.order_id) as Number_of_orders
from(
select *,extract(month from order_purchase_timestamp) as month 
from `target_customer.orders `  o join `target_customer.customers` c
on o.Customer_id = c.customer_id
join `target_customer.geolocation` g
on g.geolocation_zip_code_prefix = c.customer_zip_code_prefix
) tb
group by tb.customer_state,tb.month
order by tb.customer_state,tb.month
```
```SQL
select g.geolocation_state,count(c.customer_id) as no_of_customers
from  `target_customer.geolocation` g join `target_customer.customers` c
on g.geolocation_zip_code_prefix = c.customer_zip_code_prefix
group by g.geolocation_state
order by no_of_customers desc
```
```SQL
select((sum(case when year = 2018 and month between 01 and 08 then payment_value end) - sum(case when year = 2017 and month between 01 and 08 then payment_value end))/sum(case when year = 2018 and month between 01 and 08 then payment_value end))*100 as percentage 
from
 (
select payment_value,extract(year from O.order_purchase_timestamp) as year,extract(month from O.order_purchase_timestamp) as month 
from `target_customer.orders ` O inner join `target_customer.payments ` P 
on O.order_id = P.order_id
) A
```
```SQL
select geo.geolocation_state,sum(oi.price) as total_order_price, avg(oi.price) as average_value
from `target_customer.order_items ` oi  join `target_customer.sellers ` sel
on oi.seller_id = sel.seller_id 
join `target_customer.geolocation` geo
on sel.seller_zip_code_prefix = geo.geolocation_zip_code_prefix
group by geo.geolocation_state
order by geo.geolocation_state
limit 50
```
```SQL
select geo.geolocation_state,round(sum(oi.freight_value),2) as total_freight_value, round(avg(oi.freight_value),2) as Average_freight_value
from `target_customer.order_items ` oi  join `target_customer.sellers ` sel
on oi.seller_id = sel.seller_id 
join `target_customer.geolocation` geo
on sel.seller_zip_code_prefix = geo.geolocation_zip_code_prefix
group by geo.geolocation_state
order by geo.geolocation_state
limit 50
```
```SQL
select date_diff(date(order_delivered_customer_date),date(order_purchase_timestamp),day) as dilever_time, 
date_diff(date(order_delivered_customer_date),date(order_estimated_delivery_date),day ) as diff_estimated_dilevery_date
from `target_customer
```
```SQL
select geo.geolocation_state,round(avg(oi.freight_value),2) as average_freight_value 
from `target_customer.order_items ` oi  join `target_customer.sellers ` sel
on oi.seller_id = sel.seller_id 
join `target_customer.geolocation` geo
on sel.seller_zip_code_prefix = geo.geolocation_zip_code_prefix
group by geo.geolocation_state 
order by average_freight_value  desc
limit 5
```
```SQL
select geo.geolocation_state,round(avg(oi.freight_value),2) as lowest_average_freight_value
from `target_customer.order_items ` oi  join `target_customer.sellers ` sel
on oi.seller_id = sel.seller_id
join `target_customer.geolocation` geo
on sel.seller_zip_code_prefix = geo.geolocation_zip_code_prefix
group by geo.geolocation_state
order by lowest_average_freight_value asc
```
```SQL
select C.customer_state,avg(date_diff(date(order_delivered_customer_date),date(order_purchase_timestamp),day)) as average_deliver_time 
from `target_customer.orders ` o inner join `target_customer.customers` C       
on o.customer_id = C.customer_id
group by C.customer_state
order by average_deliver_time desc
limit 5 
```
```SQL
select C.customer_state,round(avg(date_diff(date(order_delivered_customer_date),date(order_purchase_timestamp),day)),2) as average_deliver_time 
from `target_customer.orders ` o inner join `target_customer.customers` C       
on o.customer_id = C.customer_id
group by C.customer_state
order by average_deliver_time asc
limit 5 
```
```SQL
select distinct customer_state
from
(
select *,date_diff(date(order_delivered_customer_date),date(order_purchase_timestamp),day) as dilever_time,
date_diff(date(order_delivered_customer_date),date(order_estimated_delivery_date),day) as diff_estimated_dilevery_date
from `target_customer.orders ` o join `target_customer.customers` c 
on o.customer_id = c.customer_id
) A 
where order_status = 'shipped'
```
```SQL
select P.payment_installments,count(O.order_id) as no_of_orders 
from `target_customer.orders `  O join `target_customer.payments ` P  
on O.order_id = P.order_id 
group by P.payment_installments 
order by 1
```



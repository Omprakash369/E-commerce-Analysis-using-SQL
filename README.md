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

```select column_name,data_type
from `target_customer.INFORMATION_SCHEMA.COLUMNS`
where table_name = 'customers'




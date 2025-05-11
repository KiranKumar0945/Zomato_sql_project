# SQL Project: Data Analysis for Zomato - A Food Delivery Company
## Overview

This project demonstrates my SQL problem-solving skills through the analysis of data for Zomato, a popular food delivery company in India.
The project involves setting up the database, importing data, handling null values, and solving a variety of business problems using complex SQL queries.

## Project Structure

- **Database Setup:** Creation of the `zomato_db` database and the required tables.
- **Data Import:** Inserting sample data into the tables.
- **Data Cleaning:** Handling null values and ensuring data integrity.
- **Business Problems:** Solving 20 specific business problems using SQL queries.

- ## Database Setup
```sql
CREATE DATABASE zomato_db;
```
### 1. Dropping Existing Tables
```sql
DROP TABLE IF EXISTS deliveries;
DROP TABLE IF EXISTS Orders;
DROP TABLE IF EXISTS customers;
DROP TABLE IF EXISTS restaurants;
DROP TABLE IF EXISTS riders;

-- Creating Tables
create table customers(
	customer_id int primary key,
	customer_name varchar(25) not null,
	reg_date date
	
);

create table restaurants(
	restaurant_id int primary key,
	restaurant_name varchar(55),
	city varchar(25),
	opening_hours varchar(55)
);

create table riders(
	rider_id int primary key,
	rider_name varchar(55),
	sign_up date
);

create table orders(
	order_id int primary key,
	customer_id int,
	restaurant_id int,
	order_item varchar(55).
	order_date date,
	order_time time,
	order_status varchar(25),
	total_amount numeric,
	foreign key (customer_id) references customers(customer_id),
	foreign key (restaurant_id) references restaurants(restaurant_id)
);

create table deliveries(
	delivery_id int primary key,
	order_id int,
	delivery_status varchar(35),
	delivery_time time,
	rider_id int,
	foreign Key (order_id) references orders(order_id),
	foreign key (rider_id) references riders(rider_id
);

## Data Import

## Data Cleaning and Handling Null Values
select count(*) from customers
where 
	customer_name is null
	or
	reg_date is null

select count(*) from restaurants
where
	restaurant_name is null
	or 
	city is null
	or 
	opening_hours is null

select * from orders
where 
	order_item is null
	or
	order_date is null
	or 
	order_time is null
	or
	order_status is null
	or 
	total_amount is null

select count(*) from riders
where
	rider_name is null
	or 
	sign_up is null

select count(*) from riders
where
	order_id is null
	or
	delivery_status


Before performing analysis, I ensured that the data was clean and free from null values where necessary. For instance:

```sql
UPDATE orders
SET total_amount = COALESCE(total_amount, 0);
```

## Business Problems Solved

### Top 5 Most Frequently Ordered Dishes
### 1. Write a query to find the top 5 most frequently ordered dishes by customer called "Arjun Mehta" in the last 1 year.
```sql
with ranking_cte as(
select
	c.customer_id,
	c.customer_name,
	o.order_item,
	count(o.order_item) as total_orders,
	dense_rank() over(order by count(o.order_item) desc) as rn
from orders as o 
join customers as c 
on o.customer_id = c.customer_id 
where c.customer_name = 'Arjun Mehta' and order_date between '2023-09-02' and '2024-09-02'
group by 1,2,3
order by 4 desc)
select
	customer_id,
	customer_name,
	order_item,
	total_orders
from ranking_cte
where rn <= 5
```










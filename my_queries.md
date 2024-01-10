--Create tables (orders, customers, products) and import csv files  
```sql
create table orders (
    	row_id int
    ,	order_id varchar(50)
    ,	order_date date
    ,	ship_date date
    ,	ship_mode varchar(20)
    ,	customer_id varchar(50)
    ,	product_id varchar(50)
    ,	country varchar(20)
    ,	city varchar(20)
    ,	state varchar(20)
    ,	postal_code int
    ,	region varchar(10)
    ,	sales decimal(10,2)
    ,	constraint "pk_orders" primary key (
        "row_id")
);
```
```sql
create table customers (
    	customer_id varchar(50)
    ,	customer_name varchar(50)
    ,	segment varchar(20)
    ,	constraint "pk_customers" primary key (
        "customer_id")
);
````

```sql
create table products (
    	product_id varchar(50)
    ,	category varchar(20)
    ,	sub_category varchar(20)
    ,	constraint "pk_products" primary key (
        "product_id")
);
````

/*
 Get a comprehensive view of store revenue. This query gathers 
 data across various aspects of each sale, such as the total items, 
 total revenues and combines it with customer information. The aim 
 is to get a deeper insight into revenue trends in multiple directions,
 such as over time, by region, and by customer segment 
*/
```sql
select
		o.order_date
	,	o.order_id
	,	c.customer_name
	,	c.segment
	,	o.state
	,	o.region
	,	count(p.sub_category) as total_items  -- Counts the number of items per order
	,	sum(o.sales) as total_revenues  --  Sum of sales per order
from
 orders o
    left join customers c 
	on o.customer_id = c.customer_id
    left join products p 
	on o.product_id = p.product_id
group by
		o.order_date
	,	o.order_id
	,	c.customer_name
	,	c.segment
	,	o.state
	,	o.region
order by
		o.order_date
;
```
![image](https://github.com/hoanthien2017/effectiveness_of_marketing_campaign/blob/3767fc313082cd1a2ce5f4ca985ad14096c16991/hinh%203.png)

/* 
The query focuses on sales data by product category. This provides the
performance of product categories on daily, monthly, and yearly basics, 
allowing us to track which categories perform best on a day-to-day basis
and vice versa. Tt is necessary for identifying market trends and making
informed decisions about marketing strategies, and sales forecasting.
*/
```sql
select
		o.order_date	
	,	p.category
	,	count(p.product_id) as total_item -- Counts the number of items per category
	,	sum(o.sales) as total_sale -- Sum of sales per category
from
	orders o
	left join products p
		on o.product_id = p.product_id
group by 
		o.order_date	
	,	p.category
order by
		o.order_date
;
```
![image](https://github.com/hoanthien2017/effectiveness_of_marketing_campaign/blob/a13bad513247e7620b5fc8a99f2fffa646395362/hinh%204.png)

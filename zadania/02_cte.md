## Zadanie 1
```sql
with paid_orders as(
	select
	order_id,
	customer_id,
	order_date,
	total_amount
	from course.orders o
	where status = 'paid'
	order by order_date)
select * from paid_orders
```
## Zadanie 2
```sql
with customer_sales as(
	select
	customer_id,
	sum(total_amount) as total_revenue
	from course.orders
	group by customer_id 
	having sum(total_amount) > 200)
select * from customer_sales
```
## Zadanie 3
```sql
with customer_sales as(
	select
	c.customer_id,
	c.customer_name,
	count(order_id) as orders_count,
	sum(total_amount) as total_revenue
	from course.orders o
	join course.customers c
	on c.customer_id = o.customer_id
	group by c.customer_id
	order by total_revenue desc)
select * from customer_sales
```
## Zadanie 4
```sql
with paid_orders as(
	select
	order_id,
	order_date,
	status,
	total_amount
	from course.orders 
	where status = 'paid'
	),
cancelled_orders as (
	select
	order_id,
	order_date,
	status,
	total_amount
	from course.orders 
	where status = 'cancelled'
	) 

```
## Zadanie 5
```sql
with customer_totals as(
	select
	c.customer_id,
	c.customer_name,
	sum(o.total_amount) as total_revenue
	from course.customers c
	join course.orders o
	on c.customer_id = o.customer_id 
	group by c.customer_id, c.customer_name 
	),
average_customer as(
	select 
	avg(total_revenue) as average_total_revenue
	from customer_totals 
	)
select
customer_id,
customer_name,
total_revenue 
from customer_totals 
order by total_revenue desc;
```
## Zadanie 6
```sql

```
## Zadanie 7
```sql
with customers_without_orders as(
	select
	c.customer_id,
	c.customer_name,
	c.country
	from course.customers c
	left join course.orders o
	on c.customer_id = o.customer_id
	where o.customer_id is null)
select 
customer_id,
customer_name,
country 
from customers_without_orders
```
## Zadanie 8
```sql
```

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

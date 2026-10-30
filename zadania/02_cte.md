## Zadanie 1
```sql
with paid_orders as(
	select
	o.order_id,
	o.customer_id,
	o.order_date,
	o.total_amount
	from course.orders o
	where status = 'paid'
	order by o.order_date)
select * from paid_orders 
```
## Zadanie 2
```sql
with customer_sales as(
	select
	c.customer_id,
	sum(o.total_amount) as total_revenue
	from course.customers c
	join course.orders o
	on c.customer_id = o.customer_id 
	group by c.customer_id 
	having sum(o.total_amount) > 200
	)
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
with order_items_totals as(
	select
	oi.order_id,
	sum(oi.quantity * oi.unit_price) as items_total_amount
	from course.order_items oi
	group by oi.order_id
	)
select
o.order_id,
o.status,
sum(o.total_amount) as order_total_amount,
items_total_amount,
items_total_amount - sum(o.total_amount) as difference_amount
from course.orders o
left join order_items_totals oit
on o.order_id = oit.order_id 
group by o.order_id, o.status, items_total_amount
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
with customers_without_orders as(
	select
	c.customer_id as object_id,
	c.customer_name as object_name,
	0 as metric_value
	from course.customers c
	left join course.orders o
	on c.customer_id = o.customer_id 
	where o.order_id is null
	),
 products_without_sales as(
	select
	p.product_id as object_id,
	p.product_name as object_name,
	0 as metric_value
	from course.products p
	left join course.order_items oi
	on p.product_id = oi.product_id 
	where oi.order_item_id is null
	),
customers_with_high_revenue as (
	select
	c.customer_id as object_id,
	c.customer_name as object_name,
	sum(o.total_amount) as total_revenue
	from course.customers c
	join course.orders o
	on c.customer_id = o.customer_id 
	group by c.customer_id, c.customer_name
	having sum(o.total_amount) > 300
	)
select 
	'customer_without_order' as issue_type,
	object_id,
	object_name,
	metric_value
from customers_without_orders 

union all

select 
	'products_without_sales' as issue_type,
	object_id,
	object_name,
	metric_value
from products_without_sales

union all

select 
	'customers_with_high_revenue' as issue_type,
	object_id,
	object_name,
	total_revenue as metric_value
from customers_with_high_revenue
```
## Zadanie 9
```sql
with product_sales as(
	select
	p.product_id,
	p.product_name,
	p.category,
	sum(oi.quantity) as units_sold,
	sum(oi.quantity * oi.unit_price) as total_revenue 
	from course.products p
	join course.order_items oi
	on p.product_id = oi.product_id
	group by p.product_id,p.product_name, p.category
	having sum(oi.quantity) > 1)
select * from product_sales
```
## Zadanie 10
```sql
with monthly_sales as(
	select
	date_trunc('month',o.order_date) as sales_month,
	c.country,
	count(o.order_id) as orders_count,
	sum(o.total_amount) as total_revenue
	from course.customers c
	join course.orders o
	on c.customer_id = o.customer_id 
	group by date_trunc('month',o.order_date), c.country
	having sum(o.total_amount) > 100
	)
select * from monthly_sales
```
## Zadanie 11
```sql
with customer_activity as(
	select
	c.customer_id,
	c.customer_name,
	count(o.order_id) as orders_count
	from course.customers c
	left join course.orders o
	on c.customer_id = o.customer_id 
	group by c.customer_id, c.customer_name 
)
select
customer_id,
customer_name,
orders_count,
case
	when orders_count = 0 then 'without_orders'
	when orders_count > 0 then 'with_orders'
end as customer_segments 
from customer_activity 
```

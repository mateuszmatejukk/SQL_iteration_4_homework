## Zadanie 1
```sql
select
c.customer_id,
c.customer_name,
c.country,
c.acquisition_channel,
count(o.order_id) as orders_count,
count(case when o.status = 'paid' then 1 end) as paid_orders_count,
count(case when o.status = 'cancelled' then 1 end) as cancelled_orders_count,
coalesce(sum(o.total_amount),0) as total_revenue,
avg(o.total_amount) as average_order_value,
min(o.order_date) as first_order_date,
max(o.order_date) as last_order_date,
case 
	when count(o.order_id) = 0 then 'no_orders'
	else 'buyer'
end as customer_status
from course.customers c
left join course.orders o
on c.customer_id = o.customer_id 
group by c.customer_id, c.customer_name, c.country, c.acquisition_channel
order by coalesce(sum(o.total_amount),0)  desc;
```
## Zadanie 2
```sql
select
p.product_id,
p.product_name,
p.category,
p.base_price,
coalesce(sum(oi.quantity),0) as units_sold,
count(oi.order_id) as orders_count,
coalesce(sum(oi.unit_price * oi.quantity), 0) as gross_revenue,
avg(oi.unit_price) as average_unit_price,
case
	when count(oi.order_id) = 0 then 'not_sold'
	else 'sold'
end as sale_status
from course.products p
left join course.order_items oi
on p.product_id = oi.product_id 
group by p.product_id, p.product_name, p.category
order by coalesce(sum(oi.unit_price * oi.quantity), 0) desc
```
## Zadanie 3
```sql
select
date_trunc('month',o.order_date) as sales_month,
c.country,
count(o.order_id) as orders_count,
count(distinct c.customer_id) as customers_count,
count(case when o.status = 'paid' then 1 end) as paid_orders_count,
count(case when o.status = 'cancelled' then 1 end) as cancelled_orders_count,
sum(o.total_amount) as total_revenue,
avg(o.total_amount) as average_order_value
from course.orders o
join course.customers c
on c.customer_id = o.customer_id 
group by date_trunc('month',o.order_date), c.country 
having sum(o.total_amount) > 100
order by date_trunc('month',o.order_date), sum(o.total_amount) desc
```
## Zadanie 4
```sql
with customer_total as(
	select
	c.customer_id,
	c.customer_name,
	count(o.order_id) as orders_count,
	sum(o.total_amount) as total_revenue
	from course.customers c
	join course.orders o
	on c.customer_id = o.customer_id 
	group by c.customer_id, c.customer_name)
select 
customer_id,
customer_name,
orders_count,
total_revenue
from customer_total
where total_revenue > (
select
avg(total_revenue)
from customer_total
)
order by total_revenue desc;
```
## Zadanie 5
```sql
select
'customer_without_order' as issue_type,
c.customer_id as object_id,
c.customer_name as object_name,
'Customer has no orders' as details
from course.customers c
left join course.orders o
on c.customer_id = o.customer_id 
where o.order_id is null

union all

select
'product_without_sale' as issue_type,
p.product_id as object_id,
p.product_name as product_name,
'Product has no sales' as details
from course.products p
left join course.order_items oi
on p.product_id = oi.product_id 
where oi.order_id is null

union all

select
'order_without_order_item' as issue_type,
o.order_id as object_id,
o.status as object_name,
'Order has not order item' as details
from course.orders o
left join course.order_items oi
on o.order_id = oi.order_id
where oi.order_item_id is null

union all

select
'Customer without email' as issue_type,
customer_id as object_id,
customer_name as object_name,
'Customer has no email' as details
from course.customers 
where email is null
order by issue_type, object_id
```
## Zadanie 6
```sql

```

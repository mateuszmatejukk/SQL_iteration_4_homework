## Zadanie 1
```sql
select
order_id,
customer_id,
order_date,
total_amount,
row_number() over(
partition by customer_id
order by order_date desc) as customer_order_number 
from course.orders
```
## Zadanie 2
```sql
with numbered_orders as(
	select
	order_id,
	customer_id,
	order_date,
	total_amount,
	row_number() over (
	partition by customer_id
	order by order_date desc, order_id desc
	) as rn 
	from course.orders 
)
select
	order_id,
	customer_id,
	order_date,
	total_amount
from numbered_orders
where rn = 1;
```
## Zadanie 3
```sql
select
order_id,
customer_id,
total_amount,
rank() over(
	order by total_amount desc
) as revenue_rank,
dense_rank() over(
	order by total_amount desc
) as revenue_desc_rank
from course.orders 
```
## Zadanie 4
```sql
select
order_id,
customer_id,
order_date,
total_amount,
lag(total_amount) over(
partition by customer_id
order by order_date) as previous_order_amount
from course.orders 
```
## Zadanie 5
```sql

```

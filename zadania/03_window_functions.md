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
select
order_id,
customer_id,
order_date,
total_amount,
lag(total_amount) over(
	partition by customer_id
	order by order_date, order_id
	) as previous_order_amount,
total_amount - lag(total_amount) over(
	partition by customer_id
	order by order_date, order_id
	) as amount_difference
from course.orders;
```
## Zadanie 6
```sql
select
order_id,
customer_id,
order_date,
lead(total_amount) over(
	partition by customer_id
	order by order_date
	) as next_order_date
from course.orders;
```
## Zadanie 7
```sql
select
order_id,
customer_id,
order_date,
total_amount,
sum(total_amount) over(
	partition by customer_id
	order by order_date,order_id
	rows between unbounded preceding and current row
	) as running_customer_revenue
from course.orders;
```
## Zadanie 8
```sql
select
    order_id,
    customer_id,
    total_amount,
    sum(total_amount) over (
        partition by customer_id
    ) as customer_total_revenue
from course.orders;
```
## Zadanie 9
```sql
select
    order_id,
    total_amount,
    sum(total_amount) over () AS all_orders_revenue,
    round(
        total_amount / sum(total_amount) OVER () * 100,
        2
    ) as revenue_percent
from course.orders;
```
## Zadanie 10
```sql
with country_sales as(
	select
	c.country,
	sum(o.total_amount) as total_revenue
	from course.customers c
	join course.orders o
	on c.customer_id = o.customer_id 
	group by c.country
	)
select 
country,
total_revenue,
dense_rank() over(
order by total_revenue desc
) as country_rank
from country_sales
```
## Zadanie 11
```sql
with ranked_customers as(
	select
	customer_id,
	order_id,
	order_date,
	total_amount,
	row_number() over(
	partition by customer_id
	order by order_date, order_id
	) as rn
	from course.orders
	)
select
customer_id,
order_id,
order_date,
total_amount
from ranked_customers
where rn = 1;
```
## Zadanie 12
```sql
with total_products as(
	select
	p.category,
	p.product_id,
	p.product_name,
	sum(oi.quantity) as units_sold
	from course.products p
	join course.order_items oi 
	on p.product_id = oi.product_id 
	group by p.category, p.product_id,p.product_name)
select 
category,
product_id,
product_name,
units_sold,
dense_rank() over(
	partition by category
	order by units_sold desc
	) as rank_in_category
from total_products
```
## Zadanie 13
```sql
select
order_id,
customer_id,
order_date,
total_amount,
avg(total_amount) over(
	partition by customer_id
	order by order_date, order_id
	ROWS BETWEEN 2 PRECEDING AND CURRENT row
	) as three_order_moving_avg
from course.orders 
```
## Zadanie 14
```sql
with customer_total as(
	select
	customer_id,
	sum(total_amount) as total_revenue
	from course.orders 
	group by customer_id
	)
select
customer_id,
total_revenue,
ntile(4) over(
	order by total_revenue desc
	) as revenue_quartille
from customer_total
```
## Zadanie 15
```sql
select
order_id,
customer_id,
order_date,
total_amount,
first_value(total_amount) over(
	partition by customer_id
	order by order_date, order_id
) as first_order_amount,
last_value(total_amount) over(
	partition by customer_id
	order by order_date, order_id
	ROWS BETWEEN UNBOUNDED PRECEDING AND UNBOUNDED FOLLOWING
) as last_order_amount
from course.orders 
order by customer_id, order_date,order_id;
```
## Zadanie 16
```sql
select
order_id,
customer_id,
order_date,
total_amount,
lag(total_amount) over customer_timeline as previous_order_amount,
lead(total_amount) over customer_timeline as next_order_amount,
row_number() over customer_timeline as customer_order_number
from course.orders 
window customer_timeline as(
	partition by customer_id
	order by order_date, order_id
)
order by customer_id, order_date, order_id;
```
## Zadanie 17
```sql
with customers_ranked as(
	select
	c.*,
	row_number() over(
	partition by lower(email)
	order by signup_date desc, customer_id desc
	) as duplicate_rank
	from course.customers c
	where email is not null
)
select
customer_id,
customer_name,
email,
country,
signup_date
from customers_ranked
where duplicate_rank = 1;
```

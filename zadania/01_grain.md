## Zadanie 1
```sql
select * 
from course.customers
```
Grain wyniku: jeden wiersz = dane jednego klienta.
## Zadanie 2
```sql
select
order_id,
customer_id,
order_date,
status,
total_amount
from course.orders;
```
Grain wyniku: jeden wiersz = dane jednego zamówienia.
## Zadanie 3
```sql
select
c.customer_id,
c.customer_name,
o.order_id,
o.total_amount
from course.customers c
left join course.orders o
on c.customer_id = o.customer_id
```
Grain wyniku : jeden wiersz = jedno zamówienie
## Zadanie 4
```sql
select count (*) 
from course.customers c
left join course.orders o
on c.customer_id = o.customer_id
```
13 wierszy
```sql
select count (*) 
from course.customers c
```
10 wierszy
Te liczby moga byc rozne ponieważ zmienia się grain.
## Zadanie 5
```sql
select
count(distinct c.customer_id) as customers_count
from course.customers c
left join course.orders o
on c.customer_id = o.customer_id
```
## Zadanie 6
```sql
select
o.order_id,
o.total_amount,
oi.order_item_id,
oi.quantity,
oi.unit_price
from course.orders o
join course.order_items oi
on o.order_id = oi.order_id;
```
Jeden wiersz = jedna pozycja zamowienia
## Zadanie 7
```sql
select
sum(oi.quantity * oi.unit_price) as items_total_revenue
from course.order_items oi
```
## Zadanie 8
```sql
select
oi.order_id,
sum(oi.quantity * oi.unit_price) as items_total_amount
from course.order_items oi
group by oi.order_id;
```
## Zadanie 9
```sql
select
customer_id,
count(order_id) as orders_count,
sum(total_amount) as total_revenue
from course.orders 
group by customer_id
```
## Zadanie 10
```sql

```

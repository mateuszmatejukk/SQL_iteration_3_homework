## Zadanie 1
```sql
CREATE table course.customers_copy AS
SELECT *
FROM course.customers;
```
## Zadanie 2
```sql
CREATE TABLE course.paid_orders AS
SELECT
    order_id,
    customer_id,
    order_date,
    total_amount
FROM course.orders
WHERE status = 'paid';
```
```sql
select * from  course.paid_orders;
```
działa
## Zadanie 3
```sql
create table  course.orders_with_customers as
select
o.order_id,
o.order_date,
o.status,
o.total_amount,
c.customer_id,
c.customer_name,
c.country
from course.customers c
join course.orders o
on c.customer_id = o.customer_id;
```
## Zadanie 4
```sql
create table course.sales_by_country as
select
c.country,
count(o.order_id) as orders_count,
sum(o.total_amount) as total_revenue,
round(avg(o.total_amount),2) as average_order_value
from course.orders o
join course.customers c
on c.customer_id = o.customer_id
group by c.country;
```
## Zadanie 5
```sql
create table course.empty_orders_report as
select 
order_id,
order_date,
status,
total_amount
from course.orders 
where 1 = 0;
```
## Zadanie 6
```sql
create table course.orders_with_tier as 
select
order_id,
customer_id,
order_date,
status,
total_amount,
case when total_amount >= 150 then 'high'
else 'standard'
end as order_tier
from course.orders;
```
## Zadanie 7
```sql
create table course.product_sales_summary as
select
p.product_id,
p.product_name,
p.category,
sum(oi.quantity) as units_sold,
sum(oi.quantity * oi.unit_price) as total_revenue
from course.products p
join course.order_items oi
on p.product_id = oi.product_id
group by p.product_id;
```
## Zadanie 8
```sql
create table course.data_quality_report as
select
'customer without order' as issue_type,
c.customer_id as object_id,
c.customer_name as object_name
from course.customers c
left join course.orders o
on c.customer_id = o.customer_id
where o.order_id is null

union all
select
'products without sale' as issue_type,
p.product_id as object_id,
p.product_name as object_name
from course.products p
left join course.order_items oi
on p.product_id = oi.product_id
where oi.order_item_id is null;
```

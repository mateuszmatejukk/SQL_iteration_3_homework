## Zadanie 1
```sql
with paid_orders as(
	select 
	order_id,
	customer_id,
	order_date,
	total_amount
	from course.orders
	where status = 'paid'
)
select * 
from paid_orders
order by order_date;
```
## Zadanie 2
```sql
with customer_sales as (
	select
	customer_id,
	sum(total_amount) as total_revenue
	from course.orders
	group by customer_id
	having(sum(total_amount)) > 200
)
select * 
from customer_sales;
```
## Zadanie 3
```sql
with customer_sales as(
	select
	o.customer_id,
	c.customer_name,
	count(o.order_id) as orders_count,
	sum(o.total_amount) as total_revenue
	from course.orders o 
	join course.customers c
	on o.customer_id = c.customer_id
	group by 
	o.customer_id,
	c.customer_name
	)
select 
customer_id,
customer_name,
orders_count,
total_revenue
from customer_sales
order by total_revenue desc;
```
## Zadanie 4
```sql
with paid_orders as (
	select
	order_id,
	customer_id,
	order_date,
	status,
	total_amount
	from course.orders 
	where status = 'paid'
),
cancelled_orders as (
	select
	order_id,
	customer_id,
	order_date,
	status,
	total_amount
	from course.orders
	where status = 'cancelled'
)
select 
'paid orders' as status_group,
count(order_id) as orders_count,
sum(total_amount) as total_revenue
from paid_orders 
union all

select 
'cancelled orders' as status_group,
count(order_id) as orders_count,
sum(total_amount) as total_revenue
from cancelled_orders  
```
## Zadanie 5
```sql
with customer_totals as(
	select
	customer_id,
	sum(total_amount) as total_revenue
	from course.orders
	group by customer_id
),
average_customer_total as(
	select
	avg(total_revenue) as avg_total_revenue
	from customer_totals
)
select 
c.customer_id, 
c.customer_name,
ct.total_revenue 
from course.customers c
join customer_totals ct 
on c.customer_id = ct.customer_id
join average_customer_total act 
on ct.total_revenue > act.avg_total_revenue
order by ct.total_revenue desc;
```
## Zadanie 6
```sql
with order_items_totals as(
	select
	order_id,
	sum(quantity * unit_price) as items_total_amount
	from course.order_items
	group by order_id
	)
select 
o.order_id,
o.status,
o.total_amount as order_total_amount,
oit.items_total_amount,
(o.total_amount- oit.items_total_amount) as difference_amount
from course.orders o
left join order_items_totals oit
on o.order_id = oit.order_id 
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
	where o.customer_id is null
)
select
customer_id,
customer_name,
country,
'customer_without_orders' as issue_type
from customers_without_orders 
order by customer_id;
```
## Zadanie 8
```sql
with customers_without_orders as(
	select
	c.customer_id,
	c.customer_name,
	c.country
	from course.customers c
	left join course.orders o
	on c.customer_id = o.customer_id 
	where o.customer_id is null
),
products_without_sales as(
	select
	p.product_id,
	p.product_name,
	p.category
	from course.products p
	left join course.order_items oi
	on p.product_id = oi.product_id
	where oi.order_item_id is null
),
valuable_customers as(
	select
	c.customer_id,
	c.customer_name,
	c.country,
	sum(o.total_amount) as total_revenue
	from course.customers c
	join course.orders o
	on c.customer_id = o.customer_id 
	group by c.customer_id
	having sum(o.total_amount) > 300
)
select 
'customer_without_order' as issue_type,
customer_id as object_id,
customer_name as object_name,
0 as metric_value
from customers_without_orders 

union all

select 
'products_without_sales' as issue_type,
product_id as object_id,
product_name as object_name,
0 as metric_value 
from products_without_sales 

union all

select 
'valuable_customers' as issue_type,
customer_id as object_id,
customer_name as object_name,
total_revenue as metric_value
from valuable_customers
```
## Zadanie 9

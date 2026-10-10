## Zadanie 1
```sql
create view course.v_orders_with_customers as
select
o.order_id,
o.order_date,
o.status,
o.total_amount,
c.customer_id,
c.customer_name,
c.country
from course.orders o
join course.customers c
on o.customer_id = c.customer_id;
```
## Zadanie 2
```sql
select * from course.v_orders_with_customers;
```
## Zadanie 3
```sql
select 
country,
count(order_id) as orders_count,
sum(total_amount) as total_revenue
from course.v_orders_with_customers
group by country;
```
## Zadanie 4
```sql
create view course.v_order_items_with_products as
select 
oi.order_id,
oi.order_item_id,
oi.product_id,
p.product_name,
p.category,
oi.quantity,
oi.unit_price,
(oi.quantity * oi.unit_price) as line_value
from course.order_items oi
join course.products p
on oi.product_id = p.product_id;
```
## Zadanie 5
```sql
create view course.v_cars_by_make as
select 
make,
count(car_id) as cars_count
from course.cars
group by make;
```
## Zadanie 6
```sql
select * from course.cars 
limit  10;

insert into course.cars(
car_id,
make,
model,
production_year,
capacity)

values(
11,
'SsangYong',
'Korando',
2020,
1.1);
```
tak widzi nowa marke
## Zadanie 7
```sql
create materialized view course.mv_cars_by_make as
select
make,
count(*) as cars_count
from course.cars
group by make;
```
## Zadanie 8
```sql
INSERT INTO course.cars (
    car_id,
    make,
    model,
    production_year,
    capacity
)
VALUES (
    9000002,
    'SsangYong',
    'Torres',
    2025,
    1.5
);
```
```sql
select * from course.cars 
where model = 'Torres';
```
rekord jest w course.cars
```sql
select * from course.v_cars_by_make;
```
widac go w course.v_cars_by_make,
```sql
select * from course.mv_cars_by_make;
```
nie widać go w materialized view
## Zadanie 9
```sql
REFRESH MATERIALIZED VIEW course.mv_cars_by_make;
```
tak teraz widac
## Zadanie 10
```sql
create materialized view course.mv_sales_by_country as
select
c.country,
count(o.order_id) as orders_count,
sum(o.total_amount) as total_revenue
from course.customers c
join course.orders o 
on c.customer_id = o.customer_id
group by c.country;
```
## Zadanie 11
```sql
REFRESH MATERIALIZED VIEW course.mv_sales_by_country;
```
## Zadanie 12
```sql
create materialized view course.mv_monthly_sales as
select
DATE_TRUNC('month', o.order_date) as sales_month,
c.country,
count(o.order_id) as orders_count,
sum(o.total_amount) as total_revenue
from course.customers c
join course.orders o
on c.customer_id = o.customer_id
group by o.order_date, c.country;
```

## Zadanie 1
```sql
DROP SCHEMA course CASCADE;
```
```sql
create schema course;
```
## Zadanie 2
```sql
create table if not exists course.customers (
customer_id int primary key,
customer_name varchar(100) not null,
email varchar(150) unique,
country varchar(2) check(country in ('PL','DE','FR')),
signup_date DATE not null,
acquisition_channel varchar(30)
);
```
## Zadanie 3
```sql
create table if not exists course.products(
product_id INT primary key,
product_name varchar(100) not null,
category varchar(50) not null check(category in ('template','mentoring','session')), 
base_price numeric(10,2) not null check(base_price >= 0)
);
```
## Zadanie 4
```sql
create table if not exists course.orders(
order_id INT primary key,
customer_id INT references course.customers(customer_id),
order_date DATE not null,
status varchar(30) default 'pending' check(status in ('pending','paid','cancelled')),
total_amount numeric(10,2) check (total_amount >= 0)
);
```
## Zadanie 5
```sql
create table if not exists course.orders(
order_id INT primary key,
customer_id INT references course.customers(customer_id),
order_date DATE not null,
status varchar(30) default 'pending' check(status in ('pending','paid','cancelled')),
total_amount numeric(10,2) check (total_amount >= 0)
);
```
## Zadanie 6
```sql
insert into course.customers(
customer_id,
customer_name,
email,
country,
signup_date,
acquisition_channel)

values(
1,
'Jan Kowalski',
'jan.kowalski@example.com',
'PL',
'2026-10-10',
'linkedin');
```
```sql
insert into course.customers(
customer_id,
customer_name,
email,
country,
signup_date,
acquisition_channel)

values(
2,
'Jakub Nowak',
'jakub.nowak@example.com',
'DE',
'2026-10-11',
'google');
```
```sql
insert into course.products(
product_id,
product_name,
category,
base_price)

values(
100,
'PostgreSQL template',
'template',
49.99);
```
```sql
insert into course.products(
product_id,
product_name,
category,
base_price)

values(
101,
'Databricks Session',
'session',
299.99
);
```
```sql
insert into course.orders(
order_id,
customer_id,
order_date,
status,
total_amount)

values(
1000,
1,
'2026-10-11',
'paid',
49.99);
```
```sql
insert into course.orders(
order_id,
customer_id,
order_date,
status,
total_amount)

values(
1001,
2,
'2026-10-12',
'cancelled',
299.99);
```
```sql
insert into course.order_items(
order_item_id,
order_id,
product_id,
quantity,
unit_price)

values(
1,
1000,
100,
1,
49.99)
```
```sql
insert into course.order_items(
order_item_id,
order_id,
product_id,
quantity,
unit_price)

values(
2,
1001,
101,
1,
299.99
);
```
## Zadanie 7
```sql
```

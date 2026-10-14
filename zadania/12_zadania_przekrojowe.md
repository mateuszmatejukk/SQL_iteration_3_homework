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

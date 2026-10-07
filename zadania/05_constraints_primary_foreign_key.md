## Zadanie 1
```sql
create table course.customers(
customer_id INT primary key not null,
customer_name TEXT not null CHECK (LENGTH(customer_name) >= 2),
email TEXT unique,
country TEXT not null CHECK (country IN ('PL', 'DE', 'FR','US')),
signup_date DATE not null,
acquisition_channel TEXT
);
```
## Zadanie 2
```sql
create table course.products(
product_id INT primary key,
product_name TEXT not null,
category TEXT not null check(category in ('course','ebook','template','consulting')),
base_price numeric(10,2) not null  CHECK (base_price >= 0 and base_price <= 10000)
);
```
## Zadanie 3
```sql
create table course.orders(
order_id INT primary key,
customer_id INT not null REFERENCES course.customers(customer_id),
order_date DATE not null,
status TEXT not null default 'pending' check (status in ('pending','paid','cancelled')),
total_amount numeric(10,2) not null check(total_amount > 0)
);
```
## Zadanie 4
```sql
create table course.order_items(
order_id INT not NULL,
line_number INT check(line_number > 0),
product_id INT,
quantity int not null check(quantity > 0),
unit_price numeric(10,2) not null check (unit_price >= 0 and unit_price < 10000),

constraint pk_order_items 
primary key(order_id, line_number),

constraint fk_order_items_order
foreign key(order_id)
references course.orders(order_id),

constraint fk_order_items_product
foreign key(product_id)
references course.products(product_id)

);
```
## Zadanie 5
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
'Anna Nowak',
'anna@example.com',
'PL',
'2026-01-10',
'google');
```
## Zadanie 6
```sql

```

## Zadanie 1
```sql
create schema if not exists course;
```
## Zadanie 2
```sql
create table course.customers(
customer_id INT,
customer_name TEXT,
email TEXT,
country TEXT,
signup_date DATE,
acquisition_channel TEXT
);
```
## Zadanie 3
```sql
create table course.products(
product_id INT,
product_name TEXT,
category TEXT,
base_price numeric (10,2)
);
```
## Zadanie 4
```sql
create table course.orders(
order_id INT,
customer_id INT,
order_date DATE,
status TEXT,
total_amount numeric(10,2)
);
```
## Zadanie 5
```sql
create table course.order_items(
order_item_id INT,
order_id INT,
product_id INT,
quantity INT,
unit_price numeric(10,2)
);
```
## Zadanie 6
```sql
select column_type, data_type
from information.schema.columns;
```
## Zadanie 7
Jeden wiersz w tabeli course.orders oznacza jedno zamówienie złożone przez klienta, wraz z podstawowymi informacjami o nim.

## Zadanie 8
Jeden wiersz w tabeli course.order_items oznacza jedną pozycję w zamówieniu, czyli jeden produkt kupiony w danym zamówieniu wraz z jego ilością.

## Zadanie 9
```sql
select 
table_schema,
table_name
from information_schema.tables
where table_schema = 'course';
```
## Zadanie 10
Bo orders.customer_id wskazuje na customers.customer_id, wiec tabela customers musi juz istniec, a products.product_id wskazuje na order_items.product_id, więc mamy analogiczną sytuację.

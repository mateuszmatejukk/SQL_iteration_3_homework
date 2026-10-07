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
 BŁĄD: podwójna wartość klucza narusza ograniczenie unikalności "customers_pkey"
  Detail: Klucz (customer_id)=(1) już istnieje.
  Constraintem który zablokował ten insert był primary key ponieważ blokuje on duplikaty.

## Zadanie 7
BŁĄD: podwójna wartość klucza narusza ograniczenie unikalności "customers_email_key"
  Detail: Klucz (email)=(anna@example.com) już istnieje.
  Constraintem który zablokował ten insert był unique ponieważ blokuje on duplikaty.

## Zadanie 8
```sql
insert into course.customers(
customer_id,
customer_name,
email,
country,
signup_date,
acquisition_channel)

values(
3,
'',
'example@example.com',
'PL',
'2026-10-11',
'google'
);
```
SQL Error [23514]: BŁĄD: nowy rekord dla relacji "customers" narusza ograniczenie sprawdzające "customers_customer_name_check"
  Detail: Niepoprawne ograniczenia wiersza (3, , example@example.com, PL, 2026-10-11, google).
  Constraintem który zablokował ten insert był not null + check długości textu (?) bo input był poniżej 2?

## Zadanie 9
```sql
insert into course.products(
product_id,
product_name,
category,
base_price)

values(
100,
'SQL Starter Pack',
'course',
149.00);
```
## Zadanie 10
```sql
insert into course.products(
product_id,
product_name,
category,
base_price)

values(
101,
'Databricks Tutorial',
'consulting',
-432432.23);
```
 BŁĄD: nowy rekord dla relacji "products" narusza ograniczenie sprawdzające "products_base_price_check"
  Detail: Niepoprawne ograniczenia wiersza (101, Databricks Tutorial, consulting, -432432.23).
Constraintem który zablokował ten insert był CHECK base_price >= 0.

## Zadanie 11
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
'2026-02-01',
'paid',
149.00
);
```
## Zadanie 12
```sql
insert into course.orders(
order_id,
customer_id,
order_date,
status,
total_amount)

values(
1001,
999,
'2026-02-01',
'paid',
200.00);
```
BŁĄD: wstawianie lub modyfikacja na tabeli "orders" narusza klucz obcy "orders_customer_id_fkey"
  Detail: Klucz (customer_id)=(999) nie występuje w tabeli "customers".
Foreign key zablokował ten insert ponieważ w orders.customer_id możze pojawić się tylko taki customer_id który znajduje się w customers.customer_id

## Zadanie 13
```sql
insert into course.orders(
order_id,
customer_id,
order_date,
total_amount)

values(
1002,
1,
current_date,
2441.21
);
```
Pending zostało wpisane do tabeli.
## Zadanie 14

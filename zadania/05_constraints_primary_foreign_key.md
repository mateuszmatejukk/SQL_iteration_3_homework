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
BŁĄD: nowy rekord dla relacji "orders" narusza ograniczenie sprawdzające "orders_status_check"
```sql
check (status in ('pending','paid','cancelled'))
```
ten constraint zablokował ten insert.
## Zadanie 15
```sql
insert into course.order_items(
order_id,
line_number,
product_id,
quantity,
unit_price)

values(
1000,
1,
100,
1,
149.00)
```
## Zadanie 16
```sql
insert into course.order_items(
order_id,
line_number,
product_id,
quantity,
unit_price)

values(
1000,
1,
101,
2,
152.00);
```
BŁĄD: duplicate key value violates unique constraint "pk_order_items"
  Detail: Key (order_id, line_number)=(1000, 1) already exists.
Composite primary key zablokowal ten insert ponieważ para order_id + line_number musi być unikalna a była ona insertowana wcześniej.

## Zadanie 17
```sql
insert into course.order_items(
order_id,
line_number,
product_id,
quantity,
unit_price)

values(
1000,
2,
999,
1,
99.00);
```
BŁĄD: insert or update on table "order_items" violates foreign key constraint "fk_order_items_product"
  Detail: Key (product_id)=(999) is not present in table "products".

foreign key(product_id) zablokował ten insert podnieważ odnosi się on bezpośrednio do course.products(product_id)
## Zadanie 18
```sql
insert into course.order_items(
order_id,
line_number,
product_id,
quantity,
unit_price)

values(
1000,
2,
100,
0,
99.99);
```
BŁĄD: new row for relation "order_items" violates check constraint "order_items_quantity_check"
  Detail: Failing row contains (1000, 2, 100, 0, 99.99).
  Constraint "check" zablokował ten insert.

 ## Zadanie 19
 ```sql
select 
o.order_id,
o.order_date,
c.customer_id,
c.customer_name,
o.total_amount 
from course.orders o
join course.customers c
on c.customer_id = o.customer_id;
```
Foreign key to constraint który pilnuje, aby klucz obcy w jednej tabeli zawsze wskazywał na istniejący rekord w innej tabeli, aby budować querki nadal potrzebujemy JOINów.

## Zadanie 20
```sql
select
oi.order_id,
oi.line_number,
oi.product_id,
p.product_name,
oi.quantity,
oi.unit_price
from course.order_items oi
join course.products p
on oi.product_id = p.product_id;
```

## Zadanie 21
BŁĄD: update or delete on table "customers" violates foreign key constraint "orders_customer_id_fkey" on table "orders"
  Detail: Key (customer_id)=(1) is still referenced from table "orders".
Baza nie pozwoliła usunąć tego klienta ponieważ istnieje relacja w tabeli orders związana z tym klientem.

## Zadanie 22
Primary key identyfikuje rekord oraz nie pozwala na null oraz duplikaty, co widzimy w przypadku customers.customer_id, mamy tutaj oddzielne ID dla każdego klienta, constraint ten chroni przed ewentualnymi duplikatami. Foreign key natomiast pilnuje relacji między tabelami, jego zadaniem jest pilnowanie aby klucz obcy w jednej tabeli zawsze wskazywał na istniejący rekord w innej tabeli, tak jak w przypadku orders.customer_id.

## Zadanie 23
do napisania w sobote

## Zadanie 24
```sql
create table course.test_payments(
payment_id INT primary key,
order_id INT references course.orders(order_id),
amount numeric(10,2) not null check(amount >= 0),
payment_status TEXT default 'pending'  check (payment_status in('pending','paid','failed','refunded'))
);
```
## Zadanie 25
poprawny insert
```sql
insert into course.test_payments(
payment_id,
order_id,
amount,
payment_status)

values(
101,
1000,
321.32,
'refunded');
```
insert z nieistniejacym order_id
```sql
insert into course.test_payments(
payment_id,
order_id,
amount,
payment_status)

values(
101,
102,
321.32,
'refunded');
```
 insert or update on table "test_payments" violates foreign key constraint "test_payments_order_id_fkey"
  Detail: Key (order_id)=(102) is not present in table "orders". 
 insert z ujemnym amount
 ```sql
insert into course.test_payments(
payment_id,
order_id,
amount,
payment_status)

values(
105,
1002,
-23.22,
'refunded');
```
 BŁĄD: new row for relation "test_payments" violates check constraint "test_payments_amount_check"
 insert z niedozwolonym payment status
 ```sql
insert into course.test_payments(
payment_id,
order_id,
amount,
payment_status)

values(
105,
1002,
42141,
'payment error');
```
BŁĄD: new row for relation "test_payments" violates check constraint "test_payments_payment_status_check"
  Detail: Failing row contains (105, 1002, 42141.00, payment error).
 ## Zadanie 26
 ```sql
insert into course.customers(
customer_id,
customer_name,
email,
country,
signup_date,
acquisition_channel)

values(
8,
'Jan Kowalski',
'jan.kowalski@example.com',
'ES',
current_date,
'linkedin')
```
BŁĄD: new row for relation "customers" violates check constraint "customers_country_check"
Constraint check(country TEXT not null CHECK (country IN ('PL', 'DE', 'FR','US')), wywołał ten błąd, Hiszpanii nie było w krajach które mogłyby się pojawić w tym insercie.
## Zadanie 27
```sql
insert into course.customers(
customer_id,
customer_name,
email,
country,
signup_date,
acquisition_channel)

values(
4,
'F',
'filip@example.com',
'PL',
current_date,
'google'
);
```
BŁĄD: new row for relation "customers" violates check constraint "customers_customer_name_check"
Constraint check (customer_name TEXT not null CHECK (LENGTH(customer_name) >= 2)) wywołał ten błąd, ponieważ długość nazwy musi wynosić przynajmniej 2.

## Zadanie 28
```sql
insert into course.products(
product_id,
product_name,
category,
base_price)

values(
101,
'Power BI tutorial',
'video',
133.22);
```
BŁĄD: new row for relation "products" violates check constraint "products_category_check"
Baza nie przyjęła tej kategorii ponieważ 
```sql
category TEXT not null check(category in ('course','ebook','template','consulting'))
```
tu nie było kategorii "video" przez co baza nie przyjęła tej wartości.
## Zadanie 29
```sql
insert into course.products(
product_id,
product_name,
category,
base_price)

values(
103,
'AWS Fundamentals',
'ebook',
15000
);
```
BŁĄD: new row for relation "products" violates check constraint "products_base_price_check"
Check który zablokował ten insert to
```sql
base_price numeric(10,2) not null  CHECK (base_price >= 0 and base_price <= 10000)
```
z racji tego że 15 000 > 10 000 insert zostal zablokowany
## Zadanie 30
```sql
insert into course.order_items(
order_id,
line_number,
product_id,
quantity,
unit_price)

values(
1002,
0,
100,
1,
144.00);
```
BŁĄD: new row for relation "order_items" violates check constraint "order_items_line_number_check"
Line number powinien się zaczynac od 1 poniewaz oznacza to pozycje zamowienia, a wartość 0 sugerowałaby pozycję która nie istnieje.
## Zadanie 31
```sql
insert into course.order_items(
order_id,
line_number,
product_id,
quantity,
unit_price)

values(
1003,
1,
100,
1,
-1);
```
 BŁĄD: new row for relation "order_items" violates check constraint "order_items_unit_price_check"
 Constraint check zablokował ten insert, konkretnie:
 ```sql
unit_price numeric(10,2) not null check (unit_price >= 0 and unit_price < 10000),
```
## Zadanie 32
```sql
insert into course.order_items(
order_id,
line_number,
product_id,
quantity,
unit_price)

values(
1003,
1,
100,
1,
20000);
```
BŁĄD: new row for relation "order_items" violates check constraint "order_items_unit_price_check"
 ```sql
unit_price numeric(10,2) not null check (unit_price >= 0 and unit_price < 10000),
```
na podstawie produktów które sa w tabeli treningowej ustalony wczesniej limit wydaje sie byc rozsadny
## Zadanie 33
```sql
create table course.test_campaigns(
campaign_id INT primary key,
campaign_name TEXT not null,
start_date DATE not null,
end_date DATE not null check (start_date <= end_date),
budget numeric(10,2) check(budget >= 0)
);
```
## Zadanie 34
poprawny insert
```sql
insert into course.test_campaigns(
campaign_id,
campaign_name,
start_date,
end_date,
budget)

values(
101,
'Anniversary edition',
'2026-10-07',
'2026-10-08',
4134.00);
```
insert w ktorym end_date jest przed start_date
```sql
insert into course.test_campaigns(
campaign_id,
campaign_name,
start_date,
end_date,
budget)

values(
101,
'Anniversary edition',
'2026-10-08',
'2026-10-07',
4134.00);
```
 BŁĄD: new row for relation "test_campaigns" violates check constraint "test_campaigns_check"
 czyli działa :D
 insert z ujemnym budzetem
 ```sql
insert into course.test_campaigns(
campaign_id,
campaign_name,
start_date,
end_date,
budget)

values(
101,
'Anniversary edition',
'2026-10-07',
'2026-10-08',
-2144.00
);
```
 BŁĄD: new row for relation "test_campaigns" violates check constraint "test_campaigns_budget_check"
 działa
 ## Zadanie 35
 ```sql
create table course.test_signups(
signup_id INT GENERATED ALWAYS AS IDENTITY PRIMARY key,
email TEXT not null,
channel TEXT,
signup_date DATE default current_date,
constraint uq_email_channel
	unique(email, channel)
);
```
## Zadanie 36

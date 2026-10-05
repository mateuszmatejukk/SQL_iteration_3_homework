## Zadanie 1
```sql
create schema course_example;
```
## 
Zadanie 2
```sql
create schema if not exists practice;
```
## Zadanie 3
```sql
select schema_name from information_schema.schemata;
```
## Zadanie 4
```sql
create table practice.test_customers (
customer_id INT,
customer_name TEXT
);
```
## Zadanie 5
```sql
select * 
from practice.test_customers;
```
## Zadanie 6
```sql
drop schema practice cascade;
```
```sql
drop schema course_example cascade;
```
## Zadanie 7
Drop schema jest niebezpieczne ponieważ usuwa schemat razem z obiektami w środku.

## Zadanie 8
```sql
select *
from course.customers;
```
```sql
select *
from customers;
```
## Zadanie 9
Różnica między tymi querkami polega na tym, że ta z wskazaniem schematu jest dokładniejsza, jest bardziej czytelna i mniej podatna na pomyłki. 

## Zadanie 1
```sql
alter table course.customers 
add column phone_number text;
```
## Zadanie 2
```sql
alter table course.orders
add column currency text default 'PLN';
```
## Zadanie 3
```sql
select * 
from information_schema.columns
where column_name = 'currency';
```
tak, została dodana, pokazuje sie 
## Zadanie 4
```sql
alter table course.products 
add constraint price_checker 
check (base_price >=0);
```
## Zadanie 5
```sql
alter table course.orders  
add constraint checker_total_amount
check (total_amount >= 0);
```
## Zadanie 6
```sql
alter table course.customers 
rename column phone_number to phone;
```
## Zadanie 7
```sql
alter table course.customers 
drop column phone;
```
## Zadanie 8 
Ponieważ drop column usuwa kolumne razem z danymi w tej kolumnie.

## Zadanie 9
```sql
alter table course.orders
alter column total_amount type numeric(12,2);
```
## Zadanie 10
```sql
alter table course.orders 
drop constraint checker_total_amount;
```
## Zadanie 11
```sql
alter table course.customers 
add column marketing_consent boolean default 'false';
```
## Zadanie 12
```sql
alter table course.customers 
alter column marketing_consent set not null;
```
## Zadanie 13
```sql
alter table course.customers 
alter column marketing_consent drop not null;
```
## Zadanie 14
```sql
alter table course.customers 
drop column marketing_consent;
```
## Zadanie 15
Dodanie nowej kolumny not null bywa problematyczne poniewaz istniejace wiersze musza otrzymac jakas wartosc w nowej kolumnie a bez klauzuli default baza nie wie co wstawić.

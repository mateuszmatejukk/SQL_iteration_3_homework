## Zadanie 1
Delete usuwa wiersze z tabeli, tabela zostaje.
## Zadanie 2
Truncate szybko usuwa wszystkie wiersze z tabeli, nie używa się tutaj where.
## Zadanie 3
Drop table usuwa cały obiekt, po drop table nie ma żadnych danych ani struktury tabeli.
## Zadanie 4
```sql
create table course.delete_practice(
id INT,
name TEXT
);
```
## Zadanie 5
```sql
insert into course.delete_practice(
id,
name)

values
(1,'Anna Kowalska'),
(2,'Kamil Nowak'),
(3,'Piotr Kołodziej');
```
## Zadanie 6
```sql
delete from course.delete_practice
where id = 1;
```
## Zadanie 7
```sql
select * from course.delete_practice;
```
tak, istnieje, rekordy dla id = 2 oraz id =3 nadal istnieja
## Zadanie 8
```sql
truncate table course.delete_practice;
```
## Zadanie 9
```sql
select * from course.delete_practice;
```
nie ma nic
## Zadanie 10
```sql
drop table course.delete_practice;
```
## Zadanie 11
Najczęściej delete używa się przy usuwaniu konkretnego rekordu, truncate przy szybkim czyszczeniu tabel technicznych, stagingowych albo tymczasowych natomiast drop table używa się kiedy chcemy usunąć całą tabelę wraz ze strukturą tabeli.

## Zadanie 12
```sql
create table course.drop_parent(
customer_id INT,
order_id INT primary KEY);

create table course.drop_child(
order_id INT references course.drop_parent(order_id),
order_item_id INT);
```
## Zadanie 13
 BŁĄD: nie można usunąć tabela course.drop_parent ponieważ inne obiekty zależą od niego
  Detail: ograniczenie drop_child_order_id_fkey na tabela course.drop_child zależy od tabela course.drop_parent
Operacja się nie uda ponieważ w tabeli drop_parent istnieje obiekt od którego jest zależna inna kolumna w drugiej tabeli.

## Zadanie 14
```sql
drop table course.drop_parent cascade;
```
Cascade jest wygodne ale niebezpieczne ponieważ może usunąć więcej niż planuję.

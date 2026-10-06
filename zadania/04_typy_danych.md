## Zadanie 1
customer_id - INT
customer_name - TEXT,
email- TEXT,
country - TEXT,
signup_date DATE

## Zadanie 2
order_id- INT
customer_id - INT,
order_date DATE,
status TEXT,
total_amount NUMERIC(10,2)

## Zadanie 3
```sql
create table course.type_practice (
incident_id INT,
incident_type TEXT,
repair_cost numeric(10,2),
incident_date DATE,
incident_hour timestamp,
incident_is_active BOOLEAN
);
```
## Zadanie 4
```sql
insert into course.type_practice (
incident_id,
incident_type,
repair_cost,
incident_date,
incident_hour,
incident_is_active)

values(
1,
'LEAK',
4319.10,
'2026-10-05',
'2026-10-05 10:30:00',
false);
```
## Zadanie 5
ERROR: invalid input syntax for type integer: "text"
## Zadanie 6
ERROR: invalid input syntax for type numeric: "text"
## Zadanie 8
Użyłbym BIGINT zamiast zwykłego INTa kiedy miałbym do czynienia z bardzo dużymi tabelami, systemami, gdzie ID mogą przekraczać zakres zwykłego INTa.
## Zadanie 9
Ponieważ double precision jest dobre dla pomiarów np.temperatur, nie dla pieniędzy, dokładność przy numeric jest większa niż przy double precision.
## Zadanie 10
```sql
create table course.type_decision_practice (
event_id BIGINT,
event_name TEXT,
event_date DATE,
created_at TIMESTAMP,
amount numeric(10,2),
is_active BOOLEAN
);
```
## Zadanie 11
```sql
insert into course.type_decision_practice
(event_id, 
event_name,
event_date,
created_at,
amount,
is_active)

values(
51905190332,
'implementation',
'2026-10-06',
'2026-10-06 15:30:00',
41234.50,
true);
```
## Zadanie 12
Date przechowuje jedynie datę zdarzenia, natomiast timestamp przechowuje datę oraz godzinę jakiegoś eventu.
## Zadanie 13
UUID warto rozważyć przy sytuacjach kiedy dane pochodzą z wielu systemów, ID musi być unikalne globalnie, bądź gdy system źródłowy już wysyła UUID.
## Zadanie 14
JSONB jest dobry przy warstwie raw/bronze, jest przydatny kiedy zapisujemy surową odpowiedź z API, natomiast do raportowania/analityki należy raczej używać normalnych kolumn w kazdej tabeli z racji na dokładność raportów.

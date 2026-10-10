## Zadanie 1
```sql
create index idx_orders_order_date
on course.orders(order_date);
```
## Zadanie 2
```sql
create index idx_orders_customer_id
on course.orders(customer_id);
```
## Zadanie 3
```sql
create index idx_order_items_order_id
on course.order_items(order_id);
```
## Zadanie 4
```sql
create index idx_order_items_product_id
on course.order_items(product_id);
```
## Zadanie 5
```sql
create index idx_orders_customer_date
on course.orders(customer_id, order_date)
```
## Zadanie 6
```sql
explain analyze
select *
from course.orders
where order_date >= '2026-01-01';
```
## Zadanie 7
```sql
explain 
select
c.customer_id,
c.customer_name,
c.email,
c.country,
c.signup_date,
c.acquisition_channel,
o.order_id,
o.customer_id,
o.order_date,
o.status,
o.total_amount
from course.customers c
join course.orders o
on c.customer_id = o.customer_id;
```
QUERY PLAN|Hash Join  (cost=1.20..12.94 rows=9 width=740)

## Zadanie 8
```sql
CREATE TABLE course.cars (
    car_id BIGINT,
    make TEXT,
    model TEXT,
    production_year INT,
    capacity NUMERIC(3, 1)
);
```
## Zadanie 9
```sql
INSERT INTO course.cars (
    car_id,
    make,
    model,
    production_year,
    capacity
)
SELECT
    gs AS car_id,
    CASE (gs % 8)
        WHEN 0 THEN 'Toyota'
        WHEN 1 THEN 'Ford'
        WHEN 2 THEN 'BMW'
        WHEN 3 THEN 'Audi'
        WHEN 4 THEN 'Honda'
        WHEN 5 THEN 'Skoda'
        WHEN 6 THEN 'Kia'
        ELSE 'Volkswagen'
    END AS make,
    CASE (gs % 10)
        WHEN 0 THEN 'Corolla'
        WHEN 1 THEN 'Focus'
        WHEN 2 THEN 'X5'
        WHEN 3 THEN 'A4'
        WHEN 4 THEN 'Civic'
        WHEN 5 THEN 'Octavia'
        WHEN 6 THEN 'Sportage'
        WHEN 7 THEN 'Golf'
        WHEN 8 THEN 'Yaris'
        ELSE 'Passat'
    END AS model,
    2000 + (gs % 25) AS production_year,
    ((10 + (gs % 30)) / 10.0)::NUMERIC(3, 1) AS capacity
FROM generate_series(1, 1000000) AS gs;
```
## Zadanie 10
```sql
select * from course.cars;
```
## Zadanie 11
QUERY PLAN                    
------------------------------
Gather  (cost=1000.00..16156.6
  Workers Planned: 2          
  Workers Launched: 2         
  Buffers: shared hit=7813    
  ->  Parallel Seq Scan on car
        Filter: ((make = 'Toyo
        Rows Removed by Filter
        Buffers: shared hit=78
Planning Time: 0.067 ms       
Execution Time: 74.002 ms     

SEQ SCAN
## Zadanie 12
```sql
create index idx_cars_make_model_year
on course.cars(make, model, production_year);
```
## Zadanie 13
QUERY PLAN                                                                                                                              |
----------------------------------------------------------------------------------------------------------------------------------------+
Bitmap Heap Scan on cars  (cost=11.06..1652.13 rows=520 width=29) (actual time=1.218..4.467 rows=5000.00 loops=1)                       |
  Recheck Cond: ((make = 'Toyota'::text) AND (model = 'Corolla'::text) AND (production_year = 2020))                                    |
  Heap Blocks: exact=5000                                                                                                               |
  Buffers: shared hit=5000 read=7                                                                                                       |
  ->  Bitmap Index Scan on idx_cars_make_model_year  (cost=0.00..10.93 rows=520 width=0) (actual time=0.710..0.710 rows=5000.00 loops=1)|
        Index Cond: ((make = 'Toyota'::text) AND (model = 'Corolla'::text) AND (production_year = 2020))                                |
        Index Searches: 1                                                                                                               |
        Buffers: shared read=7                                                                                                          |
Planning:                                                                                                                               |
  Buffers: shared hit=21 read=1                                                                                                         |
Planning Time: 1.891 ms                                                                                                                 |
Execution Time: 4.646 ms                                                                                                                |
teraz zamiast seq scan był index scan, execution time zmniejszył się.

## Zadanie 14
```sql
create index idx_cars_model
on course.cars(model);
```
QUERY PLAN                                                                                                                                     |
-----------------------------------------------------------------------------------------------------------------------------------------------+
Bitmap Heap Scan on cars  (cost=1135.23..10218.23 rows=101600 width=29) (actual time=4.527..22.657 rows=100000.00 loops=1)                     |
  Recheck Cond: (model = 'Corolla'::text)                                                                                                      |
  Heap Blocks: exact=7813                                                                                                                      |
  Buffers: shared hit=7849 read=109                                                                                                            |
  ->  Bitmap Index Scan on idx_cars_make_model_year  (cost=0.00..1109.83 rows=101600 width=0) (actual time=3.818..3.818 rows=100000.00 loops=1)|
        Index Cond: (model = 'Corolla'::text)                                                                                                  |
        Index Searches: 17                                                                                                                     |
        Buffers: shared hit=36 read=109                                                                                                        |
Planning:                                                                                                                                      |
  Buffers: shared hit=15 read=1                                                                                                                |
Planning Time: 1.313 ms                                                                                                                        |
Execution Time: 25.154 ms                                                                                                                      |
## Zadanie 15
```sql
create index idx_orders_paid_order_date
on course.orders(order_date)
where status = 'paid';
```
## Zadanie 16
```sql
create index idx_customers_lower_email
on course.customers(LOWER(email));
```
## Zadanie 17
```sql
select
indexname,
indexdef
from pg_indexes
where schemaname = 'course'
and tablename = 'orders';
```
## Zadanie 18
```sql
drop index if exists course.idx_cars_make_model_year;
```

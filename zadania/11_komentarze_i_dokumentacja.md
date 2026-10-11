## Zadanie 1
```sql
COMMENT ON TABLE course.customer IS
'One row represents one customer data.';
```
## Zadanie 2
```sql
COMMENT ON TABLE course.orders IS
'One row represents one customer order.';
```
## Zadanie 3
```sql
COMMENT ON TABLE course.order_items IS
'One row represents one order item.';
```
## Zadanie 4
```sql
COMMENT ON COLUMN course.orders.total_amount IS
'Total order amount before additional reporting transformations.';
```
## Zadanie 5
```sql
COMMENT ON COLUMN course.order_items.quantity IS
'Number of units ordered in order item.';
```
## Zadanie 6
```sql
COMMENT ON COLUMN course.products.base_price IS
'Base price of the product';
```
## Zadanie 7
Grain mówi co oznacza jeden wiersz.

## Zadanie 8
Dobrze opisany grain tabeli zmniejsza ryzyko blednych raportow.

### Solution

```sql
SELECT 
    c.first_name,
    c.last_name,
    c.city,
    o.order_details
FROM customers c
LEFT JOIN orders o ON o.cust_id = c.id
ORDER BY c.first_name ASC, o.order_details ASC;
```

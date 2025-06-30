### Create Tables

```sql
CREATE TABLE customers (
    id BIGINT PRIMARY KEY,
    first_name TEXT,
    last_name TEXT,
    city TEXT,
    address TEXT,
    phone_number TEXT
);

CREATE TABLE orders (
    id BIGINT PRIMARY KEY,
    cust_id BIGINT,
    order_date DATE,
    order_details TEXT,
    total_order_cost BIGINT,
    FOREIGN KEY (cust_id) REFERENCES customers(id)
);
```

### Insert Data

```sql
INSERT INTO customers (id, first_name, last_name, city, address, phone_number) VALUES
(1, 'Rushi', 'Malvadkar', 'Pune', '101 Tech Park', '9001234567'),
(2, 'Anaya', 'Sharma', 'Mumbai', '202 Business Bay', '9007654321'),
(3, 'Karan', 'Desai', 'Bangalore', '303 IT Hub', '9123456789'),
(4, 'Neha', 'Patil', 'Hyderabad', '404 Green Valley', '9876543210');

INSERT INTO orders (id, cust_id, order_date, order_details, total_order_cost) VALUES
(1, 2, '2025-06-01', 'Notebook', 250),
(2, 2, '2025-06-05', 'Pen Set', 100),
(3, 3, '2025-06-07', 'Wireless Mouse', 550);
```

## Customer Orders Overview – SQL Kata

### Objective

Write a SQL query to retrieve a list of all customers, including their **first name**, **last name**, and **city**, along with their **order details** if they have made any purchases.

Ensure the result includes customers **even if they haven't placed any orders**.
Sort the final output by **first name** and then by **order details**, both in ascending order.

---

### Schema

#### Table: `customers`

| Column        | Type   | Description           |
| ------------- | ------ | --------------------- |
| id            | BIGINT | Primary key           |
| first\_name   | TEXT   | Customer's first name |
| last\_name    | TEXT   | Customer's last name  |
| city          | TEXT   | City of the customer  |
| address       | TEXT   | Full address          |
| phone\_number | TEXT   | Contact number        |

#### Table: `orders`

| Column             | Type   | Description              |
| ------------------ | ------ | ------------------------ |
| id                 | BIGINT | Primary key              |
| cust\_id           | BIGINT | Foreign key to customers |
| order\_date        | DATE   | Date of the order        |
| order\_details     | TEXT   | Product or item ordered  |
| total\_order\_cost | BIGINT | Cost of the order        |

---

### Expected Output

| first\_name | last\_name | city      | order\_details |
| ----------- | ---------- | --------- | -------------- |
| Anaya       | Sharma     | Mumbai    | Notebook       |
| Anaya       | Sharma     | Mumbai    | Pen Set        |
| Karan       | Desai      | Bangalore | Wireless Mouse |
| Neha        | Patil      | Hyderabad | *(null)*       |
| Rushi       | Malvadkar  | Pune      | *(null)*       |

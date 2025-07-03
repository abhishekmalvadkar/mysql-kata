# 🧠 SQL Deep Dive: Finding Users Active for 3 Consecutive Days using CTEs in MySQL

## 🚀 Problem Statement

You’re given a user activity log table named `sf_events`, where each row records a user’s activity on a particular day.

Your goal:

> Write a SQL query to return user IDs of users who were active for at least three consecutive days.

---

## 📊 Table Structure

```sql
CREATE TABLE sf_events (
  account_id TEXT,
  record_date DATE,
  user_id TEXT
);
```

📝 Sample data:

| user\_id | record\_date |
| -------- | ------------ |
| A        | 2024-07-01   |
| A        | 2024-07-02   |
| A        | 2024-07-03   |
| A        | 2024-07-05   |
| B        | 2024-07-02   |
| B        | 2024-07-03   |
| B        | 2024-07-05   |

---

## 🔍 Naive Attempt (Why It Fails)

Let’s say you try this query first:

```sql
SELECT 
    user_id,
    COUNT(record_date)
FROM sf_events
GROUP BY user_id
HAVING COUNT(record_date) >= 3;
```

🚫 **Problem**: This only checks the number of **records**, not whether the dates are **consecutive**.

---

## ✅ Correct Solution Using CTEs (MySQL 8+)

We’ll break it down step-by-step so you can visualize and understand each part.

---

## 🧱 Step 1: Assign Row Numbers (`ranked_dates` CTE)

Use `ROW_NUMBER()` to assign an increasing index for each user's sorted dates:

```sql
WITH ranked_dates AS (
  SELECT 
    user_id,
    record_date,
    ROW_NUMBER() OVER (PARTITION BY user_id ORDER BY record_date) AS rn
  FROM sf_events
)
```

🔍 Output:

| user\_id | record\_date | rn |
| -------- | ------------ | -- |
| A        | 2024-07-01   | 1  |
| A        | 2024-07-02   | 2  |
| A        | 2024-07-03   | 3  |
| A        | 2024-07-05   | 4  |
| B        | 2024-07-02   | 1  |
| B        | 2024-07-03   | 2  |
| B        | 2024-07-05   | 3  |

---

## 🧠 Step 2: Identify Consecutive Groups (`grouped_dates` CTE)

Subtract the row number (`rn`) from the date. If the result is the same across rows, they belong to a **consecutive group**.

```sql
, grouped_dates AS (
  SELECT 
    user_id,
    record_date,
    DATE_SUB(record_date, INTERVAL rn DAY) AS grp
  FROM ranked_dates
)
```

🔍 Output:

| user\_id | record\_date | grp        |
| -------- | ------------ | ---------- |
| A        | 2024-07-01   | 2024-06-30 |
| A        | 2024-07-02   | 2024-06-30 |
| A        | 2024-07-03   | 2024-06-30 |
| A        | 2024-07-05   | 2024-07-01 |
| B        | 2024-07-02   | 2024-07-01 |
| B        | 2024-07-03   | 2024-07-01 |
| B        | 2024-07-05   | 2024-07-02 |

🧩 Explanation:
The same `grp` value = **consecutive** days. Why? Because `record_date - rn` stays constant in a sequence.

---

## 🎯 Step 3: Count Streaks (`consecutive_streaks` CTE)

Now group by `user_id` and `grp`, then filter groups with at least 3 records:

```sql
, consecutive_streaks AS (
  SELECT 
    user_id,
    COUNT(*) AS streak_length
  FROM grouped_dates
  GROUP BY user_id, grp
  HAVING COUNT(*) >= 3
)
```

🔍 Output:

| user\_id | streak\_length |
| -------- | -------------- |
| A        | 3              |

---

## 🏁 Final Result

Select distinct users with valid streaks:

```sql
SELECT DISTINCT user_id
FROM consecutive_streaks;
```

📦 Final Output:

| user\_id |
| -------- |
| A        |

---

## 📘 Summary

✅ What you learned:

* How `ROW_NUMBER()` helps in detecting date patterns
* How `DATE_SUB(record_date, INTERVAL row_number DAY)` is a smart trick to group consecutive dates
* How breaking problems down into CTEs improves clarity and debuggability

---

## 🧪 Try It Yourself

Use a test dataset like this:

```sql
INSERT INTO sf_events(user_id, record_date) VALUES
('A', '2024-07-01'),
('A', '2024-07-02'),
('A', '2024-07-03'),
('A', '2024-07-05'),
('B', '2024-07-02'),
('B', '2024-07-03'),
('B', '2024-07-05');
```

Then run the full query with CTEs and observe the result.

---

## 🔎 Deep Dive: How `DATE_SUB(record_date, INTERVAL row_number DAY)` Detects Consecutive Days

This is the core trick of the entire query. Here's how it works step by step.

---

### 🧠 Goal Recap

We want to group together rows that represent **consecutive days** of user activity.

### 🔢 Key Idea

For each row, we assign a `row_number` and subtract that many days from `record_date`.
If the user was active on **consecutive days**, the result (`record_date - row_number`) will be **constant**.

---

### 🔧 Example Walkthrough

| user\_id | record\_date | row\_number | record\_date - row\_number |
| -------- | ------------ | ----------- | -------------------------- |
| A        | 2024-07-01   | 1           | 2024-06-30                 |
| A        | 2024-07-02   | 2           | 2024-06-30                 |
| A        | 2024-07-03   | 3           | 2024-06-30                 |
| A        | 2024-07-05   | 4           | 2024-07-01                 |

🧠 All three consecutive days (July 1–3) subtract their row number and give `June 30`. This means they're part of the same **consecutive streak group**.

But July 5 breaks the pattern and forms its own group.

---

### 🎯 Why It Works

* If dates are consecutive, subtracting increasing row numbers keeps the difference **constant**.
* This gives a **group identifier** for each consecutive streak.
* Later, we `GROUP BY` this identifier and count how many dates are in each group.

---

### 🧩 Visualizing It

```
record_date:        2024-07-01   2024-07-02   2024-07-03   2024-07-05
row_number:                1            2            3            4
DATE_SUB result:     2024-06-30   2024-06-30   2024-06-30   2024-07-01
Group Identifier:         A            A            A            B
```

Then:

```sql
GROUP BY user_id, group_identifier
HAVING COUNT(*) >= 3
```

Gives you users active for **3 or more consecutive days**.

---

### ✅ Summary of the Trick

| Expression                               | Purpose                                  |
| ---------------------------------------- | ---------------------------------------- |
| `ROW_NUMBER() OVER (...)`                | Indexes the dates in order per user      |
| `DATE_SUB(record_date, INTERVAL rn DAY)` | Creates a consistent value for sequences |
| `GROUP BY user_id, diff`                 | Groups consecutive day sequences         |
| `HAVING COUNT(*) >= 3`                   | Filters for users with streaks           |

---

This clever trick avoids complex joins or recursive CTEs and gives a clean, fast way to detect date streaks using window functions.

## 💬 Final Thoughts

Understanding consecutive date problems is crucial in activity tracking, retention analysis, and time-series data queries. With window functions and CTEs, even complex problems become clean, readable, and scalable.

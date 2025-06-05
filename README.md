

# 📘 PostgreSQL এবং তার প্রধান কনসেপ্টসমূহ

## 1. PostgreSQL কী?

PostgreSQL হলো একটি ওপেন সোর্স রিলেশনাল ডেটাবেস ম্যানেজমেন্ট সিস্টেম যা ACID properties মেনে চলে। এটি SQL এবং JSON উভয় ফরম্যাটে ডেটা ম্যানেজ করতে পারে। এটি বড় অ্যাপ্লিকেশন, ওয়েব সার্ভিস এবং বিশ্লেষণধর্মী ডেটা হ্যান্ডল করার জন্য খুবই উপযুক্ত।

### 🎯 উদাহরণ:
আপনি যদি একটি ই-কমার্স অ্যাপ বানান যেখানে হাজার হাজার প্রোডাক্ট আছে, PostgreSQL ব্যবহার করে আপনি সেই প্রোডাক্টগুলোর ইনভেন্টরি, অর্ডার এবং ইউজার তথ্য সঠিকভাবে সংরক্ষণ করতে পারবেন।

---

## 2. PostgreSQL-এ Schema এর উদ্দেশ্য কী?

Schema হচ্ছে ডেটাবেসের মধ্যে একটি লজিক্যাল কাঠামো বা নেমস্পেস যা ডেটা অবজেক্টগুলো (যেমন টেবিল, ভিউ, ফাংশন ইত্যাদি) সংগঠিত করে।

### ✅ উদ্দেশ্য:
- ডেটা আলাদা করে রাখার ব্যবস্থা
- মাল্টি-ইউজার এনভায়রনমেন্টে নিরাপত্তা নিশ্চিত করা
- বড় সিস্টেমে মডিউলার স্ট্রাকচার তৈরি করা

### 🎯 উদাহরণ:
```sql
CREATE SCHEMA sales;

CREATE TABLE sales.orders (
  id SERIAL PRIMARY KEY,
  customer_id INT,
  total_amount NUMERIC
);
```

---

## 3. Primary Key ও Foreign Key কী?

### ✅ Primary Key:
একটি ইউনিক কলাম যা প্রতিটি রেকর্ডকে আলাদা করে শনাক্ত করে।

### ✅ Foreign Key:
অন্য টেবিলের Primary Key এর সাথে সম্পর্ক স্থাপন করে, যা রেফারেন্স ইনটিগ্রিটি নিশ্চিত করে।

### 🎯 উদাহরণ:
```sql
CREATE TABLE customers (
  id SERIAL PRIMARY KEY,
  name TEXT
);

CREATE TABLE orders (
  id SERIAL PRIMARY KEY,
  customer_id INT REFERENCES customers(id)
);
```

এখানে `orders.customer_id` হলো `customers.id` এর Foreign Key।

---

## 4. VARCHAR ও CHAR এর পার্থক্য কী?

- `VARCHAR(n)` হচ্ছে ভ্যারিয়েবল দৈর্ঘ্যের টেক্সট। এটা শুধু প্রয়োজন অনুযায়ী স্পেস নেয়।
- `CHAR(n)` হচ্ছে ফিক্সড দৈর্ঘ্যের টেক্সট। এটা সবসময় n চরেক্টার স্পেস নেয়।

### 🎯 উদাহরণ:
```sql
CREATE TABLE products (
  code VARCHAR(10),
  category CHAR(10)
);
```

যদি `code = 'P001'` হয় এবং `category = 'Toy'`, তাহলে `code` হবে `'P001'` আর `category` হবে `'Toy       '` (7 টি স্পেস সহ)।

---

## 5. WHERE clause এর কাজ কী?

`WHERE` ক্লজ দিয়ে ডেটা ফিল্টার করা যায় যাতে নির্দিষ্ট শর্ত অনুযায়ী ডেটা রিটার্ন হয়।

### 🎯 উদাহরণ:
```sql
SELECT * FROM employees
WHERE department = 'HR' AND salary > 50000;
```

এখানে শুধুমাত্র HR ডিপার্টমেন্টের এবং যাদের স্যালারি ৫০,০০০ টাকার বেশি তাদের তথ্য রিটার্ন হবে।

---

## 6. LIMIT ও OFFSET এর ব্যবহার কী?

- `LIMIT`: কতগুলো রেকর্ড রিটার্ন করতে চান তা নির্ধারণ করে।
- `OFFSET`: কতগুলো রেকর্ড স্কিপ করবে তা নির্ধারণ করে।

### 🎯 উদাহরণ (pagination):
```sql
SELECT * FROM products
LIMIT 10 OFFSET 20;
```

এটি ২১তম রেকর্ড থেকে শুরু করে ১০টি রেকর্ড রিটার্ন করবে। সাধারণত pagination-এ ব্যবহৃত হয়।

---

## 7. UPDATE স্টেটমেন্ট দিয়ে ডেটা কিভাবে পরিবর্তন করা যায়?

`UPDATE` দিয়ে একটি টেবিলের এক বা একাধিক রেকর্ড পরিবর্তন করা যায়। `SET` দিয়ে নতুন মান সেট করা হয় এবং `WHERE` ক্লজ দিয়ে নির্দিষ্ট রেকর্ড টার্গেট করা হয়।

### 🎯 উদাহরণ:
```sql
UPDATE employees
SET salary = salary + 5000
WHERE department = 'IT';
```

এটি IT ডিপার্টমেন্টের সবার স্যালারিতে ৫০০০ টাকা যোগ করবে।

---

## 8. JOIN অপারেশনের গুরুত্ব ও কাজ কী?

`JOIN` দিয়ে একাধিক টেবিলের ডেটা যুক্ত করে একটি ইউনিফাইড ভিউ তৈরি করা যায়।

### 🎯 উদাহরণ (INNER JOIN):
```sql
SELECT e.name, d.name AS department
FROM employees e
JOIN departments d ON e.dept_id = d.id;
```

এটি `employees` এবং `departments` টেবিলকে `dept_id` এর মাধ্যমে যুক্ত করে।

---

## 9. GROUP BY ক্লজ ও এটি কীভাবে Aggregation-এ সাহায্য করে?

`GROUP BY` একই ধরনের মানকে একত্রিত করে এবং সেটির উপর Aggregate ফাংশন (যেমন COUNT, SUM) প্রয়োগ করতে সাহায্য করে।

### 🎯 উদাহরণ:
```sql
SELECT department, COUNT(*) AS total
FROM employees
GROUP BY department;
```

এখানে প্রতিটি ডিপার্টমেন্টে কতজন কর্মচারী আছে তা গণনা করা হয়।

---

## 10. Aggregate Functions (COUNT, SUM, AVG) কীভাবে কাজ করে?

Aggregate functions অনেকগুলো রেকর্ডের উপর ভিত্তি করে একটি একক রেজাল্ট রিটার্ন করে।

### 🎯 উদাহরণ:
```sql
SELECT COUNT(*) FROM orders; -- মোট অর্ডার সংখ্যা

SELECT SUM(total_amount) FROM orders; -- মোট বিক্রির পরিমাণ

SELECT AVG(salary) FROM employees; -- গড় স্যালারি
```

এই ফাংশনগুলো রিপোর্টিং এবং বিশ্লেষণে ব্যাপকভাবে ব্যবহৃত হয়।

---

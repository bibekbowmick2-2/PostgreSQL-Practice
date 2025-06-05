### 1️⃣ PostgreSQL কী?

PostgreSQL হলো একটি শক্তিশালী, ওপেন সোর্স রিলেশনাল ডেটাবেস ম্যানেজমেন্ট সিস্টেম (RDBMS)। এটি স্ট্যান্ডার্ড SQL এবং JSON উভয় ধরনের ডেটার সাপোর্ট প্রদান করে। এটি নির্ভরযোগ্যতা, ডেটা ইন্টিগ্রিটি এবং এক্সটেনসিবিলিটির জন্য পরিচিত।

এর বৈশিষ্ট্যগুলো হলো:

- ট্রানজ্যাকশনের জন্য ACID কমপ্লায়েন্স
- কমপ্লেক্স কুয়েরি ও ভিউ, ফাংশন, স্টোরড প্রোসিজার সাপোর্ট
- JSON, XML, এবং বিভিন্ন ইনডেক্সিং সিস্টেম সাপোর্ট

### 2️⃣ Schema এর উদ্দেশ্য কী PostgreSQL-এ?

Schema হলো একটি নেমস্পেস যেখানে টেবিল, ভিউ, ফাংশন ইত্যাদি রাখা হয়। এটি ব্যবহার হয়:

- ডেটা অবজেক্টগুলোকে লজিকালি সংগঠিত করতে
- একাধিক ইউজারের জন্য নিরাপত্তা নিশ্চিত করতে
- মাল্টি-টেনেন্ট সাপোর্ট করতে

উদাহরণস্বরূপ:

```sql
CREATE SCHEMA hr;
CREATE TABLE hr.employees (
    id SERIAL PRIMARY KEY,
    name TEXT
);
```

### 3️⃣ Primary Key ও Foreign Key কী?

**Primary Key** হলো একটি ইউনিক ফিল্ড যা প্রতিটি রেকর্ডকে আলাদা করে চিনতে সাহায্য করে। এটি কখনো NULL হতে পারে না।

**Foreign Key** হলো এমন একটি ফিল্ড যা অন্য একটি টেবিলের Primary Key-র সাথে সম্পর্ক তৈরি করে।

```sql
CREATE TABLE departments (
    id SERIAL PRIMARY KEY,
    name TEXT
);

CREATE TABLE employees (
    id SERIAL PRIMARY KEY,
    dept_id INT REFERENCES departments(id)
);
```

এখানে `dept_id` হলো `departments` টেবিলের `id`-র প্রতি রেফারেন্স।

### 4️⃣ VARCHAR ও CHAR এর মধ্যে পার্থক্য কী?

- `VARCHAR(n)` হলো ভ্যারিয়েবল দৈর্ঘ্যের ক্যারেক্টার ফিল্ড
- `CHAR(n)` হলো ফিক্সড দৈর্ঘ্যের ফিল্ড

যেখানে `VARCHAR` কম জায়গা ব্যবহার করে, `CHAR` নির্দিষ্ট দৈর্ঘ্য নিশ্চিত করে স্পেস দিয়ে পূরণ করে।

```sql
CREATE TABLE test (
    a VARCHAR(5),
    b CHAR(5)
);
```

`'abc'` দিলে `a` থাকবে `'abc'`, আর `b` হবে `'abc  '` (স্পেসসহ)।

### 5️⃣ WHERE clause কেন ব্যবহার হয়?

`WHERE` ক্লজ ব্যবহার করে ডেটাবেজ থেকে নির্দিষ্ট শর্ত অনুযায়ী রেকর্ড ফিল্টার করা যায়।

উদাহরণ:

```sql
SELECT * FROM students WHERE grade = 'A';
```

এখানে শুধুমাত্র `grade = 'A'` এমন ছাত্রদের তথ্য ফেরত আসবে।

এছাড়াও AND, OR, NOT, LIKE, IS NULL ইত্যাদি অপারেটর ব্যবহারের মাধ্যমে আরো জটিল কুয়েরি লেখা যায়।

```sql
SELECT * FROM employees
WHERE department = 'HR' AND salary > 60000;
```
---

```

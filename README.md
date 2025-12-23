# Vehicle Rental System

> **SQL project:** A simple Vehicle Rental System database demonstrating table design, relationships, and useful SQL queries (JOIN, EXISTS, WHERE, GROUP BY, HAVING).

---

## 🔎 Project Overview

This repository contains SQL schema and example queries for a Vehicle Rental System. It's aimed at beginners who want to learn core relational concepts and practice real-world queries.

* **Database**: PostgreSQL 
* **Focus**: schema design, constraints, relations, sample queries

---

## 🧱 Tech / Tools

* PostgreSQL (any modern version)
* Any SQL client (psql, pgAdmin)
* Optional: drawSQL / any ERD tool for visuals

---

## 📁 Database Schema

### 1. Create database

```sql
CREATE DATABASE vehicle_rental_system;
```

### 2. `users` table

```sql
CREATE TABLE users (
  user_id SERIAL PRIMARY KEY,
  name VARCHAR(30) NOT NULL,
  email VARCHAR(255) UNIQUE NOT NULL,
  password VARCHAR(255) NOT NULL,
  phone VARCHAR(32),
  role VARCHAR(10) CHECK (role IN ('Customer', 'Admin')) DEFAULT 'Customer'
);
```

### 3. `vehicles` table

> Stores details of vehicles available for rent.

```sql
CREATE TABLE vehicles (
  vehicle_id SERIAL PRIMARY KEY,
  vehicle_name VARCHAR(50),
  type VARCHAR(10) CHECK (type IN ('car', 'bike', 'truck')),
  model VARCHAR(50),
  registration_number VARCHAR(50) UNIQUE,
  rental_price_per_day NUMERIC(10,2) NOT NULL,
  status VARCHAR(15) CHECK (status IN ('available', 'rented', 'maintenance')) DEFAULT 'available'
);
```

### 4. `bookings` table

> Stores booking records and links users ↔ vehicles.

```sql
CREATE TABLE bookings (
  booking_id SERIAL PRIMARY KEY,
  user_id INTEGER REFERENCES users(user_id) ON DELETE CASCADE,
  vehicle_id INTEGER REFERENCES vehicles(vehicle_id) ON DELETE CASCADE,
  start_date DATE NOT NULL,
  end_date DATE NOT NULL,
  booking_status VARCHAR(15) CHECK (booking_status IN ('pending', 'confirmed', 'completed', 'cancelled')) DEFAULT 'pending',
  total_cost NUMERIC(10,2)
);
```

> Tip: You can calculate `total_cost` on insert/update via application logic or a trigger that multiplies days × `rental_price_per_day`.

---

## ✅ Example Queries (with short explanations)

### Query 1 -- INNER JOIN (all booking details)

```sql
SELECT
  b.booking_id,
  u.name AS customer_name,
  v.vehicle_name,
  b.start_date,
  b.end_date,
  b.booking_status AS status
FROM bookings b
INNER JOIN users u ON b.user_id = u.user_id
INNER JOIN vehicles v ON b.vehicle_id = v.vehicle_id;
```

**ব্যাখ্যা এই query তে INNER JOIN ব্যবহার করা হয়েছে যাতে bookings table-এর সাথে users এবং vehicles table connect করা যায়। এতে করে কোন user কোন vehicle বুক করেছে, start date, end date আর booking status—সব একসাথে দেখা যায় INNER JOIN ব্যবহার করার কারণে শুধু সেই booking গুলোই দেখাবে যেগুলোর user আর vehicle দুইটাই valid আছে।

---

### Query 2 -- NOT EXISTS (vehicles with no bookings)

```sql
SELECT *
FROM vehicles v
WHERE NOT EXISTS (
  SELECT 1
  FROM bookings b
  WHERE b.vehicle_id = v.vehicle_id
);
```

**ব্যাখ্যা : যেসব vehicle এখনো কেউ বুক করে নাই সেগুলো বের করতে `NOT EXISTS` ব্যবহার করা হয়েছে। এখানে NOT EXISTS ব্যবহার করা হয়েছে check করার জন্য কোনো vehicle bookings table-এ আছে কিনা। যদি কোনো vehicle-এর জন্য booking না থাকে, তাহলে সেটা result-এ দেখাবে।
এটা সাধারণত available বা unused vehicle বের করার জন্য কাজে লাগে।

---

### Query 3--WHERE (filter available cars)

```sql
SELECT *
FROM vehicles
WHERE type = 'car' AND status = 'available';
```

**ব্যাখ্যা : কেবল সেই গাড়িগুলো দেখাবে যেগুলো `type = 'car'` এবং `status = 'available'`। WHERE clause ব্যবহার করে condition অনুযায়ী data filter করা হয়। এখানে আমরা বলছি—type হবে car এবং status হবে available। মানে শুধু যেসব car এখন ভাড়ার জন্য available আছে সেগুলোই দেখাবে।

---

### Query 4 -- GROUP BY & HAVING (popular vehicles)

```sql
SELECT
  v.vehicle_name,
  COUNT(b.booking_id) AS total_bookings
FROM bookings b
JOIN vehicles v ON b.vehicle_id = v.vehicle_id
GROUP BY v.vehicle_name
HAVING COUNT(b.booking_id) > 2;
```

**ব্যাখ্যা: কোন গাড়ি ২ বার বা তার বেশি বুক হয়েছে সেগুলো বের করে। `GROUP BY` দিয়ে গ্রুপ করা হয় এবং `HAVING` দিয়ে গ্রুপের উপর শর্ত দেয়া হয়। এই query তে GROUP BY ব্যবহার করা হয়েছে vehicle অনুযায়ী booking গুলো group করার জন্য। COUNT দিয়ে প্রতিটা vehicle কতবার booking হয়েছে সেটা গণনা করা হয়। HAVING clause ব্যবহার করে group করার পরে condition দেওয়া হয়েছে—যাদের booking সংখ্যা ২ এর বেশি, শুধু তাদের দেখাবে। WHERE group-এর আগে কাজ করে, আর HAVING group-এর পরে কাজ করে।

---


## 📎 ERD & Viva

* ERD (visual): [https://drawsql.app/teams/sojibur-rahman-asif/diagrams/vehicle-rental-system](https://drawsql.app/teams/sojibur-rahman-asif/diagrams/vehicle-rental-system)
* Viva / demo video: [https://drive.google.com/file/d/10RDqveVlP_t4IFb_eGmV-6PZ3jThyDOT/view?usp=sharing](https://drive.google.com/file/d/10RDqveVlP_t4IFb_eGmV-6PZ3jThyDOT/view?usp=sharing)

---





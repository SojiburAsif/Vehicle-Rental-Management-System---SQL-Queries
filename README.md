# Vehicle Rental System – SQL Project Documentation

## 📌 Project Overview
This project demonstrates a **Vehicle Rental System** database using SQL.  
It focuses on creating tables, inserting sample data, and writing queries to retrieve meaningful information from the database.  
The goal is to showcase key SQL concepts such as **JOIN, EXISTS, WHERE, GROUP BY, and HAVING**.

---

## 🗂 Database and Tables

### 
```sql
1. Database Creation
CREATE DATABASE Vehicle_Rental_System;

CREATE TABLE users (
    user_id SERIAL PRIMARY KEY,
    name VARCHAR(30) NOT NULL,
    email VARCHAR(30) UNIQUE NOT NULL,
    password VARCHAR(255) NOT NULL,
    phone VARCHAR(16),
    role VARCHAR(10) 
        CHECK (role IN ('Customer', 'Admin')) 
        DEFAULT 'Customer'
);

2. Vehicles Table

Stores details of vehicles for rent.

CREATE TABLE vehicles (
    vehicle_id SERIAL PRIMARY KEY,
    vehicle_name VARCHAR(30),
    type VARCHAR(10) CHECK (type IN ('car', 'bike', 'truck')),
    model VARCHAR(20),
    registration_number VARCHAR(30) UNIQUE,
    rental_price_per_day NUMERIC(10, 2) NOT NULL,
    status VARCHAR(15) CHECK (availability_status IN ('available', 'rented', 'maintenance')) DEFAULT 'available'
);

3. Bookings Table

Stores booking records and links users to vehicles.

CREATE TABLE bookings (
    booking_id SERIAL PRIMARY KEY,
    user_id INTEGER REFERENCES users(user_id),
    vehicle_id INTEGER REFERENCES vehicles(vehicle_id),
    start_date DATE NOT NULL,
    end_date DATE NOT NULL,
    booking_status VARCHAR(15) CHECK (booking_status IN ('pending', 'confirmed', 'completed', 'cancelled')) DEFAULT 'pending',
    total_cost NUMERIC(10, 2)
);



Query 1: INNER JOIN
SELECT
  booking_id,
  name AS customer_name,
  vehicle_name,
  start_date,
  end_date,
  booking_status AS status
FROM bookings b
INNER JOIN users u ON b.user_id = u.user_id
INNER JOIN vehicles v ON b.vehicle_id = v.vehicle_id;

এই query দিয়ে আমরা booking-এর পুরো details একসাথে দেখতে পাই।

এই query তে INNER JOIN ব্যবহার করা হয়েছে যাতে bookings table-এর সাথে users এবং vehicles table connect করা যায়।
এতে করে কোন user কোন vehicle বুক করেছে, start date, end date আর booking status—সব একসাথে দেখা যায়।
INNER JOIN ব্যবহার করার কারণে শুধু সেই booking গুলোই দেখাবে যেগুলোর user আর vehicle দুইটাই valid আছে।



Query 2: EXISTS / NOT EXISTS
SELECT *
FROM vehicles v
WHERE NOT EXISTS (
  SELECT 1
  FROM bookings b
  WHERE b.vehicle_id = v.vehicle_id

এই query বের করে যেসব vehicle এখনো কেউ বুক করে নাই।

এখানে NOT EXISTS ব্যবহার করা হয়েছে check করার জন্য কোনো vehicle bookings table-এ আছে কিনা।
যদি কোনো vehicle-এর জন্য booking না থাকে, তাহলে সেটা result-এ দেখাবে।
এটা সাধারণত available বা unused vehicle বের করার জন্য কাজে লাগে।

Query 3: WHERE clause
SELECT *
FROM vehicles 
WHERE type = 'car' AND status = 'available';


এই query শুধু available car গুলো দেখায়।

WHERE clause ব্যবহার করে condition অনুযায়ী data filter করা হয়।
এখানে আমরা বলছি--type হবে car এবং status হবে available।
মানে শুধু যেসব car এখন ভাড়ার জন্য available আছে সেগুলোই দেখাবে।

Query 4: GROUP BY & HAVING
SELECT 
  v.vehicle_name,
  COUNT(b.booking_id) AS total_bookings
FROM bookings b
JOIN vehicles v 
  ON b.vehicle_id = v.vehicle_id
GROUP BY v.vehicle_name
HAVING COUNT(b.booking_id) > 2;

এই query দেখায় যেসব vehicle ২ বারের বেশি বুক হয়েছে।

এই query তে GROUP BY ব্যবহার করা হয়েছে vehicle অনুযায়ী booking গুলো group করার জন্য।
COUNT দিয়ে প্রতিটা vehicle কতবার booking হয়েছে সেটা গণনা করা হয়।
HAVING clause ব্যবহার করে group করার পরে condition দেওয়া হয়েছে--যাদের booking সংখ্যা ২ এর বেশি, শুধু তাদের দেখাবে।
WHERE group-এর আগে কাজ করে, আর HAVING group-এর পরে কাজ করে।


ERD (Entity Relationship Diagram)

👉 ERD Link:
[https://drawsql.app/teams/sojibur-rahman-asif/diagrams/vehicle-rental-system](https://github.com/SojiburAsif/Vehicle-Rental-Management-System---SQL-Queries.git)

এখানে table গুলোর relationship visually দেখানো হয়েছে।

🎥 Viva Practice Video

👉 Viva Video Link:
https://drive.google.com/file/d/10RDqveVlP_t4IFb_eGmV-6PZ3jThyDOT/view?usp=sharing







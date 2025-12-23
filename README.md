# Vehicle Rental System – SQL Project Documentation

## 📌 Project Overview
This project demonstrates a **Vehicle Rental System** database using SQL.  
It focuses on creating tables, inserting sample data, and writing queries to retrieve meaningful information from the database.  
The goal is to showcase key SQL concepts such as **JOIN, EXISTS, WHERE, GROUP BY, and HAVING**.

---

## 🗂 Database and Tables

### 1. Database Creation
```sql
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






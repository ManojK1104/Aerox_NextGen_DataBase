# ✈️ Aerox Database Management

A comprehensive **Airline Database Management System** built in MySQL, modeling end-to-end airline operations — flights, passengers, bookings, crew, revenue, and customer reviews — paired with a **Power BI dashboard** for analytics and reporting.

---

## 📌 Problem Definition

Modern airlines operate in a highly complex environment, managing thousands of flights, millions of passengers, and dynamic pricing across multiple channels. Without a robust database system, airlines face:

1. Inefficient flight scheduling
2. Revenue loss from poor inventory management
3. Customer dissatisfaction from overbooking
4. Regulatory compliance risks
5. Inability to analyze performance metrics

## 🎯 Problem Statement

Design and implement a comprehensive Airline Database Management System that efficiently manages flight operations, passenger bookings, crew assignments, aircraft maintenance, and provides real-time analytics for informed decision-making — while ensuring regulatory compliance and optimal revenue management.

---

## 🗂️ Database Schema

The system is built around **9 core relational tables** in MySQL, covering the full lifecycle of an airline's operations:

| # | Table | Purpose |
|---|-------|---------|
| 1 | `aircraft` | Fleet details — model, manufacturer, capacity, fuel capacity, maintenance schedule, and operational status |
| 2 | `airport` | Master list of airports with city, country, and timezone information |
| 3 | `passenger` | Passenger profiles including passport, contact, and emergency/medical details |
| 4 | `flight` | Flight route definitions (origin, destination, base airport, aircraft, scope) |
| 5 | `trip_details` | Individual scheduled trips for a flight — gates, terminals, delays, and real-time status |
| 6 | `crew` | Crew roster with role, licensing, experience, and current trip assignment |
| 7 | `booking_info` | Passenger bookings — class, seat, fare, baggage, payment, and meal preferences |
| 8 | `review` | Post-flight passenger reviews and ratings for flights and crew |
| 9 | `revenue_tracking` | Per-booking financial breakdown — fare, tax, ancillary fees, cost, and net profit |

### Key Relationships

- `flight` references `aircraft` and `airport` (base/origin/destination)
- `trip_details` references `flight` and `airport` (per-trip origin/destination)
- `crew` is linked to its `current_trip_id` in `trip_details`
- `booking_info` links `passenger` to a `trip_details` record
- `review` and `revenue_tracking` both branch off `booking_info`

This design enforces referential integrity through foreign keys (with `ON DELETE RESTRICT` / `SET NULL` / `CASCADE` rules as appropriate) and uses `ENUM` types extensively to keep operational statuses (flight status, booking status, payment status, crew role, etc.) consistent and query-friendly.

The script also seeds the schema with realistic sample data — including 130+ global airports, 150+ aircraft, and 100+ flight routes — making it ready to query and explore immediately.

---

## 📁 Repository Structure

```
Aerox_Database_Management/
├── Aerox Database.sql      # Full schema (DDL) + seed data (DML) for all 9 tables
├── aerox dashboard.pbix    # Power BI dashboard for visual analytics
└── README.md                # Project documentation
```

---

## ✨ Features

- **Fleet management** — track aircraft status (active, maintenance, leased out, retired, scrapped) and servicing schedules
- **Flight & trip scheduling** — distinguish between a flight *route* and each *scheduled trip*, with real-time status tracking (Scheduled, Boarding, Delayed, Diverted, etc.)
- **Passenger & booking management** — full booking lifecycle from reservation to boarding pass issuance, including baggage, meals, and special assistance
- **Crew operations** — licensing, safety checks, and live trip assignments
- **Revenue analytics** — fare, tax, ancillary fee, and profit tracking per booking
- **Customer feedback** — multi-criteria ratings (cleanliness, service, food, entertainment) for flights and crew
- **Power BI dashboard** — visual reporting layer built directly on top of the database

---

## 📊 Dashboard Preview

The Power BI dashboard (`aerox dashboard.pbix`) connects directly to the database and provides two report pages:

### Over View
KPIs for total flights, passengers, average aircraft capacity, operating cost, and revenue, alongside passenger trends by year, revenue split by travel class, aircraft capacity by status, and a decomposition of passengers by travel class, flight scope, meal preference, and special assistance.

<img width="300" height="250" alt="image" src="https://github.com/user-attachments/assets/0717bc29-d5a3-4951-ba25-f74c24fb1a74" />

### Methods
A breakdown of bookings by payment method, operating cost vs. revenue by travel class, and average ratings (overall, food, cleanliness, entertainment, service) per travel class.

<img width="300" height="250" alt="image" src="https://github.com/user-attachments/assets/37108358-9fe1-47a6-9292-84492040860a" />


---

## 🛠️ Tech Stack

- **Database:** MySQL
- **Analytics / BI:** Microsoft Power BI

---

## 🚀 Getting Started

### Prerequisites
- MySQL Server (8.0+ recommended) and a client such as MySQL Workbench
- Microsoft Power BI Desktop (to open the dashboard)

### Setup

1. Clone the repository
   ```bash
   git clone https://github.com/deveshdubey18/Aerox_Database_Management.git
   cd Aerox_Database_Management
   ```

2. Create the database in MySQL
   ```sql
   CREATE DATABASE aeroxv5;
   USE aeroxv5;
   ```

3. Run the SQL script to create tables and load seed data
   ```bash
   mysql -u <username> -p aeroxv5 < "Aerox Database.sql"
   ```

4. Open `aerox dashboard.pbix` in Power BI Desktop and point its data source to your local `aeroxv5` database to explore the dashboard.

---

## 📊 Sample Queries

```sql
-- All upcoming flights with delays
SELECT f.flight_code, t.scheduled_departure, t.delay_minutes, t.trip_status
FROM trip_details t
JOIN flight f ON t.flight_id = f.flight_id
WHERE t.delay_minutes > 0;

-- Revenue summary by travel class
SELECT b.travel_class, SUM(r.total_revenue) AS total_revenue, SUM(r.net_profit) AS total_profit
FROM revenue_tracking r
JOIN booking_info b ON r.booking_id = b.booking_id
GROUP BY b.travel_class;

-- Average crew/flight ratings
SELECT review_type, AVG(rating) AS avg_rating
FROM review
GROUP BY review_type;
```

---

## 🔮 Future Enhancements

- Stored procedures and triggers for automated status updates
- A reporting/admin web interface on top of the database
- Loyalty program and frequent-flyer tracking
- Real-time data refresh into the Power BI dashboard

---

## 👤 Author

**Manoj Kushwaha**
[GitHub](https://github.com/ManojK1104)

---

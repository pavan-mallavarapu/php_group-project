# StudentRide – Backend & SQL Showcase

*StudentRide* is a PHP/MySQL carpooling application designed for students and drivers to post, search, and book rides. This repository highlights my backend expertise—especially in SQL schema design, query optimization, and secure PHP–PDO integration.

---

# Live Demo Website Link:
web link: https://rides.infinityfreeapp.com/

## Key Features

- *User Management*  
  - Registration/Login with hashed passwords (bcrypt)  
  - Roles: drivers vs. riders  

- *Ride Posting & Search*  
  - Drivers create rides with origin, destination, travel_date, travel_time, total_seats, and price_per_seat.  
  - Riders search for open rides (available_seats > 0) using optimized indexing on (origin, destination, travel_date).  

- *Booking Workflow*  
  - Atomic checks for available_seats before inserting a booking.  
  - Parameterized INSERT into bookings, followed by UPDATE on rides.available_seats.  
  - Status transitions: pending → confirmed → completed/cancelled.  

- *Data Integrity & Relations*  
  - Foreign keys enforce cascade deletes between:
  - users(user_id) → rides(driver_id)  
  - rides(ride_id) → bookings(ride_id)  
  - users(user_id) → bookings(rider_id)  
  - users(user_id) → user_visits(user_id)  

- *Analytics & Logging*  
  - user_visits table logs each authenticated page access for analytics.  
  - Dashboard queries aggregate totals: user count, ride count, confirmed bookings, and visits in the last 7 days.  

---

## Technologies & Skills Demonstrated

- *Database Design (MySQL/MariaDB)*  
  - Efficient normalization of users, rides, bookings, and user_visits.  
  - ENUM types and proper data typing (e.g., DECIMAL(8,2) for price_per_seat).  

- *SQL Query Proficiency*  
  - Complex JOIN queries to fetch ride listings along with driver names:  
    sql
    SELECT r.ride_id, r.origin, r.destination, r.travel_date, r.travel_time,
           r.available_seats, r.price_per_seat, u.first_name, u.last_name
    FROM rides AS r
    JOIN users AS u ON r.driver_id = u.user_id
    WHERE r.origin = ? AND r.destination = ? AND r.travel_date = ?
      AND r.available_seats > 0 AND r.status = 'open'
    ORDER BY r.travel_time;
    
  - Transaction-like seat reservations via SELECT → INSERT → UPDATE.  
  - Aggregate queries for dashboard metrics:
    sql
    SELECT COUNT(*) AS total_users FROM users;
    SELECT COUNT(*) AS total_rides FROM rides;
    SELECT COUNT(*) AS total_bookings FROM bookings WHERE status = 'confirmed';
    SELECT COUNT(*) AS visits_last_week
      FROM user_visits
      WHERE visit_time >= DATE_SUB(NOW(), INTERVAL 7 DAY);
    

- *Secure PHP–PDO Integration*  
  - Prepared statements to prevent SQL injection ($pdo->prepare(...)->execute([...])).  
  - Password hashing (password_hash) and verification (password_verify).  
  - Session management for user authentication and role-based access.

---

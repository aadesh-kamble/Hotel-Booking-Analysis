# SQL Queries Executed

## Booking Performance

Q.1. What is the total number of bookings?
```sql
SELECT
    count(*)
FROM
    hotel_booking;
```

Q.2. How many bookings does each hotel receive?

```sql
SELECT 
    hotel, COUNT(hotel) AS ttl_count
FROM
    hotel_booking
GROUP BY hotel;
```

Q.3. Which season receives the highest number of bookings?

```sql
SELECT 
    season, COUNT(*)
FROM
    hotel_booking
GROUP BY season
ORDER BY COUNT(*) DESC
LIMIT 1;
```

Q.4. Which months generate the highest number of bookings?

```sql
SELECT 
    MIN(arrival_date) AS startdate,
    MAX(arrival_date) AS lastdate
FROM
    hotel_booking;

SELECT 
    MONTHNAME(arrival_date), COUNT(*)
FROM
    hotel_booking
GROUP BY MONTHNAME(arrival_date)
ORDER BY COUNT(*) DESC
LIMIT 1;
```

Q.5. What does the booking distribution by stay duration look like?
```sql
SELECT 
    stay_duration_type, COUNT(*)
FROM
    hotel_booking
GROUP BY stay_duration_type
ORDER BY COUNT(*)
```
---

## Revenue Analysis

Q.1. What is the total revenue generated?
```sql
SELECT 
    SUM(total_revenue) AS ttl_revenue
FROM
    hotel_booking;
```

Q.2. Which hotel generates more revenue?
```sql
SELECT 
    hotel, SUM(total_revenue) AS ttl_revenue
FROM
    hotel_booking
GROUP BY hotel
ORDER BY ttl_revenue DESC
LIMIT 1;
```

Q.3. Which customer types spend the most?
```sql
SELECT 
    customer_type, SUM(total_revenue) AS revenue
FROM
    hotel_booking
GROUP BY customer_type
ORDER BY revenue DESC
LIMIT 1;
```

Q.4. Which room types generate the highest revenue?
```sql
SELECT 
    assigned_room_type, SUM(total_revenue) AS sum_of_rev
FROM
    hotel_booking
GROUP BY assigned_room_type
ORDER BY sum_of_rev DESC
LIMIT 1;
```

Q.5. Which countries contribute the highest revenue? 
```sql
SELECT 
    country, SUM(total_revenue) AS sum_of_rev
FROM
    hotel_booking
GROUP BY country
ORDER BY sum_of_rev DESC
LIMIT 1;
```

Q.6. Which booking channels generate the highest revenue?
```sql
SELECT 
    distribution_channel, SUM(total_revenue) AS sum_of_rev
FROM
    hotel_booking
GROUP BY distribution_channel
ORDER BY sum_of_rev DESC
LIMIT 1;
```

Q.7. Which market segments are the most profitable? 
```sql
SELECT 
    market_segment, SUM(total_revenue) AS sum_of_rev
FROM
    hotel_booking
GROUP BY market_segment
ORDER BY sum_of_rev DESC
LIMIT 1;
```

Q.8. Average revenue per booking by customer type
``` sql
SELECT 
    customer_type, ROUND(AVG(total_revenue), 2) AS avg_revenue
FROM
    hotel_booking
GROUP BY customer_type
ORDER BY avg_revenue DESC;
```



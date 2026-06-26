# SQL Queries Executed

## Booking Performance

Q.1. What is the total number of bookings?
```sql
SELECT
    count(*) as ttl_bookings
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
    season, COUNT(*) as ttl_bookings
FROM
    hotel_booking
GROUP BY season
ORDER BY COUNT(*) DESC
LIMIT 1;
```

Q.4. Which months generate the highest number of bookings?

```sql
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

## Cancellation Analysis

Q.1. Overall cancellation rate 
```sql
SELECT 
    ROUND(SUM(CASE
                WHEN is_canceled = 1 THEN 1
            END) * 100.0 / COUNT(*),
            2) AS cancellation_rate_percentage
FROM
    hotel_booking;
```

Q.2. Cancellation rate by hotel
```sql
SELECT 
    hotel,
    ROUND(SUM(CASE
                WHEN is_canceled = 1 THEN 1
            END) * 100.0 / COUNT(*),
            2) AS cancellation_rate_percentage
FROM
    hotel_booking
GROUP BY hotel;
```

Q.3. Cancellation rate by booking channel
```sql
SELECT 
    distribution_channel,
    ROUND(SUM(CASE
                WHEN is_canceled = 1 THEN 1
            END) * 100.0 / COUNT(*),
            2) AS cancellation_rate_percentage
FROM
    hotel_booking
GROUP BY distribution_channel;
```

Q.4. Cancellation rate by country
```sql
SELECT 
    country,
    ROUND(SUM(CASE
                WHEN is_canceled = 1 THEN 1
            END) * 100.0 / COUNT(*),
            2) AS cancellation_rate_percentage
FROM
    hotel_booking
GROUP BY country
ORDER BY cancellation_rate_percentage DESC;
```

Q.5. Cancellation rate by market segment
```sql
SELECT 
    market_segment,
    ROUND(SUM(CASE
                WHEN is_canceled = 1 THEN 1
            END) * 100.0 / COUNT(*),
            2) AS cancellation_rate_percentage
FROM
    hotel_booking
GROUP BY market_segment
ORDER BY cancellation_rate_percentage DESC;
```

Q.6. Cancellation rate by customer type

```sql
SELECT 
    customer_type,
    ROUND(SUM(CASE
                WHEN is_canceled = 1 THEN 1
            END) * 100.0 / COUNT(*),
            2) AS cancellation_rate_percentage
FROM
    hotel_booking
GROUP BY customer_type
ORDER BY cancellation_rate_percentage DESC;
```

## Customer Behaviour

Q.1. Average stay length 
```sql
SELECT 
    ROUND(AVG(total_stay), 2) AS avg_stay_nights
FROM
    hotel_booking;
```

Q.2. Most preferred meal plan
```sql
SELECT 
    meal, COUNT(meal) AS pref_meal
FROM
    hotel_booking
GROUP BY meal
ORDER BY pref_meal DESC
LIMIT 1;
```

Q.3. Family vs Couple vs Solo travellers 
```sql
SELECT
    CASE
        WHEN total_guests = 1 THEN 'Solo'
        WHEN adults = 2 AND children = 0 AND babies = 0 THEN 'Couple'
        WHEN children > 0 OR babies > 0 THEN 'Family'
        ELSE 'Other'
    END AS traveller_type,
    COUNT(*) AS total_bookings
FROM hotel_booking
GROUP BY traveller_type
ORDER BY total_bookings DESC;
```

Q.4. Domestic vs International guests 
```sql
SELECT 
    CASE
        WHEN country = 'PRT' THEN 'Domestic guest'
        ELSE 'International guests'
    END AS guest_type,
    COUNT(*)
FROM
    hotel_booking
GROUP BY guest_type; 
```

Q.5. Which countries stay the longest?
```sql
SELECT 
    country, AVG(total_stay) AS avg_stay
FROM
    hotel_booking
WHERE
    country IS NOT NULL
GROUP BY country
ORDER BY avg_stay DESC
LIMIT 1;
```

Q.6. Which customer type stays the longest? 
```sql
SELECT 
    customer_type, AVG(total_stay) AS avg_stay
FROM
    hotel_booking
GROUP BY customer_type
ORDER BY avg_stay DESC
LIMIT 1;
```

Q.7. Repeat Guest vs New Guest distribution
```sql
SELECT 
    CASE
        WHEN is_repeated_guest = 1 THEN 'Repeat Guest'
        ELSE 'New guest'
    END AS Guest_type,
    COUNT(*) AS total_bookings
FROM
    hotel_booking
GROUP BY guest_type;
```

Q.8. Average group size
```sql
SELECT 
    ROUND(AVG(total_guests), 2) AS avg_group_size
FROM
    hotel_booking;
```

## Operational Insights

Q.1. Frequency of room upgrades (Reserved vs Assigned Room)
```sql
SELECT
    CASE
        WHEN reserved_room_type = assigned_room_type THEN 'No Change'
        ELSE 'Room Changed'
    END AS room_status,
    COUNT(*) AS total_bookings,
    ROUND(COUNT(*) * 100.0 / (SELECT COUNT(*) FROM hotel_booking), 2) AS percentage
FROM hotel_booking
GROUP BY room_status;
```
Q.2. What percentage of bookings require parking?
```sql
SELECT
    ROUND(
        COUNT(CASE WHEN required_car_parking_spaces > 0 THEN 1 END) * 100.0
        / COUNT(*),
        2
    ) AS parking_demand_percentage
FROM hotel_booking;
```
Q.3. How many guests made no requests?
```sql
SELECT 
    COUNT(*)
FROM
    hotel_booking
WHERE
    total_of_special_requests = 0;
```
Q.4. What is the average booking changes?
```sql
SELECT 
    ROUND(AVG(booking_changes), 2) AS avg_booking_changes
FROM
    hotel_booking;
```
Q.5. What is the avg days in waiting?
```sql
SELECT 
    ROUND(AVG(days_in_waiting_list), 2) AS avg_waiting_days
FROM
    hotel_booking;
```

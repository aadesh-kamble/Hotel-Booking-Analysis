Q.1. How many bookings does each hotel receive?

```sql
select hotel, count(hotel)
from hotel_booking
group by hotel;
```

Q.2. Which hotel performs better?


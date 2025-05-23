# 🧩 PART A: BUSINESS CASE QUESTIONS

### ❓ 1. Which cities bring in the highest revenue?

### 🔍 Query:
```SQL
SELECT Th.city,
       Sum(S.price_per_ticket * T.total_seats) AS total_revenue
FROM   tickets T
       JOIN shows S
         ON T.show_id = S.show_id
       JOIN theatres Th
         ON S.theatre_id = Th.theatre_id
GROUP  BY Th.city
ORDER  BY total_revenue DESC;
-- With this insight a plan can be created to target low revenue cities.
```

### 📤 Output:
![Q1](<../screenshots/Advanced Analysis SSs/Q1.png>)

### ❓ 2. Which customer segment uses the most discounts?

### 🔍 Query:
```SQL
-- ⚠️ Since there's no segmentation of customers wether customer-wise or service-wise,
--    I'm gonna group the output by customer_name and contact_number.

-- But this Gives an opportunity to the leaders to create segments like, 'Gold-seats' and 'Silver seats' etc
-- to boost overall revenue across cities.

SELECT CP.customer_name,
       CP.contact_number,
       Sum(A.discount_issued) AS total_discount_used
FROM   accounts A
       JOIN customer_profiles CP
         ON A.customer_id = CP.customer_id
GROUP  BY CP.customer_name,
          CP.contact_number
ORDER  BY total_discount_used DESC
LIMIT  5;
```

### 📤 Output:
![Q2](<../screenshots/Advanced Analysis SSs/Q2.png>)

### ❓ 3. Which show timings generate the highest ticket sales?

### 🔍 Query:
```SQL
SELECT S.show_timings,
       Sum(T.total_seats) AS total_tickets_sold
FROM   tickets T
       JOIN shows S
         ON T.show_id = S.show_id
GROUP  BY S.show_timings
ORDER  BY total_tickets_sold DESC;
```

### 📤 Output:
![Q3](<../screenshots/Advanced Analysis SSs/Q3.png>)


# 📊 PART B: KPI DASHBOARD METRICS

### 🎟️ Total Tickets Sold

### 🔍 Query:
```SQL
SELECT Sum(total_seats) AS total_tickets_sold
FROM   tickets;
```

### 📤 Output:
![Total Tickets Sold](<../screenshots/Advanced Analysis SSs/1 Total Tickets Sold.png>)

### 💰 Total Revenue

### 🔍 Query:
```SQL
SELECT Sum(S.price_per_ticket * T.total_seats) AS total_revenue
FROM   tickets T
       JOIN shows S
         ON T.show_id = S.show_id;
```

### 📤 Output:
![Total Revenue](<../screenshots/Advanced Analysis SSs/2 Total Revenue.png>)

### 👥 Unique Customers

### 🔍 Query:
```SQL
SELECT Count(DISTINCT customer_name) AS unique_customers
FROM   customer_profiles;
```

### 📤 Output:
![Unique Customers](<../screenshots/Advanced Analysis SSs/3 Unique Customers.png>)

### 🏆 Top Performing Movies (by Revenue)

### 🔍 Query:
```SQL
SELECT M.movie_name,
       Sum(S.price_per_ticket * T.total_seats) AS movie_revenue
FROM   tickets T
       JOIN shows S
         ON T.show_id = S.show_id
       JOIN movies M
         ON S.movie_id = M.movie_id
GROUP  BY M.movie_name
ORDER  BY movie_revenue DESC
LIMIT  5;
```

### 📤 Output:
![Top Performing Movies](<../screenshots/Advanced Analysis SSs/4 Top Performing Movies.png>)

### ⏰ Peak Booking Time Slots

### 🔍 Query:
```SQL
SELECT S.show_timings,
       Count(T.ticket_id) AS total_bookings
FROM   tickets T
       JOIN shows S
         ON T.show_id = S.show_id
GROUP  BY S.show_timings
ORDER  BY total_bookings DESC;
```

### 📤 Output:
![Peak Booking Time](<../screenshots/Advanced Analysis SSs/5 Peak Booking Time.png>)

### 🏷️ Total Discount

### 🔍 Query:
```SQL
SELECT Sum(discount_issued) AS total_discount
FROM   accounts;
-- Total discount is even higher than total revenue, I wonder how is that happening! Maybe customers are exploiting distant policies.
```

### 📤 Output:
![Total Discount](<../screenshots/Advanced Analysis SSs/6 Total Discount.png>)

### 📈 Total Profit

### 🔍 Query:
```SQL
SELECT Sum(S.price_per_ticket * T.total_seats) - (SELECT Sum(discount_issued)
                                                FROM accounts) AS total_profit
FROM   tickets T
       JOIN shows S
         ON T.show_id = S.show_id;
-- Profit value in (-) represents loss(which we can see is massive.)
-- The company should take serious actions right now!!
```

### 📤 Output:
![Total Profit](<../screenshots/Advanced Analysis SSs/7 total_profit.png>)
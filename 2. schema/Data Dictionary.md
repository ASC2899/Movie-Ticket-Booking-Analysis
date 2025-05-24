
## 🧾 `accounts` table

**customer_id** -------- Foreign key from customer_profiles to identify which customer this account belongs to.

**account_number** --- Unique identifier for each account, starts with 29399 (standardized during cleaning).

**discount_issued** ---- Total discount (currency) issued to this account.

**wallet_balance** ----- Current wallet balance for the customer.


## 👤 `customer_profiles` table

**customer_id** -------- Unique identifier for each customer (used across other tables).

**customer_name** ---- Name of the customer.

**email_id** ------------- Email address of the customer (standardized during cleaning).

**contact_number** ---- Contact number of the customer.


## 🎬 `movies` table

**movie_id** ------------- Unique identifier for each movie.

**movie_name** --------- Name of the movie.

**duration** -------------- Duration of the movie in minutes (standardized during cleaning).

**average_rating** ------- Average rating of the movie.

**language_available** -- Language in which the movie is available.


## 📺 `shows` table

**show_id** -------------- Unique identifier for each show.

**movie_id** ------------- Foreign key from movies table.

**theatre_id** ------------ Foreign key from theatres table.

**show_timings** -------- Time slot for the show (e.g., 7 PM - 9 PM).

**price_per_ticket** ------ Price for the show per ticket.


## 🎭 `theatres` table

**theatre_id** ------------ Unique identifier for each theatre.

**seating_capacity** ----- Total seating capacity of the theatre.

**city** -------------------- City where the theatre is located.


## 🎟️ `tickets` table

**ticket_id** -------------- Unique identifier for each ticket.

**customer_id** ---------- Foreign key from customer_profiles table.

**show_id** --------------- Foreign key from shows table.

**seat_numbers** --------- Seat number assigned for the ticket (standardized during cleaning).
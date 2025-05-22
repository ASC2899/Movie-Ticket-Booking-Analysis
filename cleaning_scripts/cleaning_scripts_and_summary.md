## Table names
I added 's' in the end of following table names to make all table names consistent.

```SQL
RENAME TABLE customer_profile TO customer_profiles;

RENAME TABLE movie TO movies;

RENAME TABLE theatre TO theatres;

RENAME TABLE ticket TO tickets;
```

## Table: `accounts`

### Issue Found:
- After complete inspection and eyeballing of accounts table, I found out that the account_number of customer_id 'UI0811' does not follow the common pattern of starting with '29399'.

### Fix:  
```SQL
UPDATE accounts
SET    account_number = '293992147483647'
WHERE  customer_id = 'UI0811';
```

## Table: `customer_profile`

### Issues Found:
- Only 198 distinct customer names in 1000 records, suggesting high duplication.

- Duplicates have different customer_id and email_id but same customer_name and contact_number.

- Email domains are inconsistent or missing:
  - `.com`  = 865
  - `.co`   = 87
  - `.c`    = 35 (this one is definitely a typo)
  - `gmil.` = 8
  - `gmil`  = 5

- All email IDs have @gmil instead of @gmail.

### Fixes
```SQL
-- Since .c is a typo and .com preferred over .co world wide, I'm going to change every record domain to .com.

-- Need more space, email_id is set to VARCHAR(20).
ALTER TABLE customer_profiles
  modify email_id VARCHAR(30);

UPDATE customer_profiles
SET    email_id = Replace(email_id, '.co', '.com')
WHERE  email_id LIKE '%.co';

UPDATE customer_profiles
SET    email_id = Replace(email_id, '.c', '.com')
WHERE  email_id LIKE '%.c';

UPDATE customer_profiles
SET    email_id = Concat(email_id, '.com')
WHERE  email_id LIKE '%gmil';

UPDATE customer_profiles
SET    email_id = Concat(email_id, 'com')
WHERE  email_id LIKE '%gmil.'; 

-- Fixing @gmil to @gmail.
UPDATE customer_profiles
SET    email_id = REPLACE(email_id, '@gmil', '@gmail')
WHERE  email_id LIKE '%@gmil%';



```

## Table: `movies`

### Issues Found:
- Duration column has string format like '118 Minutes' but it should be 118 in integer data type.

### Fixes:
```SQL
UPDATE movies
SET    duration = REPLACE(duration, RIGHT(duration, 8), '');

ALTER TABLE movies
  modify duration INT;

-- I also changed the name of column to duration_in_minutes.
ALTER TABLE movies
  CHANGE COLUMN duration duration_in_minutes INT;
```

## Table: `shows`

### No Issues Found:
- No nulls or duplicates.
- show_timings use string ranges like '7 PM - 9 PM', but values are consistently structured. No action needed.

## Table: `theatres`

### No Issues Found:
- No nulls or duplicates. Every data point is consistent. No action needed.

## Table: `tickets`

### Issues Found:

- seat_numbers column contains list-like strings (e.g. "[11, 12]") this can make our analytic queries unnecessarily complex.
- Needs a new column total_seats derived from seat_numbers.
- Some values in customer_id column are 'OFFLINE' but other values are customer ids(I think due to some Tables joining this column might have messed up).
- I will create a payment_method column using customer_id.

### Fixes:
```SQL
-- Creating total_seats column.
ALTER TABLE tickets
  ADD COLUMN total_seats INT;

UPDATE tickets
SET    total_seats = Length(seat_numbers) - Length(
                     Replace(seat_numbers, ',', '')) + 1;

-- Correcting customer_id before creating payment_method column.
-- Using customer 'UI0811'.
UPDATE tickets
SET    customer_id = 'UI0811'
WHERE  customer_id = 'OFFLINE';

--  Creating payment_method column.
ALTER TABLE tickets
  ADD COLUMN payment_method VARCHAR(10);

UPDATE tickets
SET    payment_method = 'OFFLINE'
WHERE  customer_id = 'UI0811';

UPDATE tickets
SET    payment_method = 'ONLINE'
WHERE  customer_id <> 'UI0811';
```



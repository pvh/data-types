+++
title = "Basic Data Modeling"
weight = 20
+++

# Basic data types
## A very brief review

---

# Basic types

## surrogate keys
## text
## numbers
## dates and times

---
# Keys

 * Include a surrogate key.
 * Model your natural key via constraints.
 * Use `bigserial` or `uuid` for your primary key.*

---

# Keys: BIGSERIAL

If your table is small, the extra size doesn't matter.

If your table is big, you're going to need it anyway.

---

# Keys: UUID

````sql
CREATE EXTENSION "uuid-ossp";
SELECT uuid_generate_v4();
````

````sql
CREATE EXTENSION pgcrypto;
SELECT gen_random_uuid();
````

(Or create the UUIDs in the client.)

---
# Text

 * Use `text`!
 * Avoid `varchar`, `char`, and anything else.

---
# Text: Use indexes for pattern-matching

An index supports prefixes (`LIKE 'Peter%'`)
````sql
CREATE INDEX ON users (name);
SELECT * FROM accounts WHERE email LIKE 'Peter%';
````

--

A functional index can support suffix lookups.
````sql
CREATE INDEX backsearch ON users (reverse(email));
SELECT * FROM accounts WHERE reverse(email) LIKE reverse('%doe.com');`
````

---

# Text: Other Searching

A tsvectors with a GIN index will find individual words efficiently
````sql
CREATE INDEX ON products USING gin(tsv);
````

Regular expressions: ~
````sql
SELECT * FROM users WHERE name ~ '(John|Jane)\s+Doe';
```` 

(You can get some index capability via pg_trgm/GIN index)


---
# Times & Dates

## `timestamptz`
### Always use `timestamptz`. (Not `time`.)

--
## `date`
### (For when you just need the date.)

## `time`
### (For when you only want a time-of-day.)

---
# Time functions: date_trunc()

A query that always starts at the beginning of the current week
````sql
SELECT date_trunc('week', now())::date;
````

--
A query that counts users created by day
````sql
SELECT date_trunc('day', created_at), count(*)
FROM users GROUP BY date_trunc('day', created_at)
````

---
# Time travel: interval

When was a year ago?
````sql
SELECT now() - '1 year'::interval;
````
--
All the days last month (good for joining):
````sql
SELECT generate_series(date_trunc('month', now() - '1 month'::interval),
                       date_trunc('month', now()), '1 day'::interval)
````

---
# Boolean values

## Use `bool`, not `bit`

--

Tip: Don't index on booleans. (Consider a partial index.)

---
# Binary data

## `bytea` 

(Do consider if Postgres is the right solution...)

Mostly you will leave these alone and treat them as opaque.

Handy Exception: 
````sql
SELECT md5(binary_data) FROM table;
````

---
# Don't use

 * `money`: not up to modern standards
 * `timestamp`: use `timestamptz`
 * `time`: you probably meant `timestamptz`
 * `serial`: use `bigserial`

# Are you sure?

 * `float` / `integer`: use `numeric`
 * `varchar`, `char`: use `text`, it's faster
 * `bitstring`: premature optimization
 * `xml`: libxml2 is awful, but...
 * `json`: you probably want jsonb 



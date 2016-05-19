+++
title = "Basic Data Modeling"
weight = 20
+++

# Data Modeling Basics
## Covering the fundamentals
## and frequent fudge-ups

---

# A simple `users` row

````
 id
 name
 email
 created_at
 deleted_at
 is_confirmed
````

---

# A simple `users` row

````
 id bigserial,
 name text,
 email text,
 created_at timestamptz,
 deleted_at timestamptz,
 is_confirmed bool
````

---
# Primary Keys

 * Include a primary key.
 * I don't trust natural keys to last.
--

 * Use `bigserial` or `uuid` for your primary key.*

---

# Primary keys: BIGSERIAL

If your table is small, the extra size won't really matter.
If your table is big, you're going to need it anyway.

---

# Primary keys: UUID

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
# Text: What?

 * Use `text`
 * Avoid varchar, char, and anything else.

---
# Text: Indexes for pattern-matching

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

# Other ways to search for text

 * regular expressions: ~ (see also, pg_trgm GIN index)
 * tsvectors and GIN will let you look up individual words

---
# Times & Dates

## `timestamptz`
### Always use `timestamptz`. (Not `time`.)

--
## `date`
### (Unless you actually want a `date`.)

---
# Time travel: date_trunc()

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

## use `bool`, not `bit`

They work as you'd expect.

--

Top tip: Don't index on booleans. (Consider a partial index.)

---
# Binary data

## `bytea` 

--
(Do consider if Postgres is the right solution...)

--

Mostly you will leave these alone and treat them as opaque.


Handy Exception: SELECT md5(binary_data) FROM table;

---
# Types to avoid

 * `money`: generally deprecated
 * `timestamp`: use `timestamptz`
 * `time`: you probably mean `timestamptz`
 * `serial`: use `bigserial`
 * `float` / `integer`: use `numeric`
 * `varchar`, `char`: use `text`, it's faster
 * `bitstring`: premature optimization
 * `xml`: libxml2 is awful, but...


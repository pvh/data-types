+++
title = "Basic Data Modeling"
weight = 2
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
--

 * Use `bigserial` or `uuid` for your primary key.*

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
# Time travel: ranges

Users with active accounts on Christmas Day
````sql
SELECT * FROM users
WHERE tstzrange(created_at, deleted_at) @> '2015-12-25'::timestamptz
````
--
NULLs count as -/+ infinity time.

--

You can also use a single tstzrange column type, but this is less common.

--

Top Tip: a GiST index works great here.

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
but consider if Postgres is the right solution

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


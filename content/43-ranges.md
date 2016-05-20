+++
title = "Range Types"
weight = 43
+++

# Ranges

---

# Ranges: What?

Ranges are a simple but powerful variation on composite types.
````sql
INSERT INTO reservation VALUES 
  ('[2016-01-01 11:30, 2016-01-01 15:00)');
````

Always has upper / lower, inclusive / exclusive bounds.

---

# Ranges: Why?

## Nothing lasts forever

---

# Ranges: Why?

 * operators
 * exclusion constraints
 * performance

---

# Ranges: Why?

Useful prebuilt ranges

 * numrange
 * tstzrange
 * daterange

---

# Ranges: How? (Basics)

````sql
SELECT tstzrange(created_at, deleted_at) FROM users;
````

--
NULLs count as -/+ infinity time.

--

Users with active accounts on Christmas Day
````sql
SELECT * FROM users
WHERE tstzrange(created_at, deleted_at) @> '2015-12-25'::timestamptz
````

--

Indexing makes this very fast.
````sql
CREATE INDEX ON users USING GIST( tstzrange(created_at, deleted_at) );
````

---
# Ranges: How? (Window functions)

Understand how events unfold...
````sql
WITH events_duration AS (
  SELECT tstzrange(
    at_time,
    lead(at_time, 1) OVER (partition by event_source order by at_time)) as range,
    *
  FROM events
)
SELECT * FROM events WHERE range @> now() - '4 hours';
````

---

# Ranges: How? (Constraints)

Ranges are also great for exclusion constraints.

Prevent double-booking!
````sql
ALTER TABLE reservation ADD CONSTRAINT
  EXCLUDE USING gist(room with =, during with &&)
````

# Ranges: How?


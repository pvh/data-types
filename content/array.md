+++
title = "Arrays"
weight = 41
+++

# Arrays

---
# Array: What?

It's an array.

--
In a value.

--
It's not really rocket science.

--

Useful for:
 * actual array data
 * tags / collections
 * on-the-fly aggregation in query results

---

# Array: How? (Constructing)

Make an array.
````sql
SELECT ARRAY[1, 2, 3];
````

Make it another way.
````sql
SELECT '{1, 2, 3}'::numeric[];
````

Extend your array.
````sql
SELECT ARRAY[1,2] || 3;
SELECT ARRAY[1,2] || ARRAY[3, 4];
````

Aggregate from records.
````sql
SELECT array_agg(distinct users) from events where name = 'PGDay.RU';
````

--
You can make N-dimensional arrays too.

---

# Array: How? (Accessing)

(Remember SQL arrays are 1-indexed!)
````sql
SELECT ('{one, two, three}'::text[])[1]; -- one
````

Use containment operator to find things.
````sql
CREATE INDEX ON table USING gin(tags); -- because fast!
SELECT * FROM table WHERE tags @> array['sometag'];
````
(GIN indexes are great here)

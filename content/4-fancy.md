+++
title = "Better data modeling"
+++

# Better data modeling
## Introducing some of the more advanced types

---

# Array

## Store an array inline in a value

 * actual array data
 * tags / collections
 * GIN indexes work great here

---

# Array cheatsheet

Make an array.
````sql
SELECT ARRAY[1, 2, 3];
````

Make it another way.
````sql
SELECT '{1, 2, 3}'::numeric[];
````

Remember SQL arrays are 1-indexed!
````sql
SELECT ('{one, two, three}'::text[])[1]; -- one
````

Use containment operator to find things.
````sql
CREATE INDEX ON table USING gin(tags); -- because fast!
SELECT * FROM table WHERE tags @> array['sometag'];
````

---

# JSON

## Use `jsonb`, not `json`.
## Probably add a GIN index.
## JSON(b) has so many functions

---

# Basic json(b)

Input is easy
````sql
select '{"a": "b"}'::jsonb
````

Get to elements with -> and ->>
````sql
select attrs->>'presenter_irc_nick' from talks
pvh
````

Using a single -> returns the internal JSON, using ->> casts to text.
````sql
select attrs->'previous_presentation'->>'location' from talks
Ottawa
````

(Compare with -> as the final operator.)
````sql
select attrs->'previous_presentation'->'location' from talks
"Ottawa"
````

# Searching `jsonb`


+++
title = "DIY Datatypes"
weight = 6
+++

# Do-it-yourself data-types

---

# Domains: What?

Create your own types that constrain existing types.
--
 * roll your own email type
 * a more specific URI
--
NB: SQL wants all types to be null-friendly. Don't restrict NULL away.

---
# Domains: When?

## Any time you want consistent logical constraints on a type.

---
# Domains: Email Address

````sql
CREATE DOMAIN email AS TEXT
CHECK(
  VALUE ~ '.+\@.+'
);
--
NB: A better email address would check other things like length and ASCII.

---

# Composite Types

Create your own types that combine existing types.
--
 * all tables are also types
 * members always go together
 * all members always have the same fields

---

# Custom functions and operators

A type without 
````sql
CREATE OR REPLACE FUNCTION host(email) RETURNS text 
LANGUAGE plpgsql AS $$ 
BEGIN 
  return substring($1 from '@(.+)$'); 
END 
$$;
````
--
# PL/PGSQL is slow!

# PL/V8
````sql
CREATE OR REPLACE FUNCTION host(email email) RETURNS text 
LANGUAGE plv8 AS $$ 
  return email.split("@")[1]
````

--
On my machine, 
--
PL/PGSQL took 10,000ms for 1m rows.
--
PL/V8 took 2,000ms for 1m rows.
--
pgemailaddr took 170ms.

---
# Moral of the story?
--
## If performance counts, use a C extension!
--
## If it counts a little, prefer PL/V8.
--
## If you like PL/PGSQL... go ahead?


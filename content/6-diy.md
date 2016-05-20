+++
title = "DIY Datatypes"
weight = 60
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

 * common, stable definitions
 * zip code
 * email addresses
 * NOT postal address

---
# Domains: Examples

````sql
CREATE DOMAIN email AS TEXT
CHECK(
  VALUE ~ '.+\@.+'
);
````

````sql
CREATE DOMAIN web_url AS uri CHECK ( (uri_scheme(value) = 'http' OR uri_scheme(value) = 'https')  AND uri_host(value) IS NOT null);
````

--
(A better email address would check other things like length.)

---

# Composite Types: What?

Create your own types that combine existing types.
--

 * all tables are also types
 * members always go together
 * all members always have the same fields

---

# Composite Types: How? (1/2)

Every row is also a composite type.
````sql
SELECT (users) FROM users;
````

But you can create your own:
````sql
CREATE TYPE point3d AS (x float, y float, z float);
CREATE TABLE threespace (location point3d);
````

---

# Composite Types: How? (2/2)

Referring to a row type (composite type named for table) requires parens
````sql
SELECT (users).email FROM users;
````

Row types are handy in window expressions
````sql
SELECT (agent_statuses) AS current, 
  lead((agent_statuses), 1) OVER (partition by agent_uuid order by time) AS next 
FROM agent_statuses;
````

---

# DIY functions

Defining a new function is straightforward
````sql
CREATE OR REPLACE FUNCTION host(email) RETURNS text 
LANGUAGE sql AS $$ 
  select split_part($1, '@', 2) as return; 
$$;
````

--

PL/PGSQL is very slow.
PL/V8 is relatively fast.
PL/SQL is quite fast.

---
# DIY Operators

An operator is any number of non-ASCII characters.
````sql
CREATE OPERATOR @-> ( 
  LEFTARG = 'email', 
  PROCEDURE = 'host');

SELECT email@-> FROM emails LIMIT 1;
````

+++
title = "Enums"
weight = 43
+++

# Enum

---

# Enums: What?

````sql
CREATE TYPE weekdays AS ('Mon', 'Tue', 'Wed', 'Thu', 'Fri');
````

Enums are conceptually similar to domains.

Fast, transparent mapping of words to integers.

(Data lives in pg_enum.)

---

# Enums: Why?

Enums provide a limited set of labels.

Storage is quite efficient - enums are stored as numbers.

---

# Enums: How?

Enums work like enums in other languages.
````sql
CREATE TYPE server_states AS 
  ENUM ('running', 'uncertain', 'offline', 'restarting');
````

--
You can treat them like their text values.
````sql
INSERT INTO servers(state) VALUES ('offline');
````

--

Enums block unexpected values from insertion.
````sql
INSERT INTO servers(state) VALUES ('mystery');
ERROR:  invalid input value for enum server_states: "mystery"
LINE 1: insert into servers(state) values ('mystery');
````

--
But you can add new values.
````sql
ALTER TYPE server_states ADD VALUE 'abandoned' AFTER 'offline';
````
--
(NB: You can treat enums as numbers and compare with > and < but... don't.)


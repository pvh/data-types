+++
title = "Enums"
weight = 43
+++

# Enums: What?

Enums are conceptually similar to domains.
Fast, transparent mapping of words to integers.
(Data lives in pg_enum.)

# Enums: Why?

Some datasets have huge numbers of similar labels.
Storing them as enums can reduce storage and improve performance.
It also restricts what labels can be written.

# Enums: How?

Enums work like enums in other languages.
````sql
CREATE TYPE server_states AS ENUM ('running', 'uncertain', 'offline', 'restarting');
````

--
You can treat them like their text values.
````sql
INSERT INTO servers(state) VALUES ('mystery');
````
NB: Most systems limit enum length to 63b.

--
They'll block illegal values.
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
NB: You can treat enums as numbers and compare with > and < but... don't.


+++
title = "JSON"
weight = 42
+++
# JSON

## Not exactly overlooked!

---

# JSON(b): What?

## "We made a database inside a type." 
.right[— Oleg Bartunov]

---

# JSON(b): When? (1/3)

Convenient short-lived or occasional attribute storage.
````sql
 {"weird_user?": true, "abuse_tags": ["new", "possible_abuser"]}
````
(Honour demands that I insist you make these columns if they get used often.)

---

# JSON(b): When? (2/3)

Hierarchical data.
````json
{"name": "car",
 "id": 91371
 "parts": [
   {
     "name": "engine"
     "id": 3241,
     "parts": [
       {
         "name": "crankshaft",
         "id": 23991,
         ...
````

---

# JSON(b): When? (3/3)

The data is already JSON. 

Why pull it apart?

---

# JSON(b): `jsonb` vs `json`

Prefer `jsonb`, not `json`.
````sql
ALTER TABLE users ADD COLUMN attrs jsonb;
````

Usually add a GIN index.
````sql
CREATE INDEX ON users USING gin(attrs);
````

---

# JSON(b): Anti-patterns

Probably don't do this...
````sql
CREATE TABLE bad_table(id serial, json json);
````

---

# JSON(b): Anti-patterns

Particularly if..
````sql
=# select distinct jsonb_object_keys(json) from bad_table;
 jsonb_object_keys
-------------------
 id
 created_at
 updated_at
 name
 email
 owner_id
 ...
````
Performance will suffer and your queries will be more complex.

---

# JSON(b): Construction

Input is easy.
````sql
select '{"a": "b"}'::jsonb
````

Adding members too.
```sql
UPDATE documents 
  SET contents = content || '{"new_field": "goes here"}' 
  WHERE document_id = 32421;
```

---

# JSON(b): Accessing

Get to elements with -> and ->>
````sql
select attrs->>'presenter_irc_nick' from talks
 attrs
-------
 pvh
````

Using a single -> returns the internal JSON.
````sql
select attrs->'previous_presentation'->>'location' from talks
   attrs
-----------
 "Ottawa"
````

Or send a whole path
````sql
SELECT jsonb_extract_path_text(attrs, 
  'previous_presentation', 'location')
````

---

# JSON(b): Decomposing

Turning JSON into records (requires column definition).
````sql
select * from json_to_recordset(
  '[{"a":1,"b":"foo"},{"a":"2","c":"bar"}]') as x(a int, b text);
````

---

# JSON(b): Searching

Find documents with a certain key,
````sql
select * from talks where attrs ? 'previous_presentation'
````

or documents with a whole sub-document in common.
````sql
SELECT * FROM talks WHERE attrs @> '{"previous_presentation": "PGCon"}'
````


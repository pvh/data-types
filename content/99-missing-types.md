+++
title = "Missing Types"
weight = 95
+++

# Missing Types
## Opportunities to make cool things

---

# Password

A password type could implement best practices.

---

# Currency

Why? Money is not a good type.
Add support for multiple currencies.

````sql

SELECT justify('$34.03 USD, $12.01 CAD', 
 'USD',
 '{"CAD": 0.77}');

````

---

# Physical Units

## '77 km/h'::physical
## '542 fluid ounces'::physical

--
Why?

--

So that Postgres users don't crash rovers into Mars. 

--

Also, because different countries use different units and why not have your database take care of that?
---

# Images

````sql
UPDATE graphs 
  SET image = render_graph(SELECT * FROM data WHERE run = 117) ;
````

````sql
SELECT gif_frame(image, 3) FROM gifs WHERE name = 'space-kaboom';
````

---

# Music

````sql
SELECT genre FROM mp3s;
````

Tags? Sound analysis? Conversion? Other things?

---

# 3d Meshes

Kinematics, morphing?


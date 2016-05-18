+++
title = "Missing Types"
weight = 95
+++

# Missing Types
## Opportunities to make cool things

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

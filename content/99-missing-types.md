+++
title = "Missing Types"
weight = 95
+++

# Missing Types
## Opportunities to make cool things

---

# Currency

`money` is not a good type, and we can do better.

````sql

SELECT justify('$34.03 USD, $12.01 CAD', 
 'USD',
 '{"CAD": 0.77}');

````

---
# Images

````sql
UPDATE graphs 
  SET image = render_graph(SELECT * FROM data WHERE run = 117) ;
````

````sql
SELECT gif_frame(image, 3) FROM gifs WHERE name = 'space-kaboom';
````

Image classification, sorting, organizing, resampling, and so on in-DB could be useful.

---

# Music / Sound / Tags

````sql
SELECT genre FROM mp3s;
SELECT max(sound_length(sound)) FROM samples;
````

Could be great for people with large music collections, DJs, researchers.

Consider building on taglib?


+++
title = "Special-purpose types"
weight = 50
+++

# Special-purpose Types
---

# Geometric Types

 * point
 * line
 * lseg
 * box
 * path
 * polygon
 * circle

---

# Geometric: What?

Postgres has a deep selection of geometry types.

---

# Geometric: Functions and Operators

 * translation: `+`, `-`, `*`, `/`
 * properties: `@-@` (length), `#` (intersects)
 * comparison: `@>` (contains), `<->` (distance)
 * .. and SO many others (`&<|`)
---

# Geometry: How?

''''sql
SELECT diameter(circle) FROM circles
  WHERE center(circle) = '(2, 3)'
''''

---

# PostGIS

http://postgis.net

PostGIS is the best way to store and query GIS data.

---

# PostGIS: What?

 * `geom`
 * `geog`
 * index support
 * several GIS libraries
 
---

# PostGIS: How?

````sql
SELECT
  sum(ST_Length(r.the_geom))/1000 AS kilometers
FROM
  bc_roads r,
  bc_municipality m
WHERE  r.name = 'Douglas St' AND m.name = 'VICTORIA'
  AND ST_Contains(m.the_geom, r.the_geom) ;
````

---
# PostBIS
https://colab.mpi-bremen.de/wiki/display/pbis/PostBIS

---

# IP4R
https://github.com/RhodiumToad/ip4r

---
# Optimization: FixedDecimal
https://github.com/2ndQuadrant/fixeddecimal


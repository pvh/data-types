+++
title = "Special-purpose types"
weight = 50
+++

# Special-purpose Types
---

# Geometry: What?

Postgres has a wide selection of geometry types.

 * point
 * line
 * lseg
 * box
 * path
 * polygon
 * circle

---

# Geometric: Functions and Operators

 * translation: `+`, `-`, `*`, `/`
 * properties: `@-@` (length), `#` (intersects)
 * comparison: `@>` (contains), `<->` (distance)
 * .. and SO many others (`&<|`)
---

# Geometry: How?

````sql
SELECT diameter(circle) FROM circles
  WHERE center(circle) = '(2, 3)'
````

---

# Geometry: Why?

 * good for light-weight geometry
 * not as fast or as feature rich as PostGIS
 * built in!

---

# PostGIS

http://postgis.net

PostGIS is the best way to store and query spatial data.

---

# PostGIS: What?

 * `geom`
 * `geog`
 * index support
 * wraps various spatial libraries

---

# PostGIS: Why?

 * geom datatype
 * better performance
 * huge amounts of functionality (vector / raster)
 * not built-in
 
---

# PostGIS: How?

````sql
SELECT
  sum(ST_Length(r.the_geom))/1000 AS kilometers
FROM
  bc_roads r,
  bc_municipality m
WHERE  r.name = 'Roseberry St' AND m.name = 'VICTORIA'
  AND ST_Contains(m.the_geom, r.the_geom) ;
````

---
# Network addresses

## built-in types

 * inet (`192.168.0.100`)
 * cidr (`::ffff:1.2.3.0/120`)
 * macaddr (`08:00:2b:01:02:03`)

## ip4r

https://github.com/RhodiumToad/ip4r

Adds index support for ranges, lighter-weight single IP type.
(And lots more.)


---
# Physical Units

https://github.com/ChristophBerg/postgresql-unit

New and exciting!

````sql
# SELECT '800 m'::unit + '500 m' AS length;
 length
--------
 1.3 km

# SELECT '9.81 N'::unit / 'kg' AS gravity;
  gravity
------------
 9.81 m/s^2

````

---
# Special mention

## PostBIS
Bio-informatic types for DNA querying

https://colab.mpi-bremen.de/wiki/display/pbis/PostBIS

## pg_currency
Very stale (6 years) but a solid foundation!

https://github.com/samv/pg-currency

---
# Special mention

## FixedDecimal
For when you need a fixed-precision decimal (performance)

https://github.com/2ndQuadrant/fixeddecimal

## HyperLogLog
Not really a data type, but a data structure for estimating distinct values.

https://github.com/aggregateknowledge/postgresql-hll


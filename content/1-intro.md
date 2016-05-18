+++
date = "2016-05-11T11:19:19-07:00"
weight = 1
title = "An Overview"
+++

# Data Types in PostgreSQL
A guided tour

---

## Peter van Hardenberg
### Heroku Postgres
.center[We run a lot of databases for the internet.]

http://postgres-data-types.pvh.ca/

(If you want a reference or see a bug, let me know!)

@pvh / pvh@pvh.ca

---

# The Plan

 * a quick review of using basic types
 * how data types work in postgres
 * advanced data types
 * special-purpose data types
 * customizing types
 * community extensions
* types you should write!

---
# Caveat Auditor

## "The root of all evil is premature optimization."
.right[— Knuth]

--

## My priorities

 * Query writing convenience
 * Avoid operational problems
 * Performance
 * NOT abstract notions of normalization


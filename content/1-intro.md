+++
date = "2016-05-11T11:19:19-07:00"
weight = 1
title = "An Overview"
+++

# Data Types in PostgreSQL
A guided tour

---

class: center, middle

## Peter van Hardenberg
### Heroku Postgres

http://postgres-data-types.pvh.ca/

(If you want a reference or see a bug, let me know!)

#### @pvh / pvh@pvh.ca

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

 * Nicer queries
 * Fewer operational challenges
 * Performance
 * NOT abstract notions of normalization

---

# Goals

 * fix a bad habit or two
 * encourage you to think beyond pure relational model
 * provide some intuition around what exists
 * leave you with a simple reference



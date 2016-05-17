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

---

# The Plan

 * basic data modeling
 * how data types work in postgres
 * better data types
 * special-purpose data types
 * DIY types
 * community extensions
 * data types we could use

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


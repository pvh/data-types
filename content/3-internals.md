+++
title = "Data type internals"
weight = 30
+++

# Data Type Internals
## A simplified guide for users

---
# Internals: What's a type?*

 * a row in pg_type
 * a nice name for users to remember
 * `typename_{in, out}()`
 * `typename_{send, recv}()` optionally
 * functions (`host()`, `unnest()`, `average()`)
 * operators (`<`, `=`, `~`)
 * ... and some other things

---
# Internals: How does it work?

 * typename_in() gets user input, e.g. `'rain', '{1, 3, 5}'`
 * returns a block of memory (palloc() fixed size or varlena)
 * this is added as an attribute to a tuple
 * the tuple is packed into a page
 * the page is written to disk
 * C language functions operate on this format

---
# Internals

<img width=220 src="tuple.png"/>
<img width=500 src="page.png"/>

---

# Internals: TOAST
## (The Oversized-Attribute Storage Technique)

Large values are compressed and moved to a special storage location called a TOAST table automatically.

This assumes large values shouldn't be kept with their row anyway.

---

# Internals: TOAST is great

 * Keeps data rows small, saving I/O on search.
 * Shrinks large values, saving I/O on retrieval.
 * Can (maybe?) add cost if searching contents of all rows of large data...
--

 * Try not to do that.

---
# Internals: Make TOAST faster

## `tsvector` on `text`
## `GIN` on `tsvector` / `jsonb`


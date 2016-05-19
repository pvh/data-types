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
 * `typename_{send, recv}()`
 * functions (`host()`, `unnest()`, `average()`)
 * operators (`<`, `=`, `~`)

---
# Internals: How does it work?

!!! Diagram?

 * text ('3.14', '{1, 3, 5}') is passed to typename_in().
 * typename_in() initializes a data structure
 * Postgres stores that data in a page and later, on disk

---

# Internals: "Data structure"?

!!! CONSIDER PUTTING A DIAGRAM HERE INSTEAD

 * Data structures come in fixed size or variable.
 * Fixed size types: integer, UUID, point.
 * Variable sizes: text, json, numeric
 * Variable size a.k.a "varlena" is [4b length, data]

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
# Internals: Don't get burned by TOAST

## `tsvector` on `text`
## `GIN` on `tsvector` / `jsonb`

---

# Internals: Binary Protocol

Text drivers use the textual type_out representation.

Binary protocol drivers must reimplement every type.

(This sucks.)


+++
title = "Data type internals"
+++

# Data Type Internals
## A simplified guide for users

---

# What's a data type (minimally)?

## A name
## A storage format
## Two functions, `type_{in, out}`
## maybe `type_{recv, send}`
## Length / size (fixed or variable)

---

# I/O

 * type_in: text -> internal
 * type_out: internal -> text
 * type_send: internal -> binary
 * type_recv: binary -> internal

# Type Size

 * N: a fixed number of bytes
 * -1: varlena (4b of size, then the data itself)
 * -2: null-terminated C-string

---

# And?

## You probably want some operators and functions
## like '=', for example.

---

# A note on driver support

Text drivers use the textual type_out representation.
Binary protocol drivers must reimplement every type.


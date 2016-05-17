+++
title = "Extended types in data modeling"
weight = 40
+++

# Extended types in data modeling
## Introducing some of the more advanced types

---
# Email Addresses

Everyone has email addresses, here's how you deal with them today.

````sql
SELECT * FROM users WHERE email LIKE '%@bigcorp.com'
````
--
This is always a full table scan. Sad trombone.
--
Using our reverse b-tree trick.
````sql
SELECT * FROM users WHERE reverse(email) LIKE reverse('%@bigcorp.com')
````
--
This is... clever, but ugly and hard to dream up.

---
# `emailaddr`
## github.com/petere/pgemailaddr

````sql
SELECT user(email) FROM users WHERE host(email) = 'bigcorp.com'
````
Bonus: By default, email addresses are sorted domain/user, so this is fast with no tricks!

---
# What about URLs? (Or URIs, if you prefer.)

---
# `uri`
## github.com/petere/pguri
--

````sql
SELECT path(referrer) FROM signups WHERE host = 'interestingblog.com'
````

---

# Skip the migration!

````sql
SELECT 'http://google.com/'::uri
````

````sql
SELECT host('http://google.com/'::uri)
````



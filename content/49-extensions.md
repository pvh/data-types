+++
title = "Extension Types"
weight = 49
+++

# Extension Types

---

# Email Addresses

Everyone has email addresses, here's how you deal with them today.

````sql
SELECT * FROM users WHERE email LIKE '%@bigcorp.com'
````
--
This is always a full table scan. Sad trombone.

--

We can try our reverse b-tree trick.
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
class: middle, center
# What about URLs?

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


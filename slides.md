layout: true

.signature[@algogrit]

---

class: center, middle

# GCF and Persistence

Gaurav Agarwal

---
class: center, middle

Cloud functions are stateless

---
class: center, middle

In order to persist data, you need to store it somewhere.

---
class: center, middle

Let's look at the various persistence options...

---
class: center, middle

## Cloud SQL

---
class: center, middle

Listing the instances

```bash
gcloud sql instances list
```

---
class: center, middle

Connecting to the database

```bash
gcloud sql connect emp-demo --user=postgres --quiet
```

---
class: center, middle

### Using Public IP

---
class: center, middle

*Authorized networks*: Requires IP to be whitelisted

---
class: center, middle

or

---
class: center, middle

`INSTANCE_CONNECTION_NAME`

---

class: center, middle

Code
https://github.com/algogrit/presentation-gcf-and-persistence

Slides
https://gcf-and-persistence.slides.algogrit.com

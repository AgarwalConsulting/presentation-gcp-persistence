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

*Demo*: Connecting to CloudSQL from a Go function using `database/sql`

---
class: center, middle

## Spanner

---
class: center, middle

Spanner is best used for massive-scale opportunities.

---

- 1000s of writes per second, globally

- 10,000s - 100,000s of reads per second, globally.

---
class: center, middle

`gcloud spanner instances list`

---

Client Libraries:

- `com.google.cloud.google-cloud-spanner` [for Java](https://github.com/googleapis/java-spanner)

- `cloud.google.com/go/spanner` [for Go](https://pkg.go.dev/cloud.google.com/go/spanner)

- ...

.content-credits[https://cloud.google.com/spanner/docs/reference/libraries]

---
class: center, middle

*Demo*: Connecting to Spanner from a Java function

.content-credits[https://googleapis.dev/java/google-cloud-spanner/latest/]

---
class: center, middle

*Demo*: Connecting to Spanner from a Go function

.content-credits[https://pkg.go.dev/cloud.google.com/go/spanner]

---
class: center, middle

What about `database/sql` in Go?

---
class: center, middle

Experimental library: Use with caution!

```bash
go get github.com/rakyll/go-sql-driver-spanner
```

.content-credits[https://github.com/rakyll/go-sql-driver-spanner]

---
class: center, middle

## Cloud Spanner vs Cloud SQL

---

- Spanner is globally and horizontally scalable

- Use Spanner if you have a lot of data (> 30TiB)

- Use Spanner if you have more than 1 database

- Cloud SQL is cheaper

---
class: center, middle

## Big Table

---
class: center, middle

Cloud Bigtable is a sparsely populated table that can scale to billions of rows and thousands of columns.

---
class: center, middle

A single value in each row is indexed; this value is known as the row key.

---

- store *terabytes* or even *petabytes* of data

- high read and write throughput at *low latency*

- ideal data source for *MapReduce* operations

---
class: center, middle

`gcloud bigtable instances list`

---
class: center, middle

For table level operations:

Use `cbt`

---
class: center, middle

`gcloud components install cbt`

---

```bash
cat ~/.cbtrc
```

```toml
project = "project-id"
instance = test-instance
```

---

Create a test Table

```bash
cbt createtable test-table \
"families=stats_summary:maxversions=2,stats_detail:maxversions=2,cell_plan:maxversions=2"
```

---

Populate the test Table

```bash
cbt set test-table phone#4c410523#20190501 \
  stats_summary:connected_cell=1 \
  stats_summary:connected_wifi=1 \
  stats_summary:os_build=PQ2A.190405.003 \
  cell_plan:data_plan_01gb=true \
  cell_plan:data_plan_05gb=true

cbt set test-table phone#4c410523#20190502 \
  stats_summary:connected_cell=1 \
  stats_summary:connected_wifi=1 \
  stats_summary:os_build=PQ2A.190405.004 \
  cell_plan:data_plan_05gb=true

cbt set test-table phone#4c410523#20190505 \
  stats_summary:connected_cell=0 \
  stats_summary:connected_wifi=1 \
  stats_summary:os_build=PQ2A.190406.000

cbt set test-table phone#5c10102#20190501 \
  stats_summary:connected_cell=1 \
  stats_summary:connected_wifi=1 \
  stats_summary:os_build=PQ2A.190401.002 \
  cell_plan:data_plan_10gb=true

cbt set test-table tablet#5c10102#20190502 \
  stats_summary:connected_cell=1 \
  stats_summary:connected_wifi=0 \
  stats_summary:os_build=PQ2A.190406.000 \
  cell_plan:data_plan_10gb=true
```

---
class: center, middle

Read the data

```bash
cbt read test-table

cbt count test-table
```

---

Client libraries for Big Table:

- *Apache HBase* [library for Java](https://hbase.apache.org/)

- [`cloud.google.com/go/bigtable`](https://pkg.go.dev/cloud.google.com/go/bigtable) for Go

- ...

---
class: center, middle

*Demo*: Connecting to BigTable from a Go function

.content-credits[https://pkg.go.dev/cloud.google.com/go/bigtable]

---
class: center, middle

Code
https://github.com/AgarwalConsulting/presentation-gcf-and-persistence

Slides
https://gcf-and-persistence.slides.agarwalconsulting.io/

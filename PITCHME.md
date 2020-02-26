# Welcome!
@snapend
---

@title[Introduction]

## Introduction

+++

What is this presentation all about?

+++

Introduction to Docker

+++

How we integrate ReadyAPI with Docker for database tests


+++

Intro to InSpec database tests and how we used Docker to integrate with the
development pipeline

+++

How we collaborate using Pull Requests & Git

---


# Docker

+++

### What is Docker?

</br>

@ul[text-12 list-fade-fragments](true)

- Like a Virtual Machine, but **faster** to start and stop (seconds vs. minutes)
- Provides **consistent** and **reproducible** environments (runs the same on a laptop, or in CI)
- It's the **future** of application development (and testing)

@ulend

+++

### Why did we use Docker?

@ul[text-12 list-fade-fragments](true)
- Working with **PostgreSQL** databases for the last 2 quarters
- PostgreSQL databases hosted in **AWS**
- **Unable** to test database changes due to firewall rules
- Docker allows **creating** a PostgreSQL database
- **Not just** Postgres!
@ulend

+++

### How we make use of Docker

@ul[text-13 list-fade-fragments](false)
- `docker **run** postgres`
- Point ReadyAPI at **`localhost`**
- **Run** tests!
- `docker **rm** postgres`
- **Repeat**
@ulend

---

## ReadyAPI

+++

# Demo!

---

## InSpec

+++

### What is InSpec?

</br>

@ul[list-spaced-bullets text-13 list-fade-fragments](true)

- Testing framework for **infrastructure**
- **Ruby**, built on top of RSpec
- Has a **PostgreSQL** Resource built-in

@ulend

+++

@snap[north-east span-100]
Expectations as YAML
@snapend

```yaml zoom-15
environments:
  dev:
    db_name: pcd_dev_db
    db_owner: postgres
    username: pcd_dev_user
  qa:
    db_name: pcd_qa_db
    db_owner: postgres
    username: pcd_qa_user
  uat:
    db_name: pcd_uat_db
    db_owner: postgres
    username: pcd_uat_user
```

+++

```yaml zoom-12 code-blend
db_schema:
  name: parcel_subscription
  owner: postgres
  usage_grant:
    - pcd_app_role
    - pcd_func_owner

db_tables:
  - name: pcd_subscription
    owner: pcd_app_role
    columns:
      - name: receiver_first_name
        type: CHARACTER VARYING
      - name: receiver_last_name
        type: CHARACTER VARYING
      - name: email_address
        type: CHARACTER VARYING
```

+++

```yaml zoom-14 code-blend
db_functions:
  - name: insert_customer_details
    owner: pcd_func_owner
    security_definer: true
    input_arguments: >-
      receiver_first_name      CHARACTER VARYING,
      receiver_last_name       CHARACTER VARYING,
      email_address            CHARACTER VARYING,
      collection_location_id   CHARACTER VARYING,
      collection_point_name    CHARACTER VARYING,
      parcel_tracking_number   CHARACTER VARYING
```

+++

@snap[north-east span-100]
Parameterized SQL
@snapend

```sql zoom-17 code-blend
SELECT tableowner
FROM pg_tables
WHERE tablename = '<%= table %>';
```

+++

@snap[north-east span-100]
One command to bring up database
@snapend

```bash zoom-7
$ ./flyway.sh
Waiting for postgres server, 4 remaining attempts...
CREATE ROLE
CREATE ROLE
CREATE DATABASE
REVOKE
Flyway Community Edition 6.2.2 by Redgate
Database: jdbc:postgresql://pcd_database/pcd_dev_db (PostgreSQL 10.11)
Creating schema "parcel_subscription" ...
Creating Schema History table "parcel_subscription"."flyway_schema_history" ...
Current version of schema "parcel_subscription": null
Migrating schema "parcel_subscription" to version 1 - create db user
Migrating schema "parcel_subscription" to version 2 - update sproc owner permissions
Migrating schema "parcel_subscription" to version 3 - create tables
Migrating schema "parcel_subscription" with repeatable migration create stored procs
Successfully applied 4 migrations to schema "parcel_subscription" (execution time 00:00.457s)
```

+++

@snap[north-east span-100]
One command to run tests
@snapend

``` zoom-7
$ docker-compose up
...
...
  ✔  users-exist: User roles exist
     ✔  User 'pcd_dev_user' exists
     ✔  User 'pcd_qa_user' exists
     ✔  User 'pcd_uat_user' exists
  ✔  schema-usage-granted: Defined roles have USAGE on schema
     ✔  Schema usage for user 'pcd_app_role' is granted
     ✔  Schema usage for user 'pcd_func_owner' is granted


Profile Summary: 25 successful controls, 0 control failures
Test Summary: 183 successful, 0 failures, 0 skipped
```

+++

# Demo!

---

## GitHub – Collaboration and Pull request work flow

- How we work together to ensure quality
- Using pull requests to run and review tests
- GitHub repo PRs demo

---

## Q & A

---

## Thank you for attending :D

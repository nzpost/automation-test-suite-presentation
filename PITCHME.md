# Welcome!
@snapend
---

@title[Agenda]
## Agenda
+++

Icebreaker and Introduction

+++

 ReadyAPI automated test suite

+++

InSpec for database validation

+++

GitHub – Pull request work flow

+++

Overview of Test Report Management page in Confluence

+++

Q & A session (please save any questions until then)

+++

Feedback

---

# Icebreaker!

---
@title[Introduction]

## Introduction

+++

What is this presentation all about?

+++

As test automation engineers, we are often asked what and how we are
automating

+++

How can we make our automation efforts more visible

+++

Integrating automated database tests with the development pipeline

+++

How we utilize docker to test database scripts and changes

+++

How we collaborate using ReadyAPI and Git

---

## ReadyAPI

+++

### What is ReadyAPI?

</br>

@ul[list-spaced-bullets text-13 list-fade-fragments](true)

- **API testing** tool
- **Extensible** with Groovy, Java
- **Data-driven** testing

@ulend

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

## Overview of Test Report Management page in Confluence

---

## Q & A + Feedback

---

## Thank you for attending :D

---

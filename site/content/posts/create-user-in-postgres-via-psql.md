---
title: Create user in Postgres via psql
date: 2020-03-04T20:09:00.000Z
tags:
  - postgres
  - snippets
---
In case it is needed to manually add users in postgres using the psql console is pretty simple.

First, access the command line:
```
sudo -u postgres psql
```
Then create the database, user and assign permissions:
```
CREATE DATABASE mydatabase;
CREATE USER myuser WITH ENCRYPTED PASSWORD 'secretpassword';
GRANT ALL PRIVILEGES ON DATABASE mydatabase TO myuser;
```
You can verify the users with the command `\du`

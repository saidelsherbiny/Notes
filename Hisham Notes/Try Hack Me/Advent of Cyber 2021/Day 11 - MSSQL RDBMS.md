# Day 11 - Networking - MSSQL and RDBMS

## What is MS SQL?
- MS SQL Server is a Relational Database Management System (RDBMS). 
- One simple way to think of a relational database is a group of tables that have relations.

![](https://i.imgur.com/ox383V4.png)

## Connecting to MS SQL

- sqsh pronounced skwish
- `sqsh -S server -U username -P password`

## Usage
```shell-session
1> xp_cmdshell 'whoami';
2> go
```
```shell-session
1> xp_cmdshell 'type c:\windows\WindowsUpdate.log';
2> go
```
# Introduction to Structured Query Language (SQL)

## Introduction

Structured Query Language (SQL) is a langauge that allows up to interact with relational database management systems (RDBMS) to access and manipulate the database through SQL queries. 

For learning SQL, I recommend using SQLite and DB Browser for SQLite. Available for Windows, MacOS, and Linux.

* [DB Browser for SQLite](https://sqlitebrowser.org/dl/)

There are many permutations of SQL out in the wild. Oracle, MySQL and PostgreSQL to name a few. They all follow a similar pattern, queries and commands may vary slightly but core concepts of relational databases still apply.

## Querying a Database

### SELECT and FROM

To pull data from a database there are two core commands that you will need to use. `SELECT` and `FROM`. Below is a small example table.

Table Name: Customers

|ID|Name|Address|
|---|---|---|
|1 | Ross M| Elwick Road, Ashford, TN23 1DD|
|2| Herman Cuthbert| 27 Fiasco Way, Ashford, TN24 3DS|
3| Hubert Cumberdale| Kettles R Us, Cobbs Wood Ashford|
4| The Pianoman | Joel Close, Kennington, Ashford |

To access data we need to decide two things, which table we want to pull data from, and which columns we want. `FROM` allows us to choose which table we want data *from* and `SELECT` allows us to choose which *columns* we want.

To pull all customer names stored in our simple database we would use the following command:

```sql
SELECT
    Name
FROM
    Customers
```

To pull both names and addresses we would run:

```sql
SELECT
    Name,
    Address
FROM
    Customers
```

### ORDER BY

We can also choose how the data output is presented from our query using the `ORDER BY` command. We can also sort the output by ascending (0 to 10, A to Z etc) or descending.

```sql
SELECT
    Name,
    Address
FROM
    Customers
ORDER BY
    Name ASC
```

This query sorts the customer output by name ascending (A-Z). To sort by descending, replace `ASC` with `DESC`. 

### WHERE

We can also filter our selected data to remove any matches that are irrelevant to the data we are looking for. 

SQL can use similar operators to other programming languages. Arthmetic, Comparitors, and Logical. 

```sql
SELECT
    Name,
    Address
FROM
    Customers
WHERE
    Name = 'Ross'
ORDER BY
    Name ASC
```
The above will output the columns *Name* and *Address* but only show results in which the *Name* column contains the value *Ross*. 


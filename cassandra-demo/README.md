# Cassandra Demo

This demo assumes you have a MacBook Pro with Mac OS X 10.11. Some tweaking may be required to make it work on your platform.

## Installing and Starting Cassandra

Download the latest binary for Cassandra [here](http://cassandra.apache.org/download/). This demo assumes you've downloaded Apache Cassandra 3.7.

Extract the tarball using:

```bash
tar xvf apache-cassandra-3.7-bin.tar
```

Start the Cassandra daemon using

```bash
./apache-cassandra-3.7/bin/cassandra
```

This will run the cassandra daemon. You might need to update your JDK version to 8u33 or higher.

## Demo using `cqlsh`

Start up `cqlsh` by running:

```bash
./apache-cassandra-3.7/bin/cqlsh
```

This should connect you to a cassandra shell that supports SQL-like commands. For this demo, we will try a few simple operations on Cassandra:

First create a new keyspace -- which is corresponds to a namespace for all your tables.

```sql
CREATE KEYSPACE mykeyspace
WITH REPLICATION = { 'class' : 'SimpleStrategy', 'replication_factor' : 1 };
```

Now authenticate the keyspace:

```sql
USE mykeyspace;
```

Now create a new table called `users`:

```sql
CREATE TABLE users (
  user_id int PRIMARY KEY,
  fname text,
  lname text
);
```

Insert some data into `users` table...

```sql
INSERT INTO users (user_id,  fname, lname)
  VALUES (1745, 'john', 'smith');
INSERT INTO users (user_id,  fname, lname)
  VALUES (1744, 'john', 'doe');
INSERT INTO users (user_id,  fname, lname)
  VALUES (1746, 'john', 'smith');
```

View the data you've inserted using:

```sql
SELECT * FROM users;
```

Your output should look like:

```
user_id | fname | lname
---------+-------+-------
    1745 |  john | smith
    1744 |  john |   doe
    1746 |  john | smith
```

In order to perform queries on secondary attributes (say, `lname`), we need to create an index; try executing the query without the index first:

```sql
SELECT * FROM users WHERE lname = 'smith';
```

This should result in a failure message. Go ahead and create an index:

```sql
CREATE INDEX ON users (lname);
```

Now you can perform queries on secondary attributes:

```sql
SELECT * FROM users WHERE lname = 'smith';
```

This conlcudes our demo for Cassandra. You can try running more familiar operators from SQL such as aggregates (`COUNT`, `AVERAGE`, `SUM`, etc), and commands such as `TRUNCATE` or `DELETE` for cleaning up or removing tables.

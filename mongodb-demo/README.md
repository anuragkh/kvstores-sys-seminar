# MongoDB Demo

This demo assumes you have a MacBook Pro with Mac OS X 10.11. Some tweaking may be required to make it work on your platform.

## Installing and Starting MongoDB

You can install mongodb on Mac OS X either manually or using homebrew. I prefer the latter, since you can install it with a simple one-liner:

```bash
brew update && brew install mongodb
```

If you prefer installing it manually, check out the instructions [here](https://docs.mongodb.com/manual/tutorial/install-mongodb-on-os-x/).

To start MongoDB, you first have to create a data directory for MongoDB to use:

```bash
mkdir -p path/to/data/db
```

Ensure that you have appropriate permissions to read & write to the directory. Start the mongodb daemon using:

```bash
mongod --dbpath path/to/data/db
```

## Demo using `mongo`

Start the mongodb shell using on a new terminal:

```bash
mongo
```

The mongo shell supports a javascript-like language. We will run a few simple commands on the mongo shell for this demo.

First create and authenticate a new database:

```javascript
use mydb;
```

Now create a new collection `users` by adding some data into it:

```javascript
db.users.insert( { fname : 'john', lname : 'smith' } );
db.users.insert( { fname : 'john', lname : 'doe' } );
db.users.insert( { fname : 'john', lname : 'smith' } );
```

View the data you've inserted using:

```javascript
db.users.find();
```

Your output should look something like:

```
{ "_id" : ObjectId("577d791c29b1b7302360ca17"), "fname" : "john", "lname" : "smith" }
{ "_id" : ObjectId("577d791c29b1b7302360ca18"), "fname" : "john", "lname" : "doe" }
{ "_id" : ObjectId("577d791d29b1b7302360ca19"), "fname" : "john", "lname" : "smith" }
```

We note two things:
* The records have a JSON-like structure; MongoDB calls them BSON documents.
* MongoDB automatically assigns each record a unique ObjectID. This forms the primary attribute for each record.

Try running queries on secondary attributes:

```javascript
db.users.find( { lname : 'smith' } );
```

This should run sucessfully and produce an output that looks like the following:

```
{ "_id" : ObjectId("577d791c29b1b7302360ca17"), "fname" : "john", "lname" : "smith" }
{ "_id" : ObjectId("577d791d29b1b7302360ca19"), "fname" : "john", "lname" : "smith" }
```

You can create an index on a secondary attribute to speed up queries on them:

```javascript
db.users.createIndex( { lname : 1 } );
```

Rerunning the query on the secondary attribute should produce the same results, although it would be impractical to expect significant speedups with just 3 records.

This conlcudes our demo for MongoDB. You can check out more supported operations in the `mongo` shell [here](https://docs.mongodb.com/manual/reference/mongo-shell/).
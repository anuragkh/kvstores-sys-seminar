# Elasticsearch Demo

This demo assumes you have a MacBook Pro with Mac OS X 10.11. Some tweaking may be required to make it work on your platform.

## Installing and Starting Elasticsearch

Download the latest binary for Elasticsearch [here](https://www.elastic.co/downloads/elasticsearch). This demo assumes you've downloaded Elasticsearch 2.3.3.

Extract the tarball using:

```bash
tar xvf elasticsearch-2.3.3.tar
```

Start the Elasticsearch daemon using

```bash
./elasticsearch-2.3.3/bin/elasticsearch
```

## Demo via REST API

Elasticsearch supports queries through its REST API. For this demo, we will try a few simple operations on Elasticsearch:

Create a new index called `users` (index is equivalent to tables in typical data stores) by adding new entries:

```bash
curl -XPUT "http://localhost:9200/mynamespace/users/1745" -d '
{
    "fname": "john",
    "lname": "smith"
}'
curl -XPUT "http://localhost:9200/mynamespace/users/1744" -d '
{
    "fname": "john",
    "lname": "doe"
}'
curl -XPUT "http://localhost:9200/mynamespace/users/1746" -d '
{
    "fname": "john",
    "lname": "smith"
}'
```

View the data you've inserted using:

```bash
curl -XGET "http://localhost:9200/mynamespace/users/_search"
```

Your output should look like:

```
{"took":31,"timed\_out":false,"\_shards":{"total":5,"successful":5,"failed":0},"hits":{"total":3,"max\_score":1.0,"hits":[{"\_index":"mynamespace","\_type":"users","\_id":"1746","\_score":1.0,"\_source":
{
    "fname": "john",
    "lname": "smith"
}},{"\_index":"mynamespace","\_type":"users","\_id":"1745","\_score":1.0,"\_source":
{
    "fname": "john",
    "lname": "smith"
}},{"\_index":"mynamespace","\_type":"users","\_id":"1744","\_score":1.0,"\_source":
{
    "fname": "john",
    "lname": "doe"
}}]}}
```

Elasticsearch automatically indexes all secondary attributes; perform queries on secondary attributes as follows:

```sql
curl -XPOST "http://localhost:9200/mynamespace/users/_search" -d '
{
    "query": {
        "match": {
          "lname" : "smith"
        }
    }
}'
```

This should produce the following results:

```
{"took":17,"timed\_out":false,"\_shards":{"total":5,"successful":5,"failed":0},"hits":{"total":2,"max\_score":1.0,"hits":[{"\_index":"mynamespace","\_type":"users","\_id":"1745","\_score":1.0,"\_source":
{
    "fname": "john",
    "lname": "smith"
}},{"\_index":"mynamespace","\_type":"users","\_id":"1746","\_score":0.30685282,"_source":
{
    "fname": "john",
    "lname": "smith"
}}]}}
```

This conlcudes our demo for Elasticsearch.

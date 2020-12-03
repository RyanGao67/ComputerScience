# Examples

* elasticsearch java
[https://github.com/RyanGao67/ElasticSearch_Java](https://github.com/RyanGao67/ElasticSearch_Java)

* elasticsearch java
[https://github.com/RyanGao67/elasticsearch_java_1/blob/master/src/main/java/com/example.java](https://github.com/RyanGao67/elasticsearch_java_1/blob/master/src/main/java/com/example.java)

# Elasticsearch CMD

```
//Start a elasticsearch: 
sudo docker run -d --name es762 -p 9200:9200 -e "discovery.type=single-node" elasticsearch:7.6.2

//check elasticsearch information
curl localhost:9200
{
  "name" : "5f3477e4bef8",
  "cluster_name" : "docker-cluster",
  "cluster_uuid" : "fcz4zPNZSSGl3Ti2_Z-iqw",
  "version" : {
    "number" : "7.6.2",
    "build_flavor" : "default",
    "build_type" : "docker",
    "build_hash" : "ef48eb35cf30adf4db14086e8aabd07ef6fb113f",
    "build_date" : "2020-03-26T06:34:37.794943Z",
    "build_snapshot" : false,
    "lucene_version" : "8.4.0",
    "minimum_wire_compatibility_version" : "6.8.0",
    "minimum_index_compatibility_version" : "6.0.0-beta1"
  },
  "tagline" : "You Know, for Search"
}

// check cluster health
curl -X GET localhost:9200/_cluster/health?pretty
{
  "cluster_name" : "docker-cluster",
  "status" : "green",
  "timed_out" : false,
  "number_of_nodes" : 1,
  "number_of_data_nodes" : 1,
  "active_primary_shards" : 0,
  "active_shards" : 0,
  "relocating_shards" : 0,
  "initializing_shards" : 0,
  "unassigned_shards" : 0,
  "delayed_unassigned_shards" : 0,
  "number_of_pending_tasks" : 0,
  "number_of_in_flight_fetch" : 0,
  "task_max_waiting_in_queue_millis" : 0,
  "active_shards_percent_as_number" : 100.0
}

// check elasticsearch node, ?v will print the header
curl -X GET localhost:9200/_cat/nodes?v
ip         heap.percent ram.percent cpu load_1m load_5m load_15m node.role master name
172.17.0.2           16          90  50    3.15    3.02     2.02 dilm      *      5f3477e4bef8

// check the indices
curl -X GET localhost:9200/_cat/indices?v
health status index uuid pri rep docs.count docs.deleted store.size pri.store.size

// search full version
curl -XGET -u username:password "https://adsfadfadf:9300/.kibana/_search" \
-H 'Content-Type: application/json' \
-d '{"query":{"match_all":{}}}'
```

# Sharding
* A shard is an independent index(kind of)
* Each shard is an Apache Lucene index
* An elasticsearch index consists of one or more Lucene indices
* A shard has no predefined size; it grows as documents are added to it
* A shard may store up to about two billion documents


# The purpose of Sharding
* Mainly to be able to store more documents
* To easier fit large indices onto nodes
* Improved performance
  * Parallelization of queries increases the throughput of an index
  
```
// check the indices
curl -X GET localhost:9200/_cat/indices?v
health status index uuid pri rep docs.count docs.deleted store.size pri.store.size
```

The pri is primary shards, think of this column as the number of shards that a given index has. For example, each of our indices are stored on a single shard, being the default setting. 

Up until Elasticsearch version 7, the default was actually for an index to have five shards.However, having five shards for very small indices is very unnecessary.When people created lots of small indices within small clusters, they ran into an issue of over-sharding, meaning that they had way too many shards. At the same time, it was previously not possible to change the number of shards once an index had been created.

So what you had to do if you needed to increase the number of shards, was to create a new index with more shards, and move over the documents. This was not only inconvenient, but could also be a time consuming process. To overcome these challenges, indices are created with a single shard by default, since Elasticsearch version 7. For small to medium sized indices, this is likely to be sufficient. If you do need to increase the number of shards, there is a Split API for this. It still involves creating a new index, but the process is much easier, as Elasticsearch handles the heavy lifting for us. To go the other way, i.e. to reduce the number of shards, there is a Shrink API that does this for us.




However, as a rule of thumb, a decent place to start, would be to choose five shards if you anticipate millions of documents to be added to an index. Otherwise, you should be completely fine sticking to the default of one shard and then increase the number if need be.

To recap, sharding is a way to sub-divide an index into smaller pieces, each being a shard. This serves two main purposes; enabling the index to grow in size, and to improve the throughput of the index.

By using sharding, you can store an index taking up 700 gigabytes of disk space, even if you have no single node that can store that amount of data. An index defaults to having one shard.

# Replication
![](./img/elas1.png)

# Increase query throughput with replication
We don't really need another node, and we also don't want to increase our costs.Instead, we can increase the number of replica shards by one, or however many we need. Since we only have two nodes, we are not really increasing the availability of the index, but we are increasing the throughput of the index.

A replica shard is a fully functional index on its own, just like a shard is. This actually means that both of the replica shards can be queried at the same time. This is possible because of two things; the fact that Elasticsearch automatically coordinates where queries will be executed, and parallelization.Elasticsearch will automatically select the shard that will execute a given query, based on a number of factors


```
// see shards situation
cat /_cat/shards?v

example return: 
index shard prirep state    docs store ip        node
pages 0     p/r    STARTED  0    230b  127.0.0.1 Bos-MBP-2
```



# Roles

![](./img/elas2.png)

![](./img/elas3.png)

![](./img/elas4.png) 

![](./img/elas5.png)


```
// check elasticsearch node, ?v will print the header
curl -X GET localhost:9200/_cat/nodes?v
ip         heap.percent ram.percent cpu load_1m load_5m load_15m node.role master name
172.17.0.2           16          90  50    3.15    3.02     2.02 dilm      *      5f3477e4bef8

```

node.role here is dim, data inject master


![](./img/elas6.png)

```
ryangao67@ryangao67-ThinkPad-T460s:~/tgao2020/ComputerScience$ curl -X POST localhost:9200/products/_doc/100 -H 'Content-Type:application/json' -d '{"name":"Toaster", "price":49, "in_stock":true}'
{"_index":"products","_type":"_doc","_id":"100","_version":1,"result":"created","_shards":{"total":3,"successful":1,"failed":0},"_seq_no":1,"_primary_term":1}

ryangao67@ryangao67-ThinkPad-T460s:~/tgao2020/ComputerScience$ curl -X POST localhost:9200/products/_search
{"took":184,"timed_out":false,"_shards":{"total":2,"successful":2,"skipped":0,"failed":0},"hits":{"total":{"value":2,"relation":"eq"},"max_score":1.0,"hits":[{"_index":"products","_type":"_doc","_id":"T_OHJXYBjcvHWAUM583g","_score":1.0,"_source":{"name": "Coffee Maker"}},{"_index":"products","_type":"_doc","_id":"100","_score":1.0,"_source":{"name":"Toaster", "price":49, "in_stock":true}}]}}

ryangao67@ryangao67-ThinkPad-T460s:~/tgao2020/ComputerScience$ curl -X POST localhost:9200/products/_search?pretty
{
  "took" : 6,
  "timed_out" : false,
  "_shards" : {
    "total" : 2,
    "successful" : 2,
    "skipped" : 0,
    "failed" : 0
  },
  "hits" : {
    "total" : {
      "value" : 2,
      "relation" : "eq"
    },
    "max_score" : 1.0,
    "hits" : [
      {
        "_index" : "products",
        "_type" : "_doc",
        "_id" : "T_OHJXYBjcvHWAUM583g",
        "_score" : 1.0,
        "_source" : {
          "name" : "Coffee Maker"
        }
      },
      {
        "_index" : "products",
        "_type" : "_doc",
        "_id" : "100",
        "_score" : 1.0,
        "_source" : {
          "name" : "Toaster",
          "price" : 49,
          "in_stock" : true
        }
      }
    ]
  }
}
```

# Create index
```
curl -XPUT localhost:9200/products -H 'Content-Type: application/json' -d '{"settings":{"number_of_shards":2, "number_of_replicas":2}}' 


curl -XDELETE localhost:9200/products
{"acknowledged":true}

```

# Update
```
curl -X POST localhost:9200/products/_update/100 -H "Content-Type:application/json" -d '{"doc":{"tags":["electronics"]}}'
{"_index":"products","_type":"_doc","_id":"100","_version":2,"result":"updated","_shards":{"total":2,"successful":1,"failed":0},"_seq_no":1,"_primary_term":1}

curl -X POST localhost:9200/products/_update/100 -H "search?pretty
{
  "took" : 12,
  "timed_out" : false,
  "_shards" : {
    "total" : 1,
    "successful" : 1,
    "skipped" : 0,
    "failed" : 0
  },
  "hits" : {
    "total" : {
      "value" : 1,
      "relation" : "eq"
    },
    "max_score" : 1.0,
    "hits" : [
      {
        "_index" : "products",
        "_type" : "_doc",
        "_id" : "100",
        "_score" : 1.0,
        "_source" : {
          "name" : "Toaster",
          "price" : 49,
          "in_stock" : true,
          "tags" : [
            "electronics"
          ]
        }
      }
    ]
  }
}

```

The Update API: 
1. The current document is retrieved
2. The field values are changed
3. The existing document is replaced with the modified document
4. We could do the exact same thing at the application level

*Because document is immutable*

# Script
```
curl -X POST -H "Content-Type:application/json" localhost:9200/products/_update/100 \
-d '{"script":{"source":"ctx._source.price--"}}'
{"_index":"products","_type":"_doc","_id":"100","_version":3,"result":"updated","_shards":{"total":2,"successful":1,"failed":0},"_seq_no":2,"_primary_term":1}

curl -X POST -H "Content-Type:application/json" localhost:9200/products/_search?pretty
{
  "took" : 6,
  "timed_out" : false,
  "_shards" : {
    "total" : 1,
    "successful" : 1,
    "skipped" : 0,
    "failed" : 0
  },
  "hits" : {
    "total" : {
      "value" : 1,
      "relation" : "eq"
    },
    "max_score" : 1.0,
    "hits" : [
      {
        "_index" : "products",
        "_type" : "_doc",
        "_id" : "100",
        "_score" : 1.0,
        "_source" : {
          "name" : "Toaster",
          "price" : 48,
          "in_stock" : true,
          "tags" : [
            "electronics"
          ]
        }
      }
    ]
  }
}

curl -X POST -H "Content-Type:application/json" localhost:9200/products/_update/100 -d \
'{
  "script":
    {"source":"ctx._source.price-=params.quantity",
     "params":{"quantity":4}
 }
}'
{"_index":"products","_type":"_doc","_id":"100","_version":4,"result":"updated","_shards":{"total":2,"successful":1,"failed":0},"_seq_no":3,"_primary_term":1}

curl -X POST -H "Consearch?pretty
{
  "took" : 1,
  "timed_out" : false,
  "_shards" : {
    "total" : 1,
    "successful" : 1,
    "skipped" : 0,
    "failed" : 0
  },
  "hits" : {
    "total" : {
      "value" : 1,
      "relation" : "eq"
    },
    "max_score" : 1.0,
    "hits" : [
      {
        "_index" : "products",
        "_type" : "_doc",
        "_id" : "100",
        "_score" : 1.0,
        "_source" : {
          "name" : "Toaster",
          "price" : 44,
          "in_stock" : true,
          "tags" : [
            "electronics"
          ]
        }
      }
    ]
  }
}
```


Two other possible values for the "result"(see above update command)key that is specified within the query results, apart from "updated." The key may also contain the value "noop." In the previous lecture, I told you that this is the case if we try to update a field value with its existing value. The same is not the case with scripted updates; if we set a field value within a script, the result will always equal "updated," even if no field values actually changed. There are two exceptions to this, both being if we explicitly set the operation within the script.

For example, we can write a script that ignores a given document based on a condition. This is done by setting the "op" property on the "ctx" variable to "noop."

Note that you can write a similar script that just inverts the conditional statement and only reduces the field value if it evaluates to "true." The difference between these two scripts, is that the one to the right always yields a result of "updated," regardless of whether or not the field value was actually changed. This is not the case for the one on the left. So if it's important for you to detect if nothing was changed, then that's the way to go.

Actually, you can also set the operation to "delete," which will cause the document to be deleted instead. This will set the value of the "result" key to "deleted" within the results. This is rarely useful though, as you can also delete documents that match a given query, as you will see later in this section, so this should only be used for situations where you need to use scripting to determine if a document should be deleted. You can see an example of such a query on your screen. The query decreases the value of the "in_stock" field by one as long as it is less than or equal to one.

![](./img/elas7.png)



# Upsert
```
curl -XPOST localhost:9200/products/_update/101 -H "Content-Type:application/json" -d '{"script":{"source":"ctx._source.price++"},"upsert":{"name":"Blender", "price":399, "in_stock":true}}'
{"_index":"products","_type":"_doc","_id":"101","_version":1,"result":"created","_shards":{"total":2,"successful":1,"failed":0},"_seq_no":5,"_primary_term":1}
```

If id 101 exists, it will update using the script, otherwise it will update. 


# Replace

It is the same as index 

```
PUT /products/_doc/100
{
  "name":"Toaster",
  "price":79,
  "in_stock":4
}
```

# Delete
```
DELETE /products/_doc/101
```

# Routing
![](./img/elas8.png)

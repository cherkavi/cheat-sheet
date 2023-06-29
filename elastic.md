# Elasticsearch  
[rest api documentation](https://www.elastic.co/guide/en/elasticsearch/reference/current/rest-apis.html)  
[examples](https://dzone.com/articles/23-useful-elasticsearch-example-queries)  
[examples](https://www.tutorialspoint.com/elasticsearch)  

## installation
```sh
curl -fsSL https://artifacts.elastic.co/GPG-KEY-elasticsearch | sudo apt-key add -
echo "deb https://artifacts.elastic.co/packages/7.x/apt stable main" | sudo tee -a /etc/apt/sources.list.d/elastic-7.x.list
sudo apt update && sudo apt install elasticsearch
sudo systemctl start elasticsearch
curl -X GET 'http://localhost:9200'
```

## collaboration
```bash
# common part
ELASTIC_HOST=https://elasticsearch-label-search-prod.apps.vantage.org
INDEX_NAME=ubs-single-autolabel
```

### check connection
```sh
# version info
curl -X GET $ELASTIC_HOST

# health check
curl -H "Authorization: Bearer $TOKEN" -X GET $ELASTIC_HOST/_cluster/health?pretty=true
curl -X GET $ELASTIC_HOST/_cluster/health?pretty=true
curl -X GET $ELASTIC_HOST/_cluster/health?pretty=true&level=shards
curl -X GET $ELASTIC_HOST/$INDEX_NAME
```
### check user
```sh
curl -s --user "$USER_ELASTIC:$USER_ELASTIC_PASSWORD" -X GET $ELASTIC_HOST/_security/user/_privileges
curl -s --user "$USER_ELASTIC:$USER_ELASTIC_PASSWORD" -X GET $ELASTIC_HOST/_security/user
curl -s --user "$USER_ELASTIC:$USER_ELASTIC_PASSWORD" -X GET $ELASTIC_HOST/_security/user/$USER_ELASTIC
```

### obtain bearer token
```sh
curl -s --user "$USER_ELASTIC:$USER_ELASTIC_PASSWORD" -X GET $ELASTIC_HOST/token
```

### index
[create index](https://www.elastic.co/guide/en/elasticsearch/reference/6.8/indices-create-index.html)  
[mapping](https://www.elastic.co/guide/en/elasticsearch/reference/6.8/mapping.html)  
Info: if your index or id has space ( special symbol ) you should replace it with %20 ( http escape )

#### index info
```bash
# all indexes
curl -X GET $ELASTIC_HOST/_cat/indices | grep ubs | grep label
# count records by index
curl -X GET $ELASTIC_HOST/_cat/count/$INDEX_NAME
```

#### create index from file
```sh
curl -X POST $ELASTIC_HOST/$INDEX_NAME/_mapping \
-H 'Content-Type: application/json' \
-d @labels_mappings.json
```

#### create index inline
```sh
# get index
curl -s --user "$SEARCH_USER:$SEARCH_PASSWORD" -X GET $ELASTIC_HOST/$ELASTIC_INDEX > file_with_index.json

# for using just have read index, pls remove next lines:
# {"index_name": {"aliases": {}, "mappings": {  # and last } 
# settings.index.provided_name
# settings.index.creation_date
# settings.index.uuid
# settings.index.version

# create index
json_mappings=`cat file_with_index.json`
curl -X PUT $ELASTIC_HOST/$INDEX_NAME -H 'Content-Type: application/json' \
-d @- << EOF
{
	"mappings": $json_mappings,
	"settings" : {
        "index" : {
            "number_of_shards" : 1,
            "number_of_replicas" : 0
        }
    }
}
EOF
```
#### Index creation Dynamic type creation
```sh
curl --insecure -s --user "$ELK_USER:$ELK_PASSWORD" -X PUT $ELASTIC_HOST/$INDEX_NAME -H 'Content-Type: application/json' --data @- << EOF
{
    "settings": {
        "index": {
            "number_of_shards": "3",
                "auto_expand_replicas": "false",
                "number_of_replicas": "2"
            }
        }
}

curl --insecure -s --user ${ELK_USER}:${ELK_PASSWORD} -X PUT ${ELASTIC_HOST}/${ELASTIC_INDEX}/_mapping/${DYNAMIC_TYPE_NAME}?include_type_name=true -H 'Content-Type: application/json' -d @- << EOF
{
    "label": {
        "properties": {
            "controlDate": {
                "type": "date"
            },
            "roadType": {
                "type": "keyword"
            },
            "nameOfProject": {
                "type": "keyword"
            },
        }
    }
}
EOF
```

#### update index
```sh
curl -X PUT -s --user "$SEARCH_USER:$SEARCH_PASSWORD" $ELASTIC_HOST/$ELASTIC_INDEX/_mapping
{
	"_source": {
                              "excludes": [
                                            "id"
                              ]
               },
               "properties": {
                              "mytags": {
                                            "type": "flattened"
                              }
               }
}
```

#### delete index
```
curl -s --user "$SEARCH_USER:$SEARCH_PASSWORD" -X GET $ELASTIC_HOST/$ELASTIC_INDEX > file_with_index.json
```
or it is better without types specification:
```json
{
        "settings": {
            "index": {
                "number_of_shards": "5",
                "auto_expand_replicas": "false",
                "number_of_replicas": "2"
            }
        }
}
```


### [search request query request](https://www.elastic.co/guide/en/elasticsearch/reference/current/docs.html)
```bash
# query search in the whole instance 
curl -X GET -u $ELASTIC_USER:$ELASTIC_PASSWORD "$ELASTIC_HOST/_search?q=sessionId:$SESSION_ID&pretty"
# query search in index 
curl -X GET -u $ELASTIC_USER:$ELASTIC_PASSWORD "$ELASTIC_HOST/$ELASTIC_INDEX/_search?q=sessionId:0f0062a6-e45b-4c5b-b80c-099db65edd20&pretty"
# query select first records from index
curl -X POST -H "Content-Type: application/json" -u $ELASTIC_USER:$ELASTIC_PASSWORD "$ELASTIC_HOST/$ELASTIC_INDEX/_search?pretty" -d @- <<EOF
{
  "size": 1,
  "query": {
    "match_all": {}
  }
}
EOF
 
# get by id
curl -X GET -u $ELASTIC_USER:$ELASTIC_PASSWORD $ELASTIC_HOST/$ELASTIC_INDEX/_doc/$DOC_ID

PROPERTY_XPATH=ship.distance
# inline query to ELK 
curl -X GET -u $ELASTIC_USER:$ELASTIC_PASSWORD $ELASTIC_HOST/$ELASTIC_INDEX/_search?q=sessionId:${SESSION_ID}&size=10000&pretty
curl -X GET -u $ELASTIC_USER:$ELASTIC_PASSWORD $ELASTIC_HOST/$ELASTIC_INDEX/_search?q=$PROPERTY_XPATH>100&pretty=true
curl -X GET -u $ELASTIC_USER:$ELASTIC_PASSWORD $ELASTIC_HOST/$ELASTIC_INDEX/_search?q=$PROPERTY_XPATH:>100&pretty=true
curl -X GET -u $ELASTIC_USER:$ELASTIC_PASSWORD $ELASTIC_HOST/$ELASTIC_INDEX/_search?q=$PROPERTY_XPATH>100&pretty=true

# query from external file
echo '{"query": {"match" : {"sessionId" : "a8b8-0174df8a3b3d"}}}' > request.json
echo '{"query": { "range" : {"ship.distance": {"gte": 100}}}}' > request.json
curl -X POST -H "Content-Type: application/json" -u $ELASTIC_USER:$ELASTIC_PASSWORD -d @request.json "$ELASTIC_HOST/$ELASTIC_INDEX/_search"

# elastic complex query
curl -X POST -H "Content-Type: application/json" -u $ELASTIC_USER:$ELASTIC_PASSWORD "$ELASTIC_HOST/$INDEX_NAME/_search" \
-d @- << EOF
{ "size":"10000",
  "query": {"bool": { "must": [ {"match": { "property1":"5504806" } } ] } },
  "_source":{"includes":["property1","property2","property3"],"excludes":["property4","property5"]}, 
  "sort": [
        { "property1": { "order": "desc", "unmapped_type":"long" }},
        { "property2": { "order": "desc", "unmapped_type":"keyword" }},
        { "property3": { "order": "desc", "unmapped_type":"long" }}
    ]}
EOF
```

### insert record
```sh
curl -X POST -H 'Content-Type: application/json' --data @test-example.json  -u $ELASTIC_USER:$ELASTIC_PASSWORD $ELASTIC_HOST/$ELASTIC_INDEX/label
```

### update record
```sh
curl -X POST -H "Content-Type: application/json" -u $ELASTIC_USER:$ELASTIC_PASSWORD "$ELASTIC_HOST/$ELASTIC_INDEX/_update/${DOC_ID}" -d @- <<EOF
{
  "doc": {
    "property_1": [
      "true"
    ]
  }
}
EOF
```


### remove records delete records
```sh
curl -X PUT $ELASTIC_HOST/$INDEX_NAME/_delete_by_query' -H 'Content-Type: application/json' \
-d @- << EOF
{
    "query": {
        "term": {
            "sessionId.keyword": {
                "value": "8a140c23-420c-3bf0a285",
                "boost": 1.0
            }
        }
    }
}
EOF
```
remove all records from index
```sh
curl -X POST --insecure -s --user $USER:$PASSWORD $ELASTIC_HOST/$INDEX_NAME/_delete_by_query  -H 'Content-Type: application/json' -d '{
    "query": { "match_all": {} }
}'

curl -X POST --insecure -s --user $USER:$PASSWORD $ELASTIC_HOST/$INDEX_NAME/_delete_by_query -H 'Content-Type: application/json' -d @- << EOF
{
    "query": {
        "match": {
            "_id": "_all"
        }
    }
}
EOF
```

### backup snapshot save load index
* [create snapshot](https://www.elastic.co/guide/en/elasticsearch/reference/current/snapshots-take-snapshot.html)
* [restore snapshot](https://www.elastic.co/guide/en/elasticsearch/reference/current/snapshots-restore-snapshot.html)

## Exceptions
```
org.elasticsearch.hadoop.rest.EsHadoopRemoteException: illegal_argument_exception: Can't merge because of conflicts: [Cannot update excludes setting for [_source]]
```
check your index & type - something wrong with creation

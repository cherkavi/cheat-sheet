#Elasticsearch
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

```bash
# health check
curl -X GET $ELASTIC_HOST/_cluster/health?pretty=true
curl -X GET $ELASTIC_HOST/_cluster/health?pretty=true&level=shards
curl -X GET "$ELASTIC_HOST/$INDEX_NAME

# indexex info
curl -X GET $ELASTIC_HOST/_cat/indices | grep ubs | grep label
curl -X GET $ELASTIC_HOST/_cat/count/$INDEX_NAME
```

```bash
# request example
curl -X GET "$ELASTIC_HOST/$INDEX_NAME/_search?q=front_vehicle.distance:>100&size=11&pretty=true"
curl -X GET "$ELASTIC_HOST/$INDEX_NAME/_search?q=road_type:highway"
```

```bash
echo '{"query": {"match" : {"sessionId" : "a8b8-0174df8a3b3d"}}}' > request.json
echo '{"query": { "range" : {"front_vehicle.distance": {"gte": 100}}}}' > request.json

curl -X POST -H "Content-Type: application/json" -u $LABEL_SEARCH_USERNAME:$LABEL_SEARCH_PASSWORD -d @request.json "$ELASTIC_HOST/$ELASTIC_INDEX/_search"
```

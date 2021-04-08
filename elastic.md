[examples](https://dzone.com/articles/23-useful-elasticsearch-example-queries)  
[examples](https://www.tutorialspoint.com/elasticsearch)  

```bash
ELASTIC_HOST=https://elasticsearch-label-search-prod.apps.vantage.org

curl -X GET $ELASTIC_HOST/_cluster/health?pretty=true
curl -X GET $ELASTIC_HOST/_cat/indices | grep ubs | grep label
curl -X GET "$ELASTIC_HOST/ubs-single-autolabel/_search?q=front_vehicle.distance:>100&pretty=true"
curl -X GET "$ELASTIC_HOST/ubs-single-autolabel/_search?q=front_vehicle.distance>100&pretty=true"
```

```bash
echo '{"query": {"match" : {"sessionId" : "a8b8-0174df8a3b3d"}}}' > request.json
echo '{"query": { "range" : {"front_vehicle.distance": {"gte": 100}}}}' > request.json

curl -X POST -H "Content-Type: application/json" -u $LABEL_SEARCH_USERNAME:$LABEL_SEARCH_PASSWORD -d @request.json "$ELASTIC_HOST/$ELASTIC_INDEX/_search"
```

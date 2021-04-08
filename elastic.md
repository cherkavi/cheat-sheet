[examples](https://dzone.com/articles/23-useful-elasticsearch-example-queries)
[examples](https://www.tutorialspoint.com/elasticsearch)
```bash
ELASTIC_HOST=https://elasticsearch-label-search-prod.apps.vantage.org

curl -X GET $ELASTIC_HOST/_cluster/health?pretty=true
curl -X GET $ELASTIC_HOST/_cat/indices | grep ubs | grep label
curl -X GET "$ELASTIC_HOST/ubs-single-autolabel/_search?q=front_vehicle.distance:>100&pretty=true"
curl -X GET "$ELASTIC_HOST/ubs-single-autolabel/_search?q=front_vehicle.distance>100&pretty=true"
```

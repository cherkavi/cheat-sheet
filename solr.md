# REST API collaboration 

*TIP* investigation request/response of the Solr UI 

## read collections
```
curl -s localhost:8983/solr/admin/cores?wt=json
```
```
curl localhost:8983/solr/admin/collections?action=LIST&wt=json

```

## force commit for core/collection
```
http://localhost:8983/solr/collection1/update?commit=true
```

## insert new record
* xml
```
curl http://localhost:8983/solr/update?commit=true -H "Content-Type: text/xml" --data-binary '<add><doc><field name="id">10010</field><field name="title">title #10010</field></doc></add>'
```

* json simple version
```
curl http://localhost:8983/solr/update?commit=true -H "Content-Type: text/json" --data-binary '[{"id":10011, "title": "title #10011"}]'
```

* json full request
```
curl http://localhost:8983/solr/update?commit=true -H "Content-Type: text/json" --data-binary '{"add":{ "doc":{"id":"1021","title":"title 1021"},"boost":1.0,"overwrite":true,"commitWithin":1}}'
```

* json POST request
```
curl -X POST http://localhost:8983/solr/collection1/update?commit=true -H "Content-Type: text/json" --data '{"add":{ "doc":{"id":"1023","title":"title 1023"},"boost":1.0,"overwrite":true,"commitWithin":1}}'
```

## select records, execute query
* xml
```
curl http://localhost:8983/solr/select?q=*:*
```

* json
```
curl -X GET -H "Accept: application/json, text/javascript" "http://localhost:8983/solr/collection1/select?q=*%3A*&wt=json&indent=true&_=1537470880014"
```

## delete all from collection
```
curl http://localhost:8983/solr/collection1/update?commit=true -H "Content-Type: text/xml" --data-binary '<delete><query>*:*</query></delete>'
```


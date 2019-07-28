[documentation](http://archive.apache.org/dist/lucene/solr/ref-guide/)

# Key terms
![Core, collection](https://i.postimg.cc/HLGhMgMd/Solr-_Core-_Collection.png)



# generate configuration of instance
```bash
solrctl instancedir --generate $HOME/label_collection_config
```

# create instance based on configuration 
```bash
solrctl --zk 134.191.209.235:2181/solr instancedir --create label_collection_config $HOME/label_collection_config
```

# create collection 
```bash
solrctl --zk 134.191.209.235:2181/solr collection --create label_collection -s 5 -c label_collection_config
```

# copy collection/core manually 
* cp solr/example/solr/collection1 solr/example/solr/collection2
* rm solr/example/solr/collection2/data
* core.properties # name=collection2
* change schema.xml [doc](https://wiki.apache.org/solr/SchemaXml) [src-code](https://github.com/apache/lucene-solr/blob/master/solr/solr-ref-guide/src/field-type-definitions-and-properties.adoc)


# REST API collaboration
[Official documentation for different versions](http://archive.apache.org/dist/lucene/solr/ref-guide/)
[Solr REST API admin](https://lucene.apache.org/solr/guide/6_6/coreadmin-api.html)

**TIP:** *investigate request/response of the Solr UI*

## request types:
* wt=json
* wt=xml

## system info
```
curl -i -k --negotiate -u: https://134.190.200.9:8983/solr/admin/info/system?wt=xml
curl -i -k --negotiate -u: https://134.190.200.9:8983/solr/admin/info/system?wt=json
curl 134.190.200.9:8983/solr/admin/info/system?wt=json
```

## read collections
```
curl -s localhost:8983/solr/admin/cores?wt=json
curl -i -k --negotiate -u: https://34.91.11.49:8985/solr/admin/collection_labels
```
request to server with https
```
curl -i -k --negotiate -u: https://localhost:8983/solr/admin/cores?wt=json
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

## select records, execute query, read records
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

## delete core/collection itself
```
curl localhost:8985/solr/admin/cores?action=UNLOAD&deleteInstanceDir=true&core=collection1
```

# command line parameters
* -Dsolr.admin.port=8984
SOLR_PORT

* -Dsolr.port=8985
SOLR_ADMIN_PORT

* -Dsolr.solr.home=/var/lib/solr

# folders
* log: /var/log/solr/
* conf: /etc/solr/conf
* collection conf: /home/solr_deploy_eq_s/solr_deploy/labelsCollection/conf/schema.xml

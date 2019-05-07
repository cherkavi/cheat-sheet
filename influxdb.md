list of databases
```
curl --silent -X GET "http://tesla-influx.k8sstg.mueq.adas.intel.com/query?pretty=true&db=metrics&q=SHOW%20MEASUREMENTS"
```

default settings
```
curl --silent -X GET "http://tesla-influx.k8sstg.mueq.adas.intel.com/query?pretty=true&db=metrics&q=SHOW%20RETENTION%20POLICIES"
```

list of all "tables"
```
curl --silent -X GET "http://tesla-influx.k8sstg.mueq.adas.intel.com/query?pretty=true&db=metrics&q=SHOW%20FIELD%20KEYS" | grep "name\":" | awk '{print $2}' | tr , ' '
```

examples of requests
```
curl -G 'http://tesla-influx.k8sstg.mueq.adas.intel.com/query?pretty=true' --data-urlencode "db=metrics" --data-urlencode "q=SELECT jobId FROM \"events\" limit 10"
curl -G 'http://tesla-influx.k8sstg.mueq.adas.intel.com/query?pretty=true' --data-urlencode "db=metrics" --data-urlencode "q=SELECT distinct \"jobGroup\" FROM \"events\" limit 10"
curl -G 'http://tesla-influx.k8sstg.mueq.adas.intel.com/query?pretty=true' --data-urlencode "db=metrics" --data-urlencode "q=SELECT \"name\",\"jobGroup\" FROM \"events\" WHERE \"jobGroup\"='SessionMetaData' limit 10"
curl -G 'http://tesla-influx.k8sstg.mueq.adas.intel.com/query?pretty=true' --data-urlencode "db=metrics" --data-urlencode "q=SELECT * FROM \"events\" WHERE \"type\"='start' and \"applicationName\"='SessionIngestJob$' limit 10"
```

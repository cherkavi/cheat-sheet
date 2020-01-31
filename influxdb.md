[official documentation](https://docs.influxdata.com/influxdb/v1.7/query_language/schema_exploration/)

list of all databases
```
curl --silent -G "http://tesla-influx.k8sstg.mueq.adas.intel.com/query?pretty=true" \
--data-urlencode "q=SHOW DATABASES"
```

show retention policy for specific database
```
curl --silent -G "http://tesla-influx.k8sstg.mueq.adas.intel.com/query?pretty=true" \
--data-urlencode "db=metrics" \
--data-urlencode "q=SHOW RETENTION POLICIES"
```

list of all measurements
```
curl --silent -G "http://tesla-influx.k8sstg.mueq.adas.intel.com/query?pretty=true" \
--data-urlencode "db=metrics" \
--data-urlencode "q=SHOW MEASUREMENTS"

curl --silent -G "http://tesla-influx.k8sstg.mueq.adas.intel.com/query?pretty=true" \
--data-urlencode "db=metrics" \
--data-urlencode "q=SHOW FIELD KEYS" \
| grep "name\":" | awk '{print $2}' | tr , ' '
```

delete record
```
curl --silent -G "https://dq-influxdb.dplapps.vantage.org:443/query?pretty=true" \
--data-urlencode "db=dataquality-metrics" \
--data-urlencode "q=DROP SERIES FROM \"km-dr\" WHERE \"session\"='aa416-7dcc-4537-8045-83afa2' and \"vin\"='V77777'"

```

list of all fields and keys ( for measurements )
```
curl --silent -G "http://tesla-influx.k8sstg.mueq.adas.intel.com/query?pretty=true" \
--data-urlencode "db=metrics" \
--data-urlencode "q=SHOW FIELD KEYS"
```

examples of requests
( be carefull with string values and single quota )
```
curl -G 'http://tesla-influx.k8sstg.mueq.adas.intel.com/query?pretty=true' --data-urlencode "db=metrics" --data-urlencode "q=SELECT jobId FROM \"events\" limit 10"
curl -G 'http://tesla-influx.k8sstg.mueq.adas.intel.com/query?pretty=true' --data-urlencode "db=metrics" --data-urlencode "q=SELECT distinct \"jobGroup\" FROM \"events\" limit 10"
curl -G 'http://tesla-influx.k8sstg.mueq.adas.intel.com/query?pretty=true' --data-urlencode "db=metrics" --data-urlencode "q=SELECT \"name\",\"jobGroup\" FROM \"events\" WHERE \"jobGroup\"='SessionMetaData' limit 10"
curl -G 'http://tesla-influx.k8sstg.mueq.adas.intel.com/query?pretty=true' --data-urlencode "db=metrics" --data-urlencode "q=SELECT * FROM \"events\" WHERE \"type\"='start' and \"applicationName\"='SessionIngestJob$' limit 10"
```

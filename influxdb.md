# [InfluxDB official documentation](https://docs.influxdata.com/influxdb/v2.7/query_language/schema_exploration/)
  TICK:  
* Telegram
*  InfluxDB
*   Chronograf
*    Kapacitor

## Data Model: 
InfluxDB has a specialized data model optimized for time-series data. 
It organizes data into :
* Database - container for organizing related data, in version 2.x - Bucket
* Retention Policy - one or more inside database: duration, replication, TimeToLive
* measurements - one or more inside Retention Policy ( like tables in an RDBMS )
* tags - are indexed metadata associated with data points
* fields - store the actual data values
* timestamps - represent when the data was recorded

##  [start influxdb in docker container ](https://hub.docker.com/_/influxdb/)
### start with default config 
```sh
INFLUX_TAG=2.7.1
# INFLUX_TAG=latest
docker rm influxdb
docker run --name influxdb \
    --publish 8086:8086 \
    --volume influxdb:/var/lib/influxdb \
    influxdb:${INFLUX_TAG}
```

### start influx with custom config
```sh
# INFLUX_TAG=2.0
INFLUX_TAG=2.7.1

docker run --name influxdb \
    --publish 8086:8086 \
    --volume influxdb:/var/lib/influxdb \
    --volume $PWD/config.yml:/etc/influxdb2/config.yml \
    influxdb:${INFLUX_TAG}

# influxd -config /path/to/influxdb.conf
```
config example
```properties
reporting-disabled = true

[meta]
  dir = "/var/lib/influxdb/meta"

[data]
  dir = "/var/lib/influxdb/data"
  wal-dir = "/var/lib/influxdb/wal"

[http]
  enabled = true
  bind-address = ":8086"
  auth-enabled = true
```


## connect to existing container 
```sh
docker exec -it influxdb /bin/bash
```

## use Influx CLI
### print current instance config
```sh
influxd print-config
```

### [influxdb setup, create bucket](https://docs.influxdata.com/influxdb/v2.7/reference/cli/influx/setup/)
```sh
INFLUX_USER=my-user
INFLUX_PASS=my-pass-my-pass
INFLUX_BUCKET=my-bucket
INFLUX_CONFIG=my-local-config
INFLUX_URL=localhost:8086

influx setup --org myself-org --bucket $INFLUX_BUCKET --username $INFLUX_USER --password $INFLUX_PASS --force  
influx config list 
INFLUX_TOKEN=`cat /etc/influxdb2/influx-configs | grep "^  token =" | awk '{print $3}' | awk -F '"' '{print $2}'`
echo $INFLUX_TOKEN
```


### [create config for influx db](https://docs.influxdata.com/influxdb/v2.6/get-started/setup)
if it exists - not possible to make a setup 
```sh
# INFLUX_TOKEN=my-secret-token
# INFLUX_CONFIG=my-local-config
# # influx config create --help
# influx config create --active \
#   -n $INFLUX_CONFIG \
#   -t $INFLUX_TOKEN \
#   -u http://localhost:8086 \
#   -o myself-org
# 
# # config list
# influx config --help
# influx config list 
# influx config rm $INFLUX_CONFIG
```

### influx query
#### influx query via cli
```sh
# completion
eval $(influx completion bash)

# list of buckets
influx bucket list

# simple query
INFLUX_BUCKET=my-bucket
influx query "from(bucket:\"${INFLUX_BUCKET}\") |> range(start:-1m)" --raw
```

#### [run interactive shell ](https://help.influxcloud.net/getting_started_cli/)
```sh
influx v1 shell
```
```sh
show databases
use my-bucket
```
```sh
# INSERT exampletable, field=1 field2=21 field3=test1, value=0.55 1472666050
# INSERT exampletable, field=1, field2=21 1593122400000000000
# INSERT exampletable,tag1=2 tag2=22 tag3=test2 1439856720000000000
# INSERT exampletable,tag1=3 tag2=23 tag3=test3 1439856000000000000
# 
# insert codenarc,maxPriority2Violations=917,maxPriority3Violations=3336 value=0.10 1593122400000000000
# insert codenarc,maxPriority2Violations=917,maxPriority3Violations=3336 value=0.10 1472666050
# 
# select * from exampletable;
# select * from temperature;

select * from foods;
```

#### influx query via curl
```sh
# show database 
curl --header "Authorization: Token $INFLUX_TOKEN" --header "Content-Type: application/x-www-form-urlencoded" -G "$INFLUX_URL/query?pretty=true" \
  --data-urlencode "q=SHOW DATABASES"

# show retention policy for specific database
curl --header "Authorization: Token $INFLUX_TOKEN" --header "Content-Type: application/x-www-form-urlencoded" -G "$INFLUX_URL/query?pretty=true" \
  --data-urlencode "db=${INFLUX_BUCKET}" \
  --data-urlencode "q=SHOW RETENTION POLICIES"

# show all data 
curl --header "Authorization: Token $INFLUX_TOKEN" --header "Content-Type: application/x-www-form-urlencoded" -G "$INFLUX_URL/query?pretty=true" \
  --data-urlencode "db=${INFLUX_BUCKET}" \
  --data-urlencode "q=SHOW MEASUREMENTS"

curl --header "Authorization: Token $INFLUX_TOKEN" --header "Content-Type: application/x-www-form-urlencoded" -G "$INFLUX_URL/query?pretty=true"

# show schema
curl --header "Authorization: Token $INFLUX_TOKEN" --header "Content-Type: application/x-www-form-urlencoded" -G "$INFLUX_URL/query?pretty=true" \
--data-urlencode "db=${INFLUX_BUCKET}" \
--data-urlencode "q=SHOW FIELD KEYS"
```

```sh
# insert data 
# --header "Content-Type: text"
curl -v --header "Authorization: Token $INFLUX_TOKEN" --header "Content-Type: application/x-www-form-urlencoded"  \
  -X POST -G "$INFLUX_URL/write?db=${INFLUX_BUCKET}&precision=s" \
  --data-urlencode "db=${INFLUX_BUCKET}" \
  --data-urlencode "temperature,location=1 value=90 1472666050"

curl -v --header "Authorization: Token $INFLUX_TOKEN" --header "Content-Type: application/x-www-form-urlencoded"  \
  -X POST -G "$INFLUX_URL/write?db=${INFLUX_BUCKET}&precision=s" \
  --data-urlencode "db=${INFLUX_BUCKET}" \
  --data-urlencode "temperature,location=1 value=91 1439856720000000000"
```
```sh
# select data 
curl --header "Authorization: Token $INFLUX_TOKEN" --header "Content-Type: application/x-www-form-urlencoded" -G "$INFLUX_URL/query?pretty=true" \
  --data-urlencode "db=${INFLUX_BUCKET}" \
  --data-urlencode "q=select * from foods"

# "q=SELECT * FROM \"events\" WHERE \"type\"='start' and \"applicationName\"='SessionIngestJob$' limit 10"  

curl -G 'http://tesla-influx.k8sstg.mueq.adas.intel.com/query?pretty=true' --data-urlencode "db=metrics" --data-urlencode "q=SELECT jobId FROM \"events\" limit 10"
```
delete record
```sh
curl --silent -G "https://dq-influxdb.dplapps.vantage.org:443/query?pretty=true" \
--data-urlencode "db=${INFLUX_BUCKET}" \
--data-urlencode "q=DROP SERIES FROM \"km-dr\" WHERE \"session\"='aa416-7dcc-4537-8045-83afa2' and \"vin\"='V77777'"

```

```sql
CREATE USER telegraf WITH PASSWORD 'telegrafmetrics' WITH ALL PRIVILEGES
```


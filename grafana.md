# Grafana

## Docker images
* [standalone grafana](https://github.com/cherkavi/docker-images/blob/master/grafana/README.md)
* [part of TICK stack](https://github.com/cherkavi/docker-images/blob/master/telegraf/README.md)
* [prometheus with grafana](https://github.com/cherkavi/docker-images/blob/master/prometheus/README.md#alerts--prometheus--grafana)

## [rest api documentation](https://grafana.com/docs/grafana/latest/developers/http_api/)
```sh
GRAFANA_HOST=grafana-monitoring.vantage.zur
GRAFANA_URL=https://$GRAFANA_HOST
TOKEN=866d56cc....
curl -X GET -H "Authorization: Bearer $TOKEN" ${GRAFANA_URL}/api/users?perpage=10&page=1
```

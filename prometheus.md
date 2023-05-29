# Prometheus
pull metrics from different sources and write them in TSDB ( internal, InfluxDB ... )
## ![Architecture](https://cdn.rawgit.com/prometheus/prometheus/e761f0d/documentation/images/architecture.svg)

## links
* [prometheus documentation](https://prometheus.io/docs/prometheus/latest/)
* [prometheus docker](https://hub.docker.com/r/prom/prometheus/)
* [prometheus git](https://github.com/prometheus/prometheus)

## [prometheus in docker](https://github.com/cherkavi/docker-images/blob/master/prometheus/README.md)
```sh
# --volume /path/to/prometheus.yml:/etc/prometheus/prometheus.yml \
docker run --name prometheus -d --publish 9090:9090 prom/prometheus
```
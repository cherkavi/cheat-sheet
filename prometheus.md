# Prometheus
pull metrics from different sources and write them in TSDB ( internal, InfluxDB ... )
## ![Architecture](https://cdn.rawgit.com/prometheus/prometheus/e761f0d/documentation/images/architecture.svg)

## links
* [prometheus documentation](https://prometheus.io/docs/prometheus/latest/)
* [prometheus docker](https://hub.docker.com/r/prom/prometheus/)
* [prometheus git](https://github.com/prometheus/prometheus)
* [prometheus functions](https://prometheus.io/docs/prometheus/latest/querying/functions/)

## [prometheus in docker](https://github.com/cherkavi/docker-images/blob/master/prometheus/README.md)
```sh
# --volume /path/to/prometheus.yml:/etc/prometheus/prometheus.yml \
docker run --name prometheus -d --publish 9090:9090 prom/prometheus
```

## ports of Prometheus ecosystem
* 9090 for Prometheus
* 9093 for the Alertmanager

## [prometheus exporters list](https://github.com/prometheus/docs/blob/main/content/docs/instrumenting/exporters.md)

### [prometheus node exporter](https://prometheus.io/docs/guides/node-exporter/)
prometheus is working in pull mode, that means 
observed system should emit http-endpoint on some port
* [docker container with node exporter](https://github.com/prometheus/node_exporter)
* [manual installation of node exporter](https://codewizardly.com/prometheus-on-aws-ec2-part2/)


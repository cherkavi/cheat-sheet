# [Telegraf](https://github.com/influxdata/telegraf)
Telegraf is an open-source agent written in the Go.  
It collects, manages, processes, and aggregates metrics of a DevOps system or machine  

[telegraf input plugins](https://www.influxdata.com/time-series-platform/telegraf/telegraf-input-plugin/)

## [docker containers of TICK stack](https://github.com/influxdata/influxdata-docker)
> infrastructure monitoring, alert management, data visualization, and database management
* chronograf 
  > to visualize your monitoring data and easily create alerting and automation rules.
  * [chronograf docker](https://registry.hub.docker.com/_/chronograf/)
  * [chronograf doc](https://docs.influxdata.com/chronograf/v1.10/)
* kapacitor
  > to import (stream or batch) time series data, and then transform, analyze, and act on the data.
  * [kapacitor docker](https://registry.hub.docker.com/_/kapacitor/)
  * [kapacitor doc](https://docs.influxdata.com/kapacitor/v1.6/introduction/getting-started/)
* [telegraf](https://docs.influxdata.com/platform/install-and-deploy/deploying/sandbox-install/)
* influxdb

## Possible run of telegram with diff configuration
* Filename: ` --config /etc/default/telegraf`
* include all files ending with .conf: `--config-directory /path/to/folder` 
* Remote URL: ` --config "http://remote-URL-endpoint"`

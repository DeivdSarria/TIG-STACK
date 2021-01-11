# TIG-STACK

Telegraf, influxdb and grafana on docker to show simple configurations.

## InfluxDB

Pull influxDB image

```shell
Docker pull influxdb
```
Create volume for influxdb DDBB to persist data locally
```shell
docker volume create influxdb-volume
```
create container with necessary DDBB variables, then is eliminated and its config persisted in our previosly created volume.
```shell
docker run --rm \
  -e INFLUXDB_DB=telegraf -e INFLUXDB_ADMIN_ENABLED=true \
  -e INFLUXDB_ADMIN_USER=admin \
  -e INFLUXDB_ADMIN_PASSWORD=admin123 \
  -e INFLUXDB_USER=telegraf -e INFLUXDB_USER_PASSWORD=telegraf123 \
  -v influxdb-volume:/var/lib/influxdb \
  influxdb /init-influxdb.sh
```

generate a container with mapped ports and the created volume with the proper config

```shell
docker run -d -p 8086:8086 --name=influxdb -v influxdb-volume:/var/lib/influxdb influxdb
```

## TELEGRAF

Pull Telegraf image

```shell
docker pull telegraf
```
start a telegraf container with our config and mapped ports
```shell
docker run -d -p 8092:8092 --name=telegraf -v ${PWD}/Telegraf/config/telegraf.conf:/etc/telegraf/telegraf.conf:ro telegraf
```

## GRAFANA

Pull Grafana image

```shell
docker pull grafana/grafana
```

Create volume to store our grafana dashboard and our data source 

```shell
docker volume create volume-grafana
```

Start grafana container with mapped ports and volume 

```shell
docker run -d -p 3000:3000 --name=grafana -v volume-grafana:/var/lib/grafana grafana/grafana
```
------ INFLUXDB ------

- Docker pull influx

- Crear filesystem: Estructura de carpetas

- Influx comando

docker volume create influxdb-volume

docker run --rm \
  -e INFLUXDB_DB=telegraf -e INFLUXDB_ADMIN_ENABLED=true \
  -e INFLUXDB_ADMIN_USER=admin \
  -e INFLUXDB_ADMIN_PASSWORD=admin123 \
  -e INFLUXDB_USER=telegraf -e INFLUXDB_USER_PASSWORD=telegraf123 \
  -v influxdb-volume:/var/lib/influxdb \

  influxdb /init-influxdb.sh

- Docker run para generar un contenedor con puertos y volumenes necesarios
  por donde pasamos al conetendor la configuracion local.

	docker run -d -p 8086:8086 --name=influxdb -v influxdb-volume:/var/lib/influxdb influxdb


------ TELEGRAF -------

user: telegraf
pass: telegraf123

docker run -d -p 8092:8092 --name=telegraf -v ${PWD}/Telegraf/config/telegraf.conf:/etc/telegraf/telegraf.conf:ro telegraf



------ GRAFANA ------

Volumen: volume-grafana

Inicializo contenedor añadiendo el almacenamiento: 

docker run -d -p 3000:3000 --name=grafana -v volume-grafana:/var/lib/grafana grafana/grafana





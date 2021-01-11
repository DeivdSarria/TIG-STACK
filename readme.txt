------ INFLUXDB ------

- Docker pull influx

- Crear filesystem: Estructura de carpetas

- Crear iql: 

	CREATE DATABASE weather;
	CREATE RETENTION POLICY one_week ON weather DURATION 168h REPLICATION 1 DEFAULT;

- Generar metadata a partir de comando influxdb (entrypoint para scripts) (Si no se lanza el comando, borrar meta y data local)

	docker run --rm -e INFLUXDB_HTTP_AUTH_ENABLED=true 
	-e INFLUXDB_ADMIN_USER=admin -e INFLUXDB_ADMIN_PASSWORD=admin123 
	-v C:\TIG\etc\influxdb:/var/lib/influxdb 
	-v C:\TIG\etc\influxdb\scripts:/docker-entrypoint-initdb.d 
	influxdb /init-influxdb.sh

- Docker run para generar un contenedor con puertos y volumenes necesarios
  por donde pasamos al conetendor la configuracion local.

	docker run -d -p 8086:8086 --name=influxdb 
	-v C:\TIG\etc\influxdb\influxdb.conf:/etc/influxdb/influxdb.conf 
	-v C:\TIG\etc\influxdb\meta\meta.db:/var/lib/influxdb/meta/meta.db 
	-v C:\TIG\var\lib\influxdb:/var/lib/influxdb influxdb -config /etc/influxdb/influxdb.conf


------ TELEGRAF -------

- Creo el archivo de configuracion de telegraf de la imagen oficial en mi ruta local.


user: telegraf
pass: telegraf123

docker run -d -p 8092:8092 --name=telegraf 
--net=influxdb:  
-e C:\TIG\etc\telegraf=/host/proc 
-v /proc:/host/proc:ro 
-v C:\TIG\etc\telegraf\telegraf.conf:/etc/telegraf/telegraf.conf:ro 
telegraf


- Crear volumen extra para Grafana-> store dashboards and data source
- Añadir telgraf a docker-compose. (Possible? If not manual run y por CLI crear telegraf db)
- Añadir network monitoring a docker compose para aislarlos
- Si es posible. Automatizar generación de .IQL para la creación automática de la base de datos


------ GRAFANA ------

Volumen: volume-grafana

Inicializo contenedor añadiendo el almacenamiento: 

docker run -d -p 3000:3000 --name=grafana2 
-v volume-grafana:/var/lib/grafana grafana/grafana





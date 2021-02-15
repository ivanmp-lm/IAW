---
description: >-
  En esta práctica se usará un conjunto de las siguientes aplicaciones para
  crear un dashboard con MQTT, Telegraf, InfluxDB y Grafana.
---

# Práctica 18 - IoTT Dashboard

**Todo el contenido utilizado para esta práctica ha salido del siguiente repositorio de GitHub:** 

{% embed url="https://github.com/josejuansanchez/co2-celia" %}

Se creará una máquina en Amazon Web Services cuyo propósito será almacenar gráficas con los datos atmosféricos \(en concreto los niveles de CO2\) del aula número 22 del IES Celia Viñas.

Se implementarán los cuatro servicios utilizando Docker Compose. El archivo utilizado para ello contiene lo siguiente:

```text
version: '3'

services: 
  mosquitto:
    image: eclipse-mosquitto:2
    ports:
      - 1883:1883
    volumes:
      - ./mosquitto/mosquitto.conf:/mosquitto/config/mosquitto.conf
      - mosquitto_data:/mosquitto/data
      - mosquitto_log:/mosquitto/log

  telegraf:
    image: telegraf
    volumes:
      - ./telegraf/telegraf.conf:/etc/telegraf/telegraf.conf
    depends_on: 
      - influxdb

  influxdb:
    image: influxdb
    ports:
      - 8086:8086
    volumes:
      - influxdb_data:/var/lib/influxdb
    environment:
      - INFLUXDB_DB=${INFLUXDB_DB}
      - INFLUXDB_ADMIN_USER=${INFLUXDB_USERNAME}
      - INFLUXDB_ADMIN_PASSWORD=${INFLUXDB_PASSWORD}
      - INFLUXDB_HTTP_AUTH_ENABLED=true

  grafana:
    image: grafana/grafana:7.4.0
    ports:
      - 3000:3000
    volumes:
      - grafana_data:/var/lib/grafana
      - ./grafana-provisioning/:/etc/grafana/provisioning
    depends_on:
      - influxdb

  chronograf:
    image: chronograf
    ports:
      - 8888:8888
    volumes:
      - chronograf_data:/var/lib/chronograf
    depends_on:
      - influxdb
    environment:
      - INFLUXDB_URL=http://influxdb:8086
      - INFLUXDB_USERNAME=${INFLUXDB_USERNAME}
      - INFLUXDB_PASSWORD=${INFLUXDB_PASSWORD}

volumes:
  mosquitto_data:
  mosquitto_log:
  node_red_user_data:
  influxdb_data:
  grafana_data:
  chronograf_data:
```

Se utilizará Mosquitto como servicio MQTT, InfluxDB como base de datos, Grafana como interfaz web para implementar los gráficos necesarios para la práctica, chronograf que comunicará los datos en InfluxDB con los de Grafana y por último telegraf como software que se utilizará de software para el sensor.

Los datos de entorno para el fichero de Docker Compose serán los siguientes:

```text
INFLUXDB_DB=iescelia_db
INFLUXDB_USERNAME=root
INFLUXDB_PASSWORD=root
```

Simplemente se indicará el nombre de la base de datos en InfluxDB que será "iescelia\_db" junto con sus credenciales en caso de necesitar conectarse a la misma.




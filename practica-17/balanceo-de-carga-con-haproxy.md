---
description: >-
  En esta práctica se hará un balanceo de carga utilizando HAProxy en la
  aplicación LAMP dockerizada.
---

# Balanceo de carga con HAProxy



Para empezar la práctica, se partirá de los archivos realizados para la práctica 16, siendo necesario modificar el archivo ".yml" para incluir lo siguiente:

```text
  balanceador:
    image: dockercloud/haproxy
    ports:
      - 80:80
      - 1936:1936
    links:
      - apache
    volumes:
      - /var/run/docker.sock:/var/run/docker.sock
    networks:
      - backend-network
      - frontend-network
```

Este bloque indica que la imagen de HAProxy se ejecutará bajo los puertos 80 \(servicio a balancear\) y 1936 \(el puerto que se usará para acceder a la página de estadísticas del balanceador\). Hay que tener en cuenta que se debe abrir este nuevo puerto de forma manual en la máquina de Amazon.

La línea "links" sirve para crear un enlace entre la máquina del balanceador y la de apache y el volumen creado es necesario para montar el socket UNIX de docker para permitir al balanceador comunicarse con el resto de imágenes \(como Apache\).

Con la nueva imagen añadida al archivo de la práctica anterior, se ejecutará el siguiente comando para lanzar el archivo de Docker Compose:

```text
$ sudo docker-compose up -d --scale apache=4
```

Nótese el añadido al final, el argumento "scale" que permite lanzar más de un servicio del mismo tipo a la vez, siendo esta vez 4 contenedores del Apache que contiene la aplicación LAMP.


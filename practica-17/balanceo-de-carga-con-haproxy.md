---
description: >-
  En esta práctica se hará un balanceo de carga utilizando HAProxy en la
  aplicación LAMP dockerizada.
---

# Balanceo de carga con HAProxy

* **IP:** [3.236.103.246](http://3.236.103.246)
* **ACCESO PHPMYADMIN:** [3.236.103.246:8080](http://3.236.103.246:8080)
* **USUARIO PHPMYADMIN:** root
* **CLAVE PHPMYADMIN:** root

Para empezar la práctica, se partirá de los archivos realizados para la práctica 16, siendo necesario modificar el archivo ".yml" para incluir lo siguiente \(el servicio de apache deberá ir sin puerto\):

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

Acceder a la página tendrá el siguiente resultado:

![](../.gitbook/assets/image%20%2844%29.png)

Para que la base de datos funcione se ejecutará el siguiente script en PHPMyAdmin igual que en la práctica anterior:

```text
DROP DATABASE IF EXISTS lamp_db;
CREATE DATABASE lamp_db CHARSET utf8mb4;
USE lamp_db;

CREATE TABLE users (
  id int(11) NOT NULL auto_increment,
  name varchar(100) NOT NULL,
  age int(3) NOT NULL,
  email varchar(100) NOT NULL,
  PRIMARY KEY (id)
) ENGINE=InnoDB DEFAULT CHARSET=utf8;

CREATE USER IF NOT EXISTS 'lamp_user'@'%';
SET PASSWORD FOR 'lamp_user'@'%' = 'lamp_password';
GRANT ALL PRIVILEGES ON lamp_db.* TO 'lamp_user'@'%';
```

Página con datos:

![](../.gitbook/assets/image%20%2836%29.png)

Si se accede a través del puerto 1936 se accederá a la página de estadísticas \(las credenciales son stats/stats\):

![](../.gitbook/assets/image%20%2835%29.png)

Tras actualizar unas cuantas veces, se puede observar que el balanceador realiza la rotación correctamente y todos los servicios de apache han recibido peticiones.


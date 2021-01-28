---
description: >-
  En esta práctica se usará docker y docker compose para lanzar un servidor
  apache con Prestashop.
---

# Práctica Prestashop

Esta práctica es muy parecida a la anterior, pero se sustituirá el CMS wordpress por prestashop que está pensado para lanzar una tienda online. Igual que la anterior, se usará docker y docker-compose por lo que el primer paso será instalar ambas aplicaciones:

```text
$ sudo apt update
$ sudo apt install docker.io
$ sudo apt install docker-compose
```

Una vez hecho esto, se importará el archivo de docker-compose incluído en el directorio de este repositorio, que tiene el siguiente contenido:

```text
version: '3.4'


services:
  mysql:
    image: mysql:5.7.28
    ports: 
      - 3306:3306
    environment: 
      - MYSQL_ROOT_PASSWORD=${MYSQL_ROOT_PASSWORD}
      - MYSQL_DATABASE=${MYSQL_DATABASE}
      - MYSQL_USER=${MYSQL_USER}
      - MYSQL_PASSWORD=${MYSQL_PASSWORD}
    volumes: 
      - mysql_data:/var/lib/mysql
    networks:
      - backend-network
    restart: always

  phpmyadmin:
    image: phpmyadmin
    ports:
      - 8080:80
    depends_on:
      - "mysql"
    environment: 
      - PMA_ARBITRARY=1
    networks:
      - backend-network
      - frontend-network
    restart: always

  prestashop:
    image: prestashop/prestashop
    ports: 
      - 80:80
    depends_on:
      - "mysql"
    environment: 
      - DB_NAME=${DB_NAME}
      - DB_USER=${DB_USER}
      - DB_PASSWORD=${DB_PASSWORD}
    volumes:
      - prestashop_data:/var/www/html
    networks:
      - backend-network
      - frontend-network
    restart: always

volumes:
  mysql_data:
  prestashop_data:

networks:
  backend-network:
  frontend-network:
```

Es prácticamente idéntico pero en este caso la imagen de wordpress desaparece para dejar paso a prestashop. Las imágenes de phpmyadmin y prestashop dependen de mysql como se puede observar en las cláusulas "depends\_on", por lo que este servicio será el que se ejecute en primer lugar.

También será necesario el archivo con el contenido de las variables de entorno referenciadas:

```text
MYSQL_ROOT_PASSWORD=root
MYSQL_DATABASE=prestashop
MYSQL_USER=pst_user
MYSQL_PASSWORD=pst_password
DB_NAME=mysql
DB_USER=pst_user
DB_PASSWORD=pst_password
```




---
description: >-
  Esta práctica sigue la estela de la anterior pero esta vez se utilizará Docker
  Compose para lanzar la página.
---

# HTTPS con Docker Compose

Como en el resto de prácticas, se instalará Docker y Docker Compose en la máquina de Ubuntu creada:

```text
$ sudo apt update
$ sudo apt install docker.io
$ sudo apt install docker-compose
```

Se reutilizará el nombre de dominio anterior para esta práctica, redirigiendo los registros de tipo A creados en la anterior para apuntar a la nueva máquina y se modificará el archivo ".yml" de la [práctica de prestashop](https://github.com/ivanmp-lm/IAW-Practica-Prestashop) para incluir un nuevo contenedor con "**HTTPS-PORTAL**".

Este contenedor lanzará una imagen con Nginx y Let's Encrypt para proporcionar un certificado SSL. Se añadirá el siguiente bloque al archivo de la práctica de Prestashop:                                                                                                                                                                                                                                                                                   

```text
https-portal:
  image: steveltn/https-portal:1
  ports:
    - 80:80
    - 443:443
  environment:
    DOMAINS: 'practicacertbotivan.tk -> http://prestashop:80'
    #STAGE: 'production' # Don't use production until staging works
```

Este bloque utilizará la imagen de https-portal bajo los puertos 80 y 443, que deberán estar abiertos en la configuración de seguridad de la máquina de Amazon. El puerto 80 se utilizará aquí y se redireccionarán sus peticiones al puerto 80 de la imagen de prestashop, por lo que se eliminará su puerto del archivo final. El resultado será el siguiente:

```text
version: '3.4'


services:
  mysql:
    image: mysql
    command: --default-authentication-plugin=mysql_native_password
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

  https-portal:
    image: steveltn/https-portal:1
    ports:
      - 80:80
      - 443:443
    environment:
      DOMAINS: 'practicacertbotivan.tk -> http://prestashop:80'
      STAGE: 'production' # Don't use production until staging works

volumes:
  mysql_data:
  prestashop_data:

networks:
  backend-network:
  frontend-network:
```

Error al comprobar:

![](../.gitbook/assets/image%20%2836%29.png)


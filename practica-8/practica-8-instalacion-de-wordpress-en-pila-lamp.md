---
description: >-
  En esta práctica se combinará el trabajo de las anteriores para alojar páginas
  web funcionales utilizando el CMS Wordpress.
---

# Práctica 8 - Instalación de Wordpress en pila LAMP

**Los recursos necesarios para esta práctica están en el siguiente** [**repositorio**](https://github.com/ivanmp-lm/IAW-Practica-08)\*\*\*\*

La práctica se dividirá en tres fases:

1. **Fase 0:** Instalación de Wordpress en una única máquina
2. **Fase 1:** Wordpress en dos niveles. Front-End con Wordpress y Back-End con MySQL Server
3. **Fase 2:** Wordpress en tres niveles. Balanceador de carga, dos Front-End y Back-End

La AMI utilizada en todas las fases será la de siempre, Ubuntu Server 20.04.

## 1. Fase 0 

Todo el servidor funcionará en una sola máquina, se empezará por actualizar los repositorios e instalar todo el software necesario:

```text
$ sudo apt update
$ sudo apt install apache2
$ sudo apt install mysql-server
$ apt install php libapache2-mod-php php-mysql
```

Tras esto, estará todo instalado excepto Wordpress, el cual necesita de configuración adicional para su funcionamiento. Para probar el servidor de Apache y en caso de que sea necesario consultar alguna ruta o información, se creará el archivo `/var/www/html/info.php` 

```text
<?php
phpinfo();
?>
```

En la misma ruta, se descargará el paquete de Wordpress con el siguiente comando:

```text
$ sudo wget http://wordpress.org/latest.tar.gz
$ sudo tar -xzvf latest.tar.gz
```

Con Wordpress descomprimido en la carpeta principal, se deberá crear un usuario y base de datos para que la página pueda acceder a MySQL, de lo contrario no podrá terminarse la instalación. Los comandos son los siguientes:

```text
$ sudo mysql -u root

mysql> CREATE DATABASE wordpress_db;
mysql> CREATE USER wordpress_user@localhost IDENTIFIED BY 'wordpress_password';
mysql > GRANT ALL PRIVILEGES ON db_wordpress.* TO wordpress_user@localhost;
mysql > FLUSH PRIVILEGES;
```

La base de datos " **wordpress\_db**" y el usuario "**wordpress\_user**" quedan creados, pero para que funcionen dentro de la página habrá que editar el archivo de configuración `/var/www/html/wordpress/wp-config-sample.php` que se renombrará a `wp-config.php`:

```text
$ sudo mv wp-config-sample.php wp-config.php
```

Dentro de este archivo se rellenarán cuatro campos para que Wordpress utilice la base de datos y credenciales para la misma:

```text
$ sudo sed -i "s/database_name_here/$DB_NAME/" wp-config.php
$ sudo sed -i "s/username_here/$DB_USER/" wp-config.php
$ sudo sed -i "s/password_here/$DB_PASSWORD/" wp-config.php
$ sudo sed -i "s/localhost/$IP_MYSQL_SERVER/" wp-config.php
```

Para comprobar que funciona, en el navegador se pondrá la IP pública de la máquina seguido de `/wordpress`, debería aparecer una página como la siguiente:

![](../.gitbook/assets/captura%20%281%29.png)

> **Figura 1.** Página de bienvenida de Wordpress

Tras configurar los parámetros especificados e instalar el CMS, aparecerá el panel para trabajar con la página:

![](../.gitbook/assets/captura2%20%281%29.png)

> **Figura 2.** Panel de administración de Wordpress

La fase 0 habrá finalizado.

## 2. Fase 1

Para agilizar el proceso visto anteriormente, se utilizará un script para cada máquina alojado en el repositorio indicado al principio del documento para instalar tanto el Front-End como el Back-End, después se explicará la configuración adicional necesaria para que ambas máquinas se comuniquen.

Los scripts y archivos necesarios para esta fase están alojados en este [enlace](https://github.com/ivanmp-lm/IAW-Practica-08/tree/main/Fase1).




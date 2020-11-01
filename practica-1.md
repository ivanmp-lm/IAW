---
Descripción del documento: >-
  En este documento se detallará la instalación de una pila LAMP sobre una
  máquina virtual. Se ha realizado en formato Markdown utilizando la aplicación
  web https://app.gitbook.com
---

# Práctica 1 - Pila LAMP

## 1. Instalación de la pila LAMP

La pila LAMP consiste en un conjunto de software que se complementan para montar un servidor web, es ampliamente utilizada. El nombre viene del siguiente acrónimo:

* **Linux**
* **Apache**
* **MySQL**
* **PHP**

El conjunto de estas partes, formará la pila LAMP. Para empezar, pues, vamos a necesitar partir de un SO basado en Linux, y como en cualquier instalación Linux lanzaremos los paquetes básicos para actualizar y obtener nuevos repositorios:

```text
$ sudo apt update
$ sudo apt upgrade
$ sudo apt full-upgrade
```

Con los repositorios actualizados, el siguiente paso sería instalar la herramienta **SSH**, que nos permite de forma cómoda **conectarnos remotamente a una instancia de bash y seguir lanzando comandos** desde cualquier equipo en la misma red.

```text
$ sudo apt install ssh
```

Este paso es opcional, pero siempre es recomendable tener la posibilidad de acceder de forma remota a nuestra máquina, y será esencial usar esta herramienta cuando utilicemos servidores remotos.

Ahora podemos instalar los componentes esenciales de LAMP, se hará en orden empezando por **apache:**

```text
$ sudo apt install apache2
$ sudo apt install mysql-server
$ sudo apt install php libapache2-mod-php php-mysql
```

Una vez terminemos, la pila ya estará completa pero **MySQL requiere de configuración adicional.** 

Debemos acceder como **root** a la consola MySQL con los siguientes comandos:

```text
$ sudo su
$ mysql -u root
```

Dentro de esta consola, cambiaremos la contraseña por defecto y actualizaremos los privilegios para que los cambios tengan efecto:

```text
mysql> ALTER USER 'root'@'localhost' IDENTIFIED WITH caching_sha2_password BY 'contraseña';
mysql> FLUSH PRIVILEGES;
mysql> exit
```

## 2. Comprobaciones

Con la pila LAMP completa, lanzaremos los siguientes comandos para comprobar que todo esté funcionando:

```text
$ sudo systemctl status apache2
$ sudo systemctl status mysql
```

Para comprobar php crearemos un pequeño script en el mismo lenguaje que alojaremos en `/var/www/html` para que podamos acceder desde el servidor apache.

```php
<?php

phpinfo ();

?>
```

Guardamos el archivo con el nombre `info.php`

Para acceder escribimos nuestra dirección IP en el navegador seguido de "/info.php" por ejemplo:

> 192.168.1.100/info.php

Si todo funciona correctamente, veremos una página con diversa información.

## 3. Otras aplicaciones

La pila LAMP tiene otra serie de software que normalmente va relacionado con la misma sin ser esencial, en este apartado se instalarán algunas de estas aplicaciones.

* **Aplicación propuesta**

Esta aplicación, del repositorio siguiente: [`https://github.com/josejuansanchez/iaw-practica-lamp`](https://github.com/josejuansanchez/iaw-practica-lamp)Será utilizada para realizar las prácticas con LAMP. Simplemente lo importamos con git hacia la siguiente carpeta:

```php
$ cd /var/www/html/
$ git clone https://github.com/josejuansanchez/iaw-practica-lamp
$ mv /var/www/html/iaw-practica-lamp/src/* /var/www/html/
```

* **phpMyAdmin**

Instalamos desde el repositorio:

```php
$ sudo apt install phpmyadmin php-mbstring php-zip php-gd php-json php-curl
```

Eventualmente aparecerá una ventana donde se nos pedirá que tipo de servidor web utilizaremos, seleccionamos la opción **apache2.**

Después nos preguntará si deseamos utilizar `dbconfig-common` para la base de datos y aceptamos. Por último especificaremos la contraseña para acceder a phpMyAdmin.

Para conectarnos escribimos en el navegador nuestra ip seguido de "/phpmyadmin". Ejemplo:

> 192.168.1.100/phpmyadmin

* **Adminer**

Esta aplicación es una **alternativa a phpMyAdmin** por lo que podemos usar cualquiera de las dos.

Descargamos el siguiente archivo: [https://github.com/vrana/adminer/releases/download/v4.3.1/adminer-4.3.1-mysql.php](https://github.com/vrana/adminer/releases/download/v4.3.1/adminer-4.3.1-mysql.php)

Después sólo tendremos que moverlo a `/var/www/html`

* **GoAccess**

Esta aplicación nos permitirá gestionar los archivos del log de Apache. Necesitaremos añadir su repositorio manualmente para realizar la instalación, seguimos los comandos:

```php
$ echo "deb http://deb.goaccess.io/ $(lsb_release -cs) main" | sudo tee -a /etc/apt/sources.list.d/goaccess.list
$ wget -O - https://deb.goaccess.io/gnugpg.key | sudo apt-key add -
$ sudo apt-get update
$ sudo apt-get install goaccess
```

Para utilizar GoAccess vamos a seguir los siguientes pasos. Primero implementaremos seguridad para que solo nosotros podamos ver lo que GoAccess genera a partir de los logs, para ello creamos un directorio en la ruta `/var/www/html`

```php
$ mkdir /var/www/stats
```

Iniciamos el proceso en segundo plano, pero hay que tener en cuenta que si lo hacemos desde un terminal SSH y lo cerramos, **el proceso finalizará.** Para evitar esto, añadiremos el comando `nohup` al inicio:

```php
$ nohup goaccess /var/log/apache2/access.log -o /var/www/html/stats/index.html --log-format=COMBINED --real-time-html &
```

Creamos el archivo de contraseñas. Es necesario guardar este archivo en una ruta donde no se pueda tener acceso desde el exterior:

```php
$ sudo htpasswd -c /home/usuario/.htpasswd usuario
```

Por último, editaremos el archivo de configuración de Apache al que añadiremos una pequeña sección entre las líneas &lt;VirtualHost \*:80&gt; y &lt;/VirtualHost&gt;

```php
$ sudo nano /etc/apache2/sites-enabled/000-default.conf
```

```text
<Directory "/var/www/html/stats">
  AuthType Basic
  AuthName "Acceso restringido"
  AuthBasicProvider file
  AuthUserFile "/home/usuario/.htpasswd"
  Require valid-user
</Directory>
```

## 4. Control de acceso con `.htaccess`

Creamos el archivo `.htaccess` en el directorio `stats` que hicimos en el paso anterior para la instalación de GoAccess:

```bash
$ sudo nano /var/www/html/stats/.htaccess
```

Contendrá lo siguiente:

```bash
AuthType Basic
AuthName "Acceso restringido"
AuthBasicProvider file
AuthUserFile "/home/usuario/.htpasswd"
Require valid-user
```

## 5. Script de automatización

Si tenemos que instalar esta pila en más de un dispositivo, este script nos ayudará a hacerlo de forma automática, ahorrándonos mucho tiempo. 

El script puede ser encontrado en el siguiente archivo: 

{% page-ref page="script-de-automatizacion-lamp.md" %}




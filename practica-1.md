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


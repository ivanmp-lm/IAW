---
description: En esta práctica se creará una capa front-end adicional.
---

# Práctica 4 - Segundo front-end

**Los recursos necesarios para esta práctica están en el siguiente** [**repositorio**](https://github.com/ivanmp-lm/IAW-Practica-4)\*\*\*\*

## 1. Creación y aprovisionamiento de la segunda máquina

Se creará una AMI con Ubuntu Server 20.04 de idénticas características que la de la anterior práctica. Esta vez se incorporarán al script líneas nuevas para así automatizar el proceso lo máximo posible, aún así, se aclarará su funcionamiento:

```text
#--CAMBIAR IP POR LA DE SERVER BACKEND--
IP_BACKEND=
#--CAMBIAR IP POR LA DE SERVER BACKEND--
```

Al principio del script estará la anterior variable, en ella se escribirá la dirección IP pública del servidor back-end. Es importante saber que **con cada reinicio, la IP cambiará**, así que en los próximos usos habrá que modificar manualmente esta IP en el archivo `/var/www/html/config.php`

## 2. Ajustes de phpMyAdmin

La siguiente parte del script contiene el método manual de instalación para phpMyAdmin, ya que el anterior necesitaba tener MySQL instalado en la máquina.

```text
#Instalar phpMyAdmin
rm -rf phpMyAdmin-latest-all-languages.tar.gz
wget https://www.phpmyadmin.net/downloads/phpMyAdmin-latest-all-languages.tar.gz
tar -xzvf phpMyAdmin-latest-all-languages.tar.gz
rm -rf phpMyAdmin-latest-all-languages.tar.gz
mv phpMyAdmin*/ /var/www/html/phpmyadmin
mv /var/www/html/phpmyadmin/config.sample.inc.php config.inc.php
#NECESARIO EDITAR LA LINEA SERVERS HOST CON LA IP DEL BACK-END!!
apt install php-mbstring php-zip php-gd php-json php-curl -y
phpenmod mbstring
systemctl restart apache2
```

Como se puede observar en el comentario de la línea número 8, será necesario editar el archivo `/var/www/html/phpmyadmin/config.inc.php` para que el parámetro server incluya la IP pública del back-end:

```text
$cfg['Servers'][$i]['host'] = 'IP-BACKEND'
```

Esto permitirá la conexión al servidor back-end desde phpMyAdmin:

![](../.gitbook/assets/1captura%20%281%29.png)

> Figura 1. Conexión al servidor MySQL desde phpMyAdmin

Tras estos pasos, el objetivo de la práctica estará completado.


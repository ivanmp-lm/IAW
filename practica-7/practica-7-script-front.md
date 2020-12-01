# Practica 7 - Script Front

```text
#!/bin/bash

#Mostrar comandos
set -x

#Clonar repositorio con todos los archivos necesarios
git clone https://github.com/ivanmp-lm/IAW-Practica-7.git
cd IAW-Practica-7/

#Actualizar repositorios
apt update

#Instalar nginx
apt install nginx -y

#Instalar módulos php
apt install php-fpm php-mysql -y

#Configuración de seguridad de php-fpm
sed -i "s/;cgi.fix_pathinfo=1/cgi.fix_pathinfo=0/" /etc/php/7.4/fpm/php.ini

#Reiniciar PHP para efectuar los cambios
systemctl restart php7.4-fpm

#Copiar archivo con la configuración de Nginx
cp default /etc/nginx/sites-available/

#Reiniciar servicio Nginx
systemctl restart nginx

#Copiar archivo info.php para comprobar la instalación
cp info.php /var/www/html/info.php

#Copiar archivo www.conf con la directiva listen 127.0.0.1:9000
cp www.conf /etc/php/7.4/fpm/pool.d/www.conf

#Reiniciar servicio php7.4-fpm para aplicar los cambios
systemctl restart php7.4-fpm

#Clonar repositorio de la aplicación propuesta
cd /var/www/html/
rm -rf iaw-practica-lamp
git clone https://github.com/josejuansanchez/iaw-practica-lamp
mv /var/www/html/iaw-practica-lamp/src/* /var/www/html/

#Eliminar archivos sobrantes
rm -rf iaw-practica-lamp

#Eliminar resto de archivos sobrantes
cd /home/ubuntu
rm -r IAW-Practica-7/
```


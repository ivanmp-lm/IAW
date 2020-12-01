# Práctica 7 - Script Balanceador

```text
#!/bin/bash

#--CAMBIAR LAS IP POR LAS DE LOS FRONT END--
FRONT1=
FRONT2=
#--CAMBIAR LAS IP POR LAS DE LOS FRONT END--

#Mostrar comandos
set -x

#Clonar repositorio con todos los archivos necesarios
git clone https://github.com/ivanmp-lm/IAW-Practica-7.git
cd IAW-Practica-7/

#Actualizar repositorios
apt update

#Instalar nginx
apt install nginx -y

#Copiar archivo con la configuración de Nginx
cp default-balancer /etc/nginx/sites-available/default

#Editar archivo default para incluir las IP de los Front-End
sed -i "s/IPFRONT1/$FRONT1/" /etc/nginx/sites-available/default
sed -i "s/IPFRONT2/$FRONT2/" /etc/nginx/sites-available/default

#Reiniciar servicio Nginx
systemctl restart nginx

#Eliminar archivos sobrantes
cd /home/ubuntu
rm -r IAW-Practica-7/
```


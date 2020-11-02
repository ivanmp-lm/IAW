# Práctica 3 - Script Back-End

```text
#!/bin/bash

#--VARIABLES PARA MYSQL--
DB_ROOT_PASSWD=root
#--VARIABLES PARA MYSQL--

#-----------------
#Instalación MySQL
#-----------------

#Instalar MySQL
apt install mysql-server -y

#Cambiar contraseña en MySQL
mysql -u root <<< "ALTER USER 'root'@'localhost' IDENTIFIED WITH caching_sha2_password BY '$DB_ROOT_PASSWD';"
mysql -u root <<< "FLUSH PRIVILEGES;"
```


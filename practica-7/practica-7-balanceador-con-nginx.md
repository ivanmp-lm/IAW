---
description: En esta práctica se configurará un balanceador de carga en un servidor NGINX
---

# Práctica 7 - Balanceador con NGINX

**Los recursos necesarios para esta práctica están en el siguiente** [**repositorio**](https://github.com/ivanmp-lm/IAW-Practica-7)\*\*\*\*

## **1. Creación y aprovisionamiento de las máquinas**

Se utilizarán 4 máquinas diferentes para esta práctica, el balanceador, dos front-end y un back-end.

El balanceador necesitará tener instalado el servidor NGINX el cual estará enlazado con ambos front-end, los cuales por su parte contarán también con el mismo servidor además de PHP. El back-end simplemente contará con un servidor MySQL como GBD.

* **Front-End x2**

Se crearán dos AMI con Ubuntu Server 20.04, el script será el mismo que el utilizado en la práctica 6 \(Pila LEMP\) pero eliminando la instalación de MySQL, tiene el nombre "**Script-Front.sh**".

* **Balanceador**

Necesaria AMI con Ubuntu Server 20.04. El balanceador requiere configuración adicional \(que está automatizada en el Script\) que se explicará en el siguiente punto para comprender el proceso, pero con la ejecución del script "**Script-Balanceador.sh**" estará operativo.

* **Back-End**

Exactamente igual que las tres máquinas anteriores, se lanzará un Ubuntu Server 20.04 y el script "**Script-Back.sh**" se encargará de instalar MySQL Server y dejarlo operativo.

## 2. Configuración del balanceador

Como se mencionó en el primer punto, el script automatiza el proceso que a continuación se describe, pero se explicará cómo se ha llegado a confeccionarlo.

Lo primero será tener las dos máquinas Front-End ya operativas, ya que se necesitará la dirección IP de cada una para configurar el balanceador. Con NGINX instalado, se editará el archivo `/etc/nginx/sites-available/default` para incluir toda la información del mismo, el resultado será algo como esto:

```text
upstream backend {
        least_conn;
        server IPFRONT1;
        server IPFRONT2;
    }

    server {
        location / {
            proxy_pass http://frontend/;
        }
}
```

Se sustituirán las IP por las de cada Front-End y se reiniciará el servidor con el comando:

```text
$ sudo systemctl restart nginx
```

## 3. Comprobaciones

En cada máquina Front-End se cambiará el archivo /var/www/index.php para que cada uno tenga un título distintivo, de esta forma se podrá saber si somos redireccionados a un servidor u otro por el balanceador.

![](../.gitbook/assets/image%20%289%29.png)

> **Figura 1.** Comprobación del balanceador de carga.


---
description: En esta práctica se añadirá un balanceador de carga a la estructura LAMP.
---

# Práctica 5 - Balanceador de carga

**Los recursos necesarios para esta práctica están en el siguiente** [**repositorio**](https://github.com/ivanmp-lm/IAW-Practica-5/tree/main)\*\*\*\*

## 1. Creación y aprovisionamiento de las máquinas

La estructura que se necesita para esta práctica consta de dos front-end con Apache2 y phpMyAdmin, un back-end con MySQL server y un balanceador de carga con Apache2. 

Las máquinas front-end y back-end se reutilizarán de las prácticas anteriores pero en el repositorio de la práctica se incluirán igualmente sus scripts si deben crearse de cero, este documento se centrará en la configuración del balanceador.

Para este balanceador se utilizará la misma AMI con Ubuntu Server 20.04.

El script del repositorio adjuntado, instalará Apache2, añadirá su archivo de configuración 000-default.conf y activará los módulos necesarios.

## 2. Configuración del balanceador de carga

Una vez se ejecute el script, se editará el archivo `/etc/apache2/sites-available/000-default.conf` para añadir el balanceador de carga. Las líneas que se añadirán dentro del apartado `<VirtualHost *:80>` serán algo similar a esto:

```text
    <Proxy balancer://mycluster>
        # Server 1
        BalancerMember http://IP-Server-1

        # Server 2
        BalancerMember http://IP-Server-2
    </Proxy>

    ProxyPass / balancer://mycluster/
```

Se sustuirá la parte `http://IP-Server-X` por la IP pública de cada front-end.

Una vez guardado el archivo, lo único que quedará por hacer es reiniciar el servicio de Apache2:

```text
$ sudo systemctl restart apache2
```

Tras esto, el balanceador de carga estará activo y funcionando.


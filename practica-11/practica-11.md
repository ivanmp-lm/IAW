---
description: >-
  En esta práctica se utilizará la utilidad WPScan para revisar las
  vulnerabilidades de la página web.
---

# Práctica 11 - WPScan

WPScan nos permitirá detectar todas las vulnerabilidades de nuestro Wordpress, escaneará la página web en busca de agujeros de seguridad que puedan ser utilizados de forma maliciosa y también nos avisará del software y componentes desactualizados del servidor.

Para empezar a utilizarlo se obtendrá un contenedor de docker con la utilidad instalada utilizando el siguiente comando:

```text
$ docker pull wpscanteam/wpscan
$ docker run -it --rm wpscanteam/wpscan --url http://DIRECCION-IP  --enumerate p
```

Donde "DIRECCION-IP" será la IP pública del servidor donde esté alojada la página a escanear.


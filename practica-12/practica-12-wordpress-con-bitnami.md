---
description: En esta práctica se instalará una AMI de Bitnami con Wordpress preinstalado.
---

# Práctica 12 - Wordpress con Bitnami

Bitnami es una empresa española que proporciona paquetes de software para una multitud de plataformas, incluida Wordpress, a estos paquetes se les llama "stack".

Para empezar a trabajar con Bitnami, se accederá a la página web con el [stack de Wordpress](https://bitnami.com/stack/wordpress/cloud/aws/amis) que cuenta con varias modalidades:

![](../.gitbook/assets/image%20%2813%29.png)

La que interesa para esta práctica es la modalidad en la nube "Single-Tier", bajo la plataforma de "Amazon Web Services", utilizando el enlace anterior se accederá a esta lista de AMIs. Al pulsar sobre la que se quiera utilizar, Bitnami nos redireccionará a la página de Amazon para crearla.

La AMI que se está utilizando en esta práctica utiliza Debian 10 con la versión de Wordpress 5.6-2. Cabe recordar que, como en las anteriores prácticas, se debe añadir un grupo de seguridad para los puertos 22 \(SSH\) 80 \(HTTP\) y 443 \(HTTPS\) para poder acceder a la página web y al propio servidor desde la máquina local.

Con la AMI activa, lo que se necesitará a continuación es la información de sesión que utilice Bitnami, para ello se entrará en el panel de administración EC2 en AWS y se seleccionará la instancia recién creada. A continuación se pulsará en "Acciones -&gt; Monitoreo y solución de problemas -&gt; Obtener registro del sistema":

![](../.gitbook/assets/image%20%2817%29.png)

Tal como muestra la imagen. Aparecerá una consola que deberá ser consultada y en una de sus líneas aparecerá lo siguiente:

![](../.gitbook/assets/image%20%2815%29.png)

Estas credenciales serán únicas para cada AMI y se generarán junto con su creación, con esto ya se podrá acceder al panel de administración de Wordpress:

![](../.gitbook/assets/image%20%2816%29.png)

Como último paso y para tener control total sobre nuestra AMI, necesitaremos tener acceso a phpMyAdmin, para lo cual será necesario crear un túnel SSH.

Para crear este túnel se lanzará el siguiente comando en Powershell o en Bash:

```text
ssh -N -L 8888:127.0.0.1:80 -i CLAVE.PEM bitnami@IPSERVIDOR
```

Se sustituirá "IPSERVIDOR" Y "CLAVE.PEM" por la IP pública de la AMI y la localización de nuestra clave de acceso SSH respectivamente. El resultado de este comando será que nuestra dirección localhost \(127.0.0.1\) será enlazada con el puerto 80 del servidor y redireccionada a nuestro puerto 8888.

![](../.gitbook/assets/image%20%2818%29.png)

Como puede verse en la captura, se accederá al panel utilizando nuestra dirección localhost seguido del puerto 8888. Las credenciales serán "root" y la clave anteriormente adquirida en la consola del registro del sistema de la AMI.


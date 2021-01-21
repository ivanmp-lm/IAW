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


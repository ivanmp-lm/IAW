---
description: >-
  En esta práctica se emplearán los conocimientos de la práctica 1 para
  trasladar nuestra pila a un servidor en la nube de Amazon.
---

# Práctica 2 - Instalación en AWS

## 1. Instalación de la AMI 

Una **AMI** \(Amazon Machine Image\) es una imagen que nos permite lanzar una máquina virtual en **AWS** \(Amazon Web Services\).

Para empezar a trabajar con ella, nos iremos a la consola AWS, donde nos aparecerá una serie de servicios que Amazon nos proporciona. El que necesitamos nosotros es **EC2** \(Elastic Compute Cloud\) que nos permitirá crear instancias \(máquinas virtuales\) a partir de estas AMI.

Al pulsar en EC2, entraremos en su panel de control, en la lista de la izquierda tendremos una opción llamada "**Instancias**", en ella podremos ver la lista de las que tengamos creadas y gestionarlas.

![Lista de instancias.](../.gitbook/assets/image.png)

Si pulsamos en el botón naranja llamado "**Lanzar instancias**" nos moveremos a la lista de AMIs disponible. En esta práctica, se utilizará **Ubuntu Server 20.04**, utilizando el buscador podremos afinar la lista según nuestros criterios.

![Lista de AMIs que corresponden al criterio &quot;Ubuntu&quot;.](../.gitbook/assets/image%20%281%29.png)

Al seleccionar la máquina que queramos, tendremos una larga lista con las características disponibles para la misma. Al estar usando una licencia de AWS Educate, sólo podremos elegir los tipos de instancia gratuitos, en este caso será t2.micro.

En el siguiente paso podremos configurar diversas opciones para adecuar un poco más la máquina a nuestras necesidades, por ejemplo, podemos especificar cuántas instancias queremos crear, el número de CPUs \(en el caso de que el tipo de nuestra AMI tenga más de un núcleo de CPU\) o si estará unida a un dominio, entre muchas otras.

![Configuraci&#xF3;n adicional de nuestra instancia.](../.gitbook/assets/image%20%282%29.png)


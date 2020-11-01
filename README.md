---
description: >-
  En este documento se detallará la instalación de una pila LAMP sobre una
  máquina virtual. Se ha realizado en formato Markdown utilizando la aplicación
  web https://app.gitbook.com
---

# Práctica 1 - Pila LAMP

## 1. Instalación de la pila LAMP

La pila LAMP consiste en un conjunto de software que se complementan para montar un servidor web, es ampliamente utilizada. El nombre viene del siguiente acrónimo:

* **Linux**
* **Apache**
* **MySQL**
* **PHP**

El conjunto de estas partes, formará la pila LAMP. Para empezar, pues, vamos a necesitar partir de un SO basado en Linux, y como en cualquier instalación Linux lanzaremos los paquetes básicos para actualizar y obtener nuevos repositorios:

```
$ sudo apt update
$ sudo apt upgrade
$ sudo apt full-upgrade
```

Con los repositorios actualizados, el siguiente paso sería instalar la herramienta **SSH**, que nos permite de forma cómoda **conectarnos remotamente a una instancia de bash y seguir lanzando comandos** desde cualquier equipo en la misma red.

```text
$ sudo apt install ssh
```

{% hint style="info" %}
Este paso es opcional, pero siempre es recomendable tener la posibilidad de acceder de forma remota a nuestra máquina, y será esencial usar esta herramienta cuando utilicemos servidores remotos.
{% endhint %}


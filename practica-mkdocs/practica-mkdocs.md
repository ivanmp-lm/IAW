---
description: >-
  En esta práctica se utilizará un contenedor docker de MKDocs para realizar una
  página que pueda subir su contenido a GitHub Pages
---

# Práctica MKDocs

Se creará una máquina virtual de forma local en VirtualBox para esta práctica.

Lo primero que se hará en la misma será crear una estructura de carpetas específica para el uso de MKDocs, el resultado sería algo así:

![](../.gitbook/assets/image%20%2853%29.png)

El archivo "mkdocs.yml" es similar al archivo de configuración de Jekyll, en su interior también escribiremos los parámetros necesarios para el funcionamiento y orden de la página:

```text
site_name: Iván MKDocs

nav:
    - Principal: home.md
    - Acerca de: about.md

theme: material
```

Como se indica en el archivo, el tema a utilizar tiene el nombre "material" y las páginas creadas anteriormente llamadas "index.md" y "about.md" están listadas en la página. No obstante, el archivo completo puede ser mucho más elaborado y complejo ya que pueden editarse multitud de opciones para el tema.

Para comenzar a probar la página, se ejecutará el siguiente comando partiendo desde la carpeta "proyecto":

```text
$ sudo docker run --rm -it -p 8000:8000 -v "$PWD":/docs squidfunk/mkdocs-material
```

La carpeta "docs" será enlazada como un volumen en la carpeta del contenedor relativa al tema de MKDocs. Sólo se podrá acceder de forma local a la página web:




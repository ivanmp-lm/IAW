---
description: >-
  En esta práctica se creará una página web utilizando Jekyll y se subirá a
  Github Pages.
---

# Práctica Jekyll

Para empezar será necesario utilizar una máquina \(ya sea una máquina virtual ejecutada localmente o en AWS\) con un sistema Linux para ejecutar docker cómodamente. Se utilizará un contenedor de Jekyll lanzando el siguiente comando:

```text
$ sudo docker run -it --rm -v "$PWD:/srv/jekyll" jekyll/jekyll jekyll new blog
```

Lo que hará es crear en la localización del usuario que ha ejecutado el comando una carpeta con todos los archivos necesarios para iniciar un nuevo proyecto con Jekyll. La carpeta creada con dicho contenido aparecerá bajo el nombre "blog".

Se ejecutarán a continuación los siguientes comandos:

```text
$ cd blog
$ docker run -it --rm -v "$PWD:/srv/jekyll" jekyll/jekyll jekyll build
```

Dentro de la carpeta recién creada para el proyecto, se generará una página web HTML para probar Jekyll.

Para testear la página se ejecutará el siguiente:

```text
$ sudo docker run -it --rm -p 4000:4000 -v "$PWD:/srv/jekyll" jekyll/jekyll jekyll serve --force_polling
```

Podrá accederse al contenido de la página a través del puerto 4000:

![](../.gitbook/assets/image%20%2851%29.png)

Para añadir esta página web en Github Pages, se deberá trasladar todo el contenido resultante en los pasos previos a un repositorio de github, después, en "ajustes" dentro del mismo repositorio habrá que moverse hasta encontrar la opción "Github Pages" que se configurará de la siguiente forma:

![](../.gitbook/assets/image%20%2852%29.png)

Al pulsar en "Choose a theme" aparecerá una selección de temas compatibles con Jekyll que se podrán usar para el blog:

![](../.gitbook/assets/image%20%2849%29.png)

Tras seleccionar el tema se guardarán los cambios. **NOTA: Algunos de estos temas parecen no ser compatibles con Jekyll.**

Tras añadir contenido al blog desde la carpeta "\_posts" utilizando el siguiente formato de nombre:

![](../.gitbook/assets/image%20%2848%29.png)

El resultado será algo como esto:

![](../.gitbook/assets/image%20%2850%29.png)


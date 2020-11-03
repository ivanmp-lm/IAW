---
description: >-
  En esta práctica se implementará una pila LAMP funcionando en dos servidores,
  uno actuando de front-end y otro back-end.
---

# Práctica 3 - LAMP en dos niveles

## 1. Creación y aprovisionamiento de las máquinas virtuales

Para empezar con la práctica, lanzaremos dos AMIs Ubuntu Server 20.04 en Amazon Web Services con las características básicas \(la tarifa gratuita para usuarios de AWS educate\). Debemos asegurarnos de que las máquinas tienen los puertos correspondientes abiertos, en el caso del backend necesitaremos el **puerto 3306** activado.

Una de ellas actuará como el **front-end**, que contendrá la mayor parte de la pila LAMP instalada \(Apache y PHP\) mientras que la otra será el **back-end** __contando solamente con MySQL.

Como en la práctica anterior, se aprovechará un script para tener las máquinas preparadas lo antes posible, cada máquina tendrá su propio script.

* [Práctica 3 - Script Front-End](practica-3-script-front-end.md)
* [Práctica 3 - Script Back-End](practica-3-script-back-end.md)

Nos conectamos a través de Visual Studio Code y ejecutamos el script en cada una.

## 2. Configuración de MySQL Server en la máquina back-end




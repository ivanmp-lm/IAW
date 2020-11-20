---
description: En esta práctica se añadirá un balanceador de carga a la estructura LAMP.
---

# Práctica 5 - Balanceador de carga

**Los recursos necesarios para esta práctica están en el siguiente** [**repositorio**](https://github.com/ivanmp-lm/IAW-Practica-5/tree/main)\*\*\*\*

## 1. Creación y aprovisionamiento de las máquinas

La estructura que se necesita para esta práctica consta de dos front-end con Apache2 y phpMyAdmin, un back-end con MySQL server y un balanceador de carga con Apache2. 

Las máquinas front-end y back-end se reutilizarán de las prácticas anteriores pero en el repositorio de la práctica se incluirán igualmente sus scripts si deben crearse de cero, este documento se centrará en la configuración del balanceador.

Para este balanceador se utilizará la misma AMI con Ubuntu Server 20.04.

## 2. Configuración del balanceador de carga




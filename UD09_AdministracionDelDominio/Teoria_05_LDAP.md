---
title: UD09 - Administración del dominio
author: modificación de José Manuel Alvarez a partir del material de Angel Berlanas Vicente
header-includes: |
lang: es-ES
keywords: [ASIR, ISO, Dominios]
---

# Servicio de Directorio

Para tratar de solventar los problemas que hemos planteado con anterioridad tenemos varios mecanismos, uno de los más extendidos es el uso de Directorios.

Un directorio no es más que una lista que recoge todos los recursos que están disponibles para una red y organización, sin importar si se encuentra en el servidor o no. Estos recursos se organizan de manera lógica, permitiendo a los usuarios las búsquedas en el directorio acerca de los diferentes recursos de los que dispone.

`x500` es el servicio de directorio más famoso, y `LDAP` es el estándar que está basado en `x500`, que está preparado para trabajar sobre TCP/IP.

https://es.wikipedia.org/wiki/X.500

https://en.wikipedia.org/wiki/X.500

Para los usuarios el concepto de directorio es cómodo, ellos se conectan a la red y pueden listar los recursos disponibles sin importar donde se encuentran.

# LDAP

`LDAP` (_Lightweight Directory Access Protocol_, Protocolo Ligero de Acceso a Directorios) es un protocolo que permite el acceso a un servicio de directorio ordenado y distribuido para buscar diversa información en un entorno de red. 

`LDAP`  a diferencia de una base de datos se encuentra especialmente optimizado para operaciones de lectura. No estaría optimizado para lectura y también para escritura como una base de datos.

Después de ver todo esto , podemos definir que :

> Un **Directorio** es un conjunto de objetos con atributos organizados en una manera lógica y jerárquica.

`LDAP` se apoya en el DNS (Nombres de Dominio) para estructurar y organizar los diferentes recursos:

https://es.wikipedia.org/wiki/Sistema_de_nombres_de_dominio


* Personas
* Unidades Organizativas (_OU_)
* Impresoras (_Printers_)
* Documentos
* Grupos de Personas (_Groups_)

Normalmente almacenamos la información de autenticación (_usuario y contraseña_) y es utilizado para autenticarse aunque es posible almacenar otra información.

Existen diversas implementaciones y aplicaciones reales del protocolo LDAP como pueden ser:

* Active Directory (Directorio Activo)
* Novell Directory Services
* IPLanet
* OpenLDAP
* Red Hat DS

La implementaciones que vamos a estudiar son la de _Active Directory_, utilizada por Microsoft, y la de _OpenLDAP_ que podremos
instalar en multitud de sistemas operativos y que cuenta con mucha aceptación.

## Ventajas de LDAP

Tal vez la mayor ventaja de LDAP es que permite acceder al directorio LDAP desde casi cualquier dispositivo, y desde muchas aplicaciones

Es también factible personalizar las aplicaciones internas de una empresa para añadirles soporte LDAP.

El protocolo LDAP es utilizable por distintas plataformas y basado en estándares, de ese modo las aplicaciones no necesitan 
preocuparse por el tipo de servidor en que se hospeda el directorio. De hecho, LDAP está encontrando mucha más amplia aceptación 
a causa de ese estatus como estandard de Internet. 

Un servidor LDAP puede ser cualquiera de un numero de los servidores de directorio LDAP de código abierto o comercial.

La mayoría de los servidores LDAP son simples de instalar, fácilmente mantenibles, y fácilmente optimizables.

Los servidores LDAP pueden replicar tanto algunos de sus datos como todos a través de métodos de envío o recepción,
lo que permite enviar datos a oficinas remotas, incrementar su seguridad y demás.

La tecnología de replicación está incorporada y es fácil de configurar.

LDAP permite delegar con seguridad la lectura y modificación basada en autorizaciones según tus necesidades utilizando ACLs.

LDAP es particularmente utilizable para almacenar información que se desee leer desde muchas localizaciones, pero que no sea actualizada frecuentemente.

# Dominios

Un dominio se puede entender como el conjunto de computadoras conectadas en una red informática que confían a uno de los equipos de dicha red, la administración de los usuarios y los privilegios que cada uno de los usuarios tiene en dicha red.

Los servidores de directorio LDAP almacenan sus datos jerárquicamente. Al igual que los árboles DNS descendientes o directorios de ficheros UNIX, 
así es la estructura de directorio LDAP. Como con los nombres de host en DNS, un registro Nombre Distinguido (Distinguished Name en ingles, DN en corto) de un directorio LDAP 
se lee desde su entrada individual, recursivamente a través del árbol, hasta el nivel más alto.

En los ejemplos que veremos trabajaremos con un dominio _base_ que denominaremos **iso.com**. 

Esta será nuestra raiz, de la que colgaremos las diferentes estructuras, unidades organizativas, utilizando una estructura jerárquica.

* _dc=iso, dc=com_

>Será el DN (_Distinguished Name_) base que utilizaremos.

Para que esto funcione de manera correcta necesitamos varios servicios funcionando de manera _coordinada_. 

* Necesitaremos un servicio de _directorio_, donde almacenaremos información acerca de los usuarios, grupos, permisos, etc. de nuestro dominio (**LDAP**). 
* Un servicio de resolución de nombres de máquinas, usuarios, que nos permita trabajar mediante nomenclatura _de Internet_. (**DNS**).
* Un servicio validación de usuarios de manera remota, que nos asegure de que los usuarios son _quienes dicen ser_. (**Kerberos**).
* Un sistema de compartición de recursos en red, como podría ser **SMB** (Server Message Block), pero podríamos utilizar otro.
* Un sistema de sincronización de la hora, que nos asegure que todas las máquinas de nuestro dominio están en la misma hora (requisito muy importante para Kerberos).(**NTP**).

Deberemos configurar toda esta infraestructura para el correcto funcionamiento de un dominio.

Haciendo un breve resumen de los pasos necesarios para configurar un servicio de ldap podemos citar los siguientes:

Estableceremos un ordenador como controlador del dominio, donde crearemos cuentas _globales_ que podrán ser utilizadas en todos los ordenadores que configuremos en la red.

Las cuentas locales de los ordenadores seguirán en cada una de las máquinas.

Debemos configurar tanto el ordenador servidor como los clientes. Uno para que sea el controlador y los clientes para que tenga _confianza_ y sean dominados por el servidor.

De alguna manera los servidores, dentro del dominio, anuncian sus servicios a los usuarios. Los usuarios obtienen acceso sin importar que ordenador del dominio está _ofreciendo_ el servicio.

Si la red se vuelve demasiado amplia y aparecen varios dominios, podemos establecer relaciones de confianza (_trust_) entre ellos. De tal manera que un usuario solo requerirá iniciar sesión en uno de ellos y los demás dominios confiarán en el dominio de conexión.



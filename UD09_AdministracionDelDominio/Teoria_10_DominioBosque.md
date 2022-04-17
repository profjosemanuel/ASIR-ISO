---
title: UD09 - Administración del dominio
author: modificación de José Manuel Alvarez a partir del material de Angel Berlanas Vicente
header-includes: |
lang: es-ES
keywords: [ASIR, ISO, Dominios]
---

# Dominios

Un dominio se puede entender como el conjunto de computadoras conectadas en una red informática que confían a uno de los equipos de dicha red, la administración de los usuarios y los privilegios que cada uno de los usuarios tiene en dicha red.

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

## Árbol de directorio LDAP

Los servidores de directorio LDAP almacenan sus datos jerárquicamente. Al igual que los árboles DNS descendientes o directorios de ficheros UNIX, 
así es la estructura de directorio LDAP. Como con los nombres de host en DNS, un registro Nombre Distinguido (Distinguished Name en ingles, DN en corto) de un directorio LDAP 
se lee desde su entrada individual, recursivamente a través del árbol, hasta el nivel más alto.

En los ejemplos que veremos trabajaremos con un dominio _base_ que denominaremos **iso.com**. 

Esta será nuestra raiz, de la que colgaremos las diferentes estructuras, unidades organizativas, utilizando una estructura jerárquica.

* _dc=iso, dc=com_

>Será el DN (_Distinguished Name_) base que utilizaremos.


## Estructura

Cada dominio contiene una serie de máquinas clientes, unos recursos y al menos un servidor que domina a los equipos clientes, este servidor se conoce como Controlador de Dominio (_Domain Controler, DC_).

Podemos agrupar varios dominios formando estructuras de dominios, donde cada uno de estos dominios cuenta con su propio controlador de dominio. Esta agrupación de dominios se realiza de forma anidada, al igual que hemos visto en la estructura de directorios. Cada dominio puede tener dominios padres y dominios hijos, y todos los dominios tienen un dominio padre menos el dominio raíz, que es el primero de todos.

Esta estructura de dominios anidados se conoce como **árbol**.

# Bosque

A lo largo de las prácticas trabajaremos con diferentes dominios, en Windows veremos el dominio directo de _iso.com_, mientras que en GNU/LinuX veremos el subdominio de _valencia.iso.com_. 
Las relaciones de confianza y la administración avanzada del dominio se verán en mayor profundidad en el módulo de ASO (Administración de Sistemas Operativos) perteneciente al segundo curso del Ciclo Formativo.
En ASO se continúan trabajando aspectos relacionados con la administración del dominio.

![Bosque](AD2016/ISO_AD_Arboles.png)
\ 

Vemos como el dominio raíz tiene dos dominios hijos (Valencia y Castellón), y uno de ellos tiene a su vez dos hijos más (Utiel y Torrent). Al hablar de árboles, podemos decir que el dominio raíz tiene dos ramas, y una de esas ramas tiene a su vez dos ramas más. Es posible crear una estructura que cuente con más de un árbol, estas estructuras de carácter superior al árbol se conocen como bosque.

El modelo de bosque permite a las organizaciones que no forman un espacio de nombres contiguo, o jerarquizado, mantener la continuidad de toda la organización en su estructura de dominios agregados.

## Cómo organizar los datos

Debajo de tu base de directorio, crearemos contenedores que separen lógicamente los datos.
Por razones históricas (_X.500_), la mayoría de los directorios configuran estas separaciones lógicas como entradas OU. 

`OU` vienen de _Unidades Organizacionales_ (Organizational Units, en inglés), que en X.500 eran utilizadas para indicar 
la organización funcional dentro de la empresa: ventas, finanzas, etcétera. 

Actualmente las implementaciones de LDAP han mantenido la convención del nombre `ou=`, pero separa las cosas por categorías 
amplias como `ou=gente` (`ou=people`), `ou=grupos` (`ou=groups`), `ou=dispositivos` (`ou=devices`), y demás. 

Los niveles inferiores de OUs son utilizados a veces para separar categorías por debajo. 

Por ejemplo, un árbol de directorio LDAP (sin incluir entradas individuales) podría parecerse a esto:

```shell
    dc=valencia,dc=iso, dc=com 
        ou=esbirros
            ou=tecnicos
            ou=transportistas
        ou=dispositivos
        ou=grupos

```

De esta manera un esbirro que crearamos dentro la `ou=tecnicos` tendría un DN como este:

`uid=esbirro01,ou=tecnicos,ou=esbirros,dc=iso,dc=com`

y un nombre en el formato DNS como este:

`esbirro01.tecnicos.esbirros.iso.com`

A lo largo de la unidad veremos como crear estas estructuras en los diferentes LDAPs. En la tarea de instalación y configuración del OpenLDAP se crea un usuario, y este al final tienen una serie de atributos que nos permiten trabajar con él.

![OpenLDAP](OpenLDAP/Slapd31.png)
\

Esta captura ha sido obtenida tras la ejecución del comando siguiente en la máquina en la que realizaremos.

`ldapsearch -x -h localhost -p 389 -b "dc=valencia,dc=iso,dc=com"`

Vamos ahora a describir los atributos más importantes que definen a este usuario en el LDAP:

* **dn** (Distinguised Name) consta del nombre único que define a este usuario y lo situa en una rama determinada de la estructura del LDAP. Contiene toda la ruta del LDAP.
* **cn** (Common Name) Nombre del Usuario + Apellidos
* **givenName** : Nombre del usuario
* **gidNumber** Grupo principal al que pertence el usuario.
* **homeDirectory** : _RUTA_ a la carpeta personal del usuario.
* **sn**: Apellidos del usuario
* **loginShell** : Intérprete de comandos por defecto para el usuario.
* **objectclass** : Es una colección de _atributos_ que se aplican al objeto (en este caso usuario). Por ejemplo, en este caso contamos que los atributos que se aplican son:
  * `inetOrgPerson` -> Persona de la organización
  * `posixAccount` -> Indica que es una cuenta de usuario que sigue el estándar POSIX.
  * `top` -> Todos los objetos del LDAP heredan de `top` (es la clase _padre_)
* **uidNumber** : Identificador numérico del usuario.
* **uid**: Identificador (_login_) del usuario.

Con todos estos datos establecidos, en esa base de datos remota a la que accedemos mediante LDAP, podemos realizar operaciones de usuario (login, asignar permisos, etc) con los usuarios y grupos de LDAP como si de usuarios locales se tratase. 

De esta manera tenemos una gestión _centralizada_ de los usuarios en nuestra red.



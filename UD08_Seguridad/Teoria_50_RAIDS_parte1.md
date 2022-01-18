---
title: UD08 - Seguridad
author: Angel Berlanas Vicente
header-includes: |
lang: es-ES
keywords: [ASIR, ISO, Seguridad]
---

\newpage

# RAID

## ¿Qué es un RAID?

Un grupo/matriz redundante de discos independientes1​ (también, RAID, del inglés _redundant array of independent disks_) hace referencia a un sistema de almacenamiento de datos que utiliza múltiples unidades (discos duros o SSD), entre las cuales se distribuyen o replican los datos.

Dependiendo de su configuración (a la que suele llamarse nivel), los beneficios de un RAID respecto a un único disco son uno o varios de los siguientes: mayor integridad, tolerancia frente a fallos, tasa de transferencia y capacidad. En sus implementaciones originales, su ventaja clave era la habilidad de combinar varios dispositivos de bajo coste y tecnología más vieja en un conjunto que ofrecía mayor capacidad, fiabilidad, velocidad o una combinación de éstas que un solo dispositivo de última generación y coste más alto.

En el nivel más simple, un RAID combina varios discos duros en una sola unidad lógica. Así, en lugar de ver varios discos duros diferentes, el sistema operativo ve uno solo. Los RAID suelen usarse en servidores y normalmente (aunque no es necesario) se implementan con unidades de disco de la misma capacidad. Debido al descenso en el precio de los discos duros y la mayor disponibilidad de las opciones RAID incluidas en los chipsets de las placas base, los RAID se encuentran también como opción en las computadoras personales más avanzadas. Esto es especialmente frecuente en las computadoras dedicadas a tareas intensivas y que requiera asegurar la integridad de los datos en caso de fallo del sistema. Esta característica está disponible en los sistemas RAID por hardware (dependiendo de que estructura elijamos). Por el contrario, los sistemas basados en software son mucho más flexibles y los basados en hardware añaden un punto de fallo más al sistema (la controladora RAID).

>Fuente: _Wikipedia_

Se trata de un mecanismo que aporta una nueva capa de abstracción entre las aplicaciones y el lugar donde van a ser almacenados los datos. Es importante que tengamos esto presente, ya que nos permitirá luego poder administrar de manera más precisa aquellos servicios que utilicemos para este propósito.

Añadir un RAID por software añade complejidad a nuestro Sistema Operativo, ya que tiene que encargarse él, pero prescindimos de la controladora RAID por Hardware, que nos aporta otro punto de fallo. Si bien es verdad que en servidores y sistemas de alto rendimiento, son las controladoras hardware las que suelen realizar esta tarea.

Sin embargo si existe algún problema, en los `RAIDs` por Software el proceso es independiente de la placa o controladora, al usarse discos llamados _Dinámicos_. Para recuperar un RAID en otra instalación, placa o controladora de Windows o GNU/LinuX, solo tendrás que 'Importar' el sistema de discos, lo cual lo hace mucho mas versátil.

## Tipos de RAID

Al igual que hemos visto con los sistemas de ficheros, existen multitud de opciones que resuelven configuraciones de RAID diferentes, vamos a ver aquí las más habituales y en las prácticas trabajaremos con ellas.

Cada una de estas configuraciones tiene una serie de características que la hacen más o menos adecuada para la resolución de un problema.

### Volumen distribuido

Se trata de la asociación de varios discos en una única partición lógica, el resultado de la capacidad es la suma de las capacidades de los discos que la forman, sin embargo no tenemos control sobre donde se almacenan exactamente los datos, pudiendo quedar datos en un disco solamente, o en ambos.

### RAID 0

También conocido como volumen divido o seccionado, se trata de una configuración en la que los datos se guardan en todos los discos a la vez. Obteniendo la suma del espacio de todos los discos en caso de que sean del mismo tamaño. Si **No** fueran del mismo tamaño obtendríamos la capacidad del disco más pequeño por cada uno de los discos. Ejemplos:

* Si 2 discos son iguales en tamaño, se aprovecha el 100% de cada disco.

2 HDD de 1TB -> Reales 930GBs -> RAID 0 -> 930GB + 930GB -> 1860 GB

* Si un disco es mayor que el otro, se usará el 100% del disco menor y el % correspondiente del grande hasta igualar el espacio, quedando el resto del disco inutilizado.

1 HDD de 500GB (reales 465GB) y 1 HDD de 1TB (reales 930GB) -> se usara el 100% del disco de 465GB y el 50% del disco de 930GB, resultando en una partición de 465GB , y quedando los otros 465GB sin uso

![RAID 0](CreacionDeRaidyVolumenes/Raid0.png)
\

En esta configuración los datos se escriben en _paralelo_ pero a la velocidad del más lento. Si 2 HDD son iguales, siempre hay una minima diferencia, que no se va a notar, pero si se usan 2 discos distintos (en velocidad, independientemente del espacio) el disco mas rápido trabajará 'esclavo' de la velocidad del disco de menor velocidad, dando como resultado un rendimiento mejor que si fuese un solo disco siempre que contemos con la velocidad del mas lento.

 * Si un HDD 'A' permite hasta 100MB/s de escritura, y un HDD 'B' permite 140MB/s, la tasa de escritura combinada seria (teoricamente) de 200MB/s, aprovechando los 100MB/s del 'A' y hasta 100MB/s del 'B'.

### RAID 1

También conocido como volumen reflejado. Los datos se escriben en ambos discos obteniendo una mejora en el tiempo de lectura (ya que podemos leer de ambos a la vez) pero no de la escritura, ya que deberemos escribir los datos en ambos discos.

Si uno de los discos falla, el sistema avisa, pero podemos seguir trabajando con los datos. Un RAID 1 no es una copia de seguridad, pero si mejora el UPTIME de los servicios, ya que nos permite trabajar con datos durante más tiempo.


Esto quiere decir que si falla un disco en un servidor web apache por ejemplo podemos esperar bastante tiempo a apagar el servidor. El servicio web se puede seguir dando hasta que se encuentre el momento oportuno para apagar el servidor y cambiar el disco. 

![RAID 1](CreacionDeRaidyVolumenes/Raid1.png)
\ 


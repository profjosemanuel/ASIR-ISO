---
title: UD08 - Seguridad
author: Angel Berlanas Vicente
header-includes: |
lang: es-ES
keywords: [ASIR, ISO, Seguridad]
---



### RAID 5

Un RAID 5 (también llamado distribuido con paridad) es una división de datos a nivel de bloques que distribuye la información de paridad entre todos los discos miembros del conjunto. El RAID 5 ha logrado popularidad gracias a su bajo coste de redundancia. Generalmente, el RAID 5 se implementa con soporte hardware para el cálculo de la paridad. RAID 5 necesitará un mínimo de 3 discos para ser implementado. Guardando la paridad de ambos discos en el tercero. Esta paridad se calcula de muchas maneras, pero una de las más famosas es la XOR o disyunción exclusiva:

La disyunción exclusiva discriminaría entre pareja de valores booleanos. No importaría tanto si son ciertos o falso como si los dos valores son iguales o diferentes

Ante una pareja de valores iguales el resultado de operar la disyunción exclusiva a esos valores daría por resultado falso
Ante una pareja de valores diferentes el resultaado de operar la disyunción exclusiva a esos valores daría por resultado cierto.


https://es.wikipedia.org/wiki/Disyunci%C3%B3n_exclusiva

![RAID 5](CreacionDeRaidyVolumenes/Raid5.png)
\ 

Ejemplo:

Si quisiéramos guardar la siguiente secuencia de bits en un RAID 5, suponiendo que cada byte va separado obtendríamos lo siguiente:

Datos: 0001, 0111, 1111, 1110, 1011, 0000.

| DATOS D1 | DATOS D2 | DATOS D3 (paridad) |
| ------- | ------- |-------------------|
| 0001    | 0111    | 0110 |
| 1111    | 1110    | 0001 |
| 1011    | 0000    |1011|

De esta manera en caso de que se dañara cualquiera de los discos podríamos reconstruirlos a partir de los almacenados en los otros dos discos.

### RAID 0+1

Un RAID 0+1 (o 01) es un RAID usado para replicar y compartir datos entre varios discos. La diferencia entre un RAID 0+1 y un RAID 1+0 es la localización de cada nivel RAID dentro del conjunto final: un RAID 0+1 es un espejo de divisiones.

![RAID 0+1](CreacionDeRaidyVolumenes/Raid0mas1.png)
\ 

Primero se crean dos  RAID 0 (dividiendo los datos en discos) y luego, sobre los anteriores, se crea un conjunto RAID 1 (realizando un espejo de los anteriores).

### RAID 1+0

Un RAID 1+0, a veces llamado RAID 10, es parecido a un RAID 0+1 pero con los niveles RAID en el orden inverso:
 el RAID 10 es una división de espejo

![RAID 1+0](CreacionDeRaidyVolumenes/Raid10.png)
\ 

¿Ya que tenemos cuatro discos porqué no usamos raid 5?
El problema de esta configuración es que si un disco falla y no se cambia rápidamente, el restante de la división pasa a ser el único punto de fallo y si se pierde no serán recuperables los datos.

Esta configuración es habitual el bases de datos de alto rendimiento, ya que no incluye coste de cálculo de CRC (paridad). 


### Otros tipos de RAID

Existen más tipos de RAID, pero se trata de asociaciones y configuraciones de los vistos hasta ahora, si los conceptos están aprendidos, la comprensión y puesta en marcha de los otros tipos de RAID están a nuestro alcance.


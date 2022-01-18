lang: es-ES
keywords: [ASIR, ISO, Seguridad]
---


## ¿Que nos proporciona un RAID?

### Un RAID puede mejorar el uptime

Los niveles RAID 1, 0+1 o 10, 5 y otros niveles permiten que un disco falle mecánicamente y que aun así los datos del conjunto sigan siendo accesibles para los usuarios. Un RAID permite que los datos se recuperen en un disco de reemplazo a partir de los restantes discos del conjunto, mientras al mismo tiempo permanece disponible para los usuarios en un modo degradado. Esto es muy valorado por las empresas, ya que el tiempo de no disponibilidad suele tener graves repercusiones.

### RAID puede mejorar el rendimiento de ciertas aplicaciones

Los niveles RAID 0, 5 y 6 usan variantes de división (stripping) de datos, lo que permite que varios discos atiendan simultáneamente las operaciones de lectura lineales, aumentando la tasa de transferencia sostenida. Las aplicaciones de escritorio que trabajan con archivos grandes, como la edición de vídeo e imágenes, se benefician de esta mejora. Las lecturas simultáneas también se ven beneficiadas por estructuras de RAID, ya que se pueden leer de varios discos a la vez.

## Lo que RAID NO puede hacer

### RAID no protege los datos

Un conjunto RAID tiene un sistema de archivos, lo que supone un punto único de fallo al ser vulnerable a una amplia variedad de riesgos aparte del fallo físico de disco, por lo que RAID no evita la pérdida de datos por estas causas. RAID no impedirá que un virus destruya los datos, que éstos se corrompan, que sufran la modificación o borrado accidental por parte del usuario ni que un fallo físico en otro componente del sistema afecten a los datos.
  
### RAID no simplifica la recuperación de un desastre

Cuando se trabaja con un solo disco, éste es accesible normalmente mediante un controlador ATA o SCSI incluido en la mayoría de los sistemas operativos. Sin embargo, las controladoras RAID necesitan controladores software específicos. Las herramientas de recuperación que trabajan con discos simples en controladoras genéricas necesitarán controladores especiales para acceder a los datos de los conjuntos RAID. Si estas herramientas no los soportan, los datos serán inaccesibles para ellas.

### RAID no mejora el rendimiento de todas las aplicaciones

Esto resulta especialmente cierto en las configuraciones típicas de escritorio. La mayoría de aplicaciones de escritorio y videojuegos hacen énfasis en la estrategia de _buffering_ y los tiempos de búsqueda de los discos. Una mayor tasa de transferencia **sostenida** supone poco beneficio para los usuarios de estas aplicaciones, al ser la mayoría de los archivos a los que se accede muy pequeños. Para estos usos, lo mejor es comprar un disco más grande y rápido, en lugar de dos discos más lentos y pequeños en una configuración RAID 0.

### RAID no facilita el traslado a un sistema nuevo

Cuando se usa un solo disco, es relativamente fácil trasladar el disco a un sistema nuevo: basta con conectarlo, si cuenta con la misma interfaz. Con un RAID no es tan sencillo: si tenemos una controladora Hardware la BIOS RAID debe ser capaz de leer los _metadatos_ de los miembros del conjunto para reconocerlo adecuadamente y hacerlo disponible al sistema operativo. Esta limitación puede obviarse con el uso de RAID por software, que a su vez añaden otras diferentes (especialmente relacionadas con el rendimiento).

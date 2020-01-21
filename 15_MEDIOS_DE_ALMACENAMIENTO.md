# 15 MEDIOS DE ALMACENAMIENTO

En capítulos anteriores, analizamos la manipulación de datos a nivel de archivo. En este capítulo, consideraremos los datos a nivel de dispositivo. Linux tiene capacidades sorprendentes para manejar dispositivos de almacenamiento, ya sea almacenamiento físico como discos duros, almacenamiento en red o dispositivos de almacenamiento virtual como RAID (matriz redundante de discos independientes) y LVM (administrador de volumen lógico).

Sin embargo, debido a que este no es un libro sobre administración de sistemas, no trataremos de cubrir todo este tema en profundidad. Lo que intentaremos hacer es presentar algunos de los conceptos y comandos clave que se utilizan para administrar dispositivos de almacenamiento.

Para llevar a cabo los ejercicios en este capítulo, utilizaremos una unidad flash USB y un disco CD-RW (para sistemas equipados con una grabadora de CD-ROM).

Veremos los siguientes comandos:

**`mount`** Montar un sistema de archivos

**`umount`** Desmontar un sistema de archivos

**`fsck`** Verificar y reparar un sistema de archivos

**`fdisk`** Manipular tabla de particiones de disco

**`mkfs`** Crear un sistema de archivos

**`dd`** Convertir y copiar un archivo

**`genisoimage (mkisofs)`** Cree un archivo de imagen ISO 9660

**`wodim (cdrecord)`** Escribir datos en medios de almacenamiento óptico

**`md5sum`** Calcular una suma de comprobación MD5

## MONTAJE Y DESMONTAJE DE DISPOSITIVOS DE ALMACENAMIENTO

Los avances recientes en el escritorio de Linux han hecho que la administración de dispositivos de almacenamiento sea extremadamente fácil para los usuarios de escritorio. En su mayor parte, conectamos un dispositivo a nuestro sistema y "simplemente funciona". En los viejos tiempos (por ejemplo, 2004), estas cosas tenían que hacerse manualmente. En sistemas que no son de escritorio (es decir, servidores), este sigue siendo un procedimiento en gran parte manual, ya que los servidores a menudo tienen necesidades de almacenamiento extremas y requisitos de configuración complejos.

El primer paso para administrar un dispositivo de almacenamiento es conectar el dispositivo al árbol del sistema de archivos (file system tree). Este proceso, llamado *mounting* (montaje) , permite que el dispositivo interactúe con el sistema operativo. Como recordamos del Capítulo 2, los sistemas operativos tipo Unix (como Linux) mantienen un único árbol de sistema de archivos con dispositivos conectados en varios puntos. Esto contrasta con otros sistemas operativos como Windows que mantienen árboles de sistema de archivos separados para cada dispositivo (por ejemplo, `C:\`, `D:\`, etc.).

Un archivo llamado `/etc/fstab` (abreviatura de "file system table" (tabla del sistema de archivos) enumera los dispositivos (generalmente las particiones del disco duro) que se deben montar en el momento del arranque. Aquí hay un ejemplo de archivo `/etc/fstab` de un sistema anterior de Fedora:

```sh
LABEL=/12         /                       ext4    defaults        1 1
LABEL=/home       /home                   ext4    defaults        1 2
LABEL=/boot       /boot                   ext4    defaults        1 2
tmpfs             /dev/shm                tmpfs   defaults        0 0
devpts            /dev/pts                devpts  gid=5,mode=620  0 0
sysfs             /sys                    sysfs   defaults        0 0
proc              /proc                   proc    defaults        0 0
LABEL=SWAP-sda3   swap                    swap    defaults        0 0
```

La mayoría de los sistemas de archivos enumerados en este archivo de ejemplo son virtuales y no son aplicables a nuestra discusión. Para nuestros propósitos, los interesantes son los primeros tres.

```sh
LABEL=/12         /                       ext4    defaults        1 1
LABEL=/home       /home                   ext4    defaults        1 2
LABEL=/boot       /boot                   ext4    defaults        1 2
```
Estas son las particiones del disco duro. Cada línea del archivo consta de seis campos, como se describe en la Tabla 15-1 .

**Tabla 15-1**: Campos `/etc/fstab` 

Contenido Campo | Descripción
----------------|------------
1 Device        | Tradicionalmente, este campo contiene el nombre real de un archivo de dispositivo asociado con el dispositivo físico, como `/dev/sda1` (la primera partición del primer disco duro detectado). Pero con las computadoras actuales, que tienen muchos dispositivos que se pueden conectar en caliente (como unidades USB), muchas distribuciones modernas de Linux asocian un dispositivo con una etiqueta de texto. Esta etiqueta (que se agrega al medio de almacenamiento cuando se formatea) puede ser una etiqueta de texto simple o un UUID (Universally Unique Identifier (Identificador único universal)) generado de forma aleatoria. El sistema operativo lee esta etiqueta cuando el dispositivo está conectado al sistema. De esa manera, no importa qué archivo de dispositivo esté asignado al dispositivo físico real, todavía puede identificarse correctamente.
2 Mount point | El directorio donde está conectado el dispositivo al file system tree.
3 File system type | Linux permite montar muchos tipos de sistemas de archivos. La mayoría de los sistemas de archivos Linux nativos son el Fourth Extended File System (`ext4`), pero muchos otros son compatibles, como FAT16 (`msdos`), FAT32 (`vfat`), NTFS (`ntfs`), CD-ROM (`iso9660`), etc.
4 Options | Los sistemas de archivos se pueden montar con varias opciones. Es posible, por ejemplo, montar sistemas de archivos como de solo lectura o evitar que se ejecuten programas desde ellos (una característica de seguridad útil para medios extraíbles).
5 Frequency | Un número único que especifica si se debe hacer una copia de seguridad de un sistema de archivos con el comando `dump`.
6 Order | Un número único que especifica en qué orden deben verificarse los sistemas de archivos con el comando `fsck`.

### Ver una Lista de los Sistemas de Archivos Montados (Mounted File Systems)

El comando `mount` se usa para montar file systems (sistemas de archivos). Al ingresar el comando sin argumentos, se mostrará una lista de los sistemas de archivos actualmente montados.

```sh
[me@linuxbox ~]$ mount
/dev/sda2 on / type ext4 (rw)
proc on /proc type proc (rw)
sysfs on /sys type sysfs (rw)
devpts on /dev/pts type devpts (rw,gid=5,mode=620)
/dev/sda5 on /home type ext4 (rw)
/dev/sda1 on /boot type ext4 (rw)
tmpfs on /dev/shm type tmpfs (rw)
none on /proc/sys/fs/binfmt_misc type binfmt_misc (rw)
sunrpc on /var/lib/nfs/rpc_pipefs type rpc_pipefs (rw)
fusectl on /sys/fs/fuse/connections type fusectl (rw)
/dev/sdd1 on /media/disk type vfat (rw,nosuid,nodev,noatime,
uhelper=hal,uid=500,utf8,shortname=lower)
twin4:/musicbox on /misc/musicbox type nfs4 (rw,addr=192.168.1.4)
```
El formato de la lista es el siguiente: `device on mount_point type filesystem_type (options)`. Por ejemplo, la primera línea muestra que el dispositivo `/dev/sda2` está montado como el sistema de archivos raíz, es del tipo ext4 y es readable y writable (la opción rw). Este listado también tiene dos entradas interesantes al final de la lista. La penúltima entrada muestra una tarjeta de memoria SD de 2GB en un lector de tarjetas montado en `/media/disk`, y la última entrada es una unidad de red montada en `/misc/musicbox`.

Para nuestro primer experimento, trabajaremos con un CD-ROM. Primero, veamos un sistema antes de insertar un CD-ROM.

```sh
[me@linuxbox ~]$ mount
/dev/mapper/VolGroup00-LogVol00 on / type ext4 (rw)
proc on /proc type proc (rw)
sysfs on /sys type sysfs (rw)
devpts on /dev/pts type devpts (rw,gid=5,mode=620)
/dev/sda1 on /boot type ext4 (rw)
tmpfs on /dev/shm type tmpfs (rw)
none on /proc/sys/fs/binfmt_misc type binfmt_misc (rw)
sunrpc on /var/lib/nfs/rpc_pipefs type rpc_pipefs (rw)
```
Este listado es de un sistema CentOS, que está utilizando LVM (Logical Volume Manager) para crear su sistema de archivos raíz. Al igual que muchas distribuciones modernas de Linux, este sistema intentará montar automáticamente el CD-ROM después de la inserción. Después de insertar el disco, vemos lo siguiente:
```sh
[me@linuxbox ~]$ mount
/dev/mapper/VolGroup00-LogVol00 on / type ext4 (rw)
proc on /proc type proc (rw)
sysfs on /sys type sysfs (rw)
devpts on /dev/pts type devpts (rw,gid=5,mode=620)
/dev/hda1 on /boot type ext4 (rw)
tmpfs on /dev/shm type tmpfs (rw)
none on /proc/sys/fs/binfmt_misc type binfmt_misc (rw)
sunrpc on /var/lib/nfs/rpc_pipefs type rpc_pipefs (rw)
/dev/sdc on /media/live-1.0.10-8 type iso9660 (ro,noexec,nosuid,nodev,uid=500)
```
Después de insertar el disco, vemos la misma lista que antes con una entrada adicional. Al final de la lista, vemos que el CD-ROM (que es dispositivo `/dev/sdc` en este sistema) se ha montado en `/media/live-1.0.10-8` y es del tipo `iso9660` (un CD-ROM). Para los fines de nuestro experimento, estamos interesados en el nombre del dispositivo. Cuando realice este experimento usted mismo, el nombre del dispositivo probablemente será diferente.

##### ADVERTENCIA

*En los ejemplos que siguen, es de vital importancia que preste mucha atención a los nombres reales de los dispositivos en uso en su sistema y no use los nombres utilizados en este texto. También tenga en cuenta que los CD de audio no son lo mismo que los CD-ROM. Los CD de audio no contienen sistemas de archivos y, por lo tanto, no se pueden montar en el sentido habitual.*

Ahora que tenemos el nombre del dispositivo de la unidad de CD-ROM, desmontemos el disco y lo volvamos a montar en otra ubicación en el árbol del sistema de archivos. Para hacer esto, nos convertimos en el superusuario (usando el comando apropiado para nuestro sistema) y desmontamos el disco con el comando `umount` (observe la ortografía).

```sh
[me@linuxbox ~]$ su -
Password:
[root@linuxbox ~]# umount /dev/sdc
```
El siguiente paso es crear un nuevo *punto de montaje* (mount point) para el disco. Un punto de montaje es simplemente un directorio en algún lugar del árbol del sistema de archivos. No tiene nada de especial. Ni siquiera tiene que ser un directorio vacío, aunque si monta un dispositivo en un directorio no vacío, no podrá ver los contenidos anteriores del directorio hasta que desmonte el dispositivo. Para nuestros propósitos, crearemos un nuevo directorio.

```sh
[root@linuxbox ~]# mkdir /mnt/cdrom
```

Finalmente, montamos el CD-ROM en el nuevo punto de montaje. La opción `-t` se usa para especificar el tipo de sistema de archivos.

```sh
[root@linuxbox ~]# mount -t iso9660 /dev/sdc /mnt/cdrom
```
Luego, podemos examinar el contenido del CD-ROM a través del nuevo punto de montaje.

```sh
[root@linuxbox ~]# cd /mnt/cdrom
[root@linuxbox cdrom]# ls
```
Observe lo que sucede cuando intentamos desmontar el CD-ROM.
```sh
[root@linuxbox cdrom]# umount /dev/sdc
umount: /mnt/cdrom: device is busy
```
¿Por qué es esto? La razón es que no podemos desmontar un dispositivo si alguien o algún proceso está utilizando el dispositivo. En este caso, cambiamos nuestro directorio de trabajo al punto de montaje para el CD-ROM, lo que hace que el dispositivo esté ocupado. Podemos remediar fácilmente el problema cambiando el directorio de trabajo a otro que no sea el punto de montaje.
```sh
[root@linuxbox cdrom]# cd
[root@linuxbox ~]# umount /dev/hdc
```
Ahora el dispositivo se desmonta con éxito.

##### POR QUÉ EL DESMONTAJE ES IMPORTANTE

Si observa el resultado del comando `free`, que muestra estadísticas sobre el uso de la memoria, verá una estadística llamada `buffers`. Los sistemas informáticos están diseñados para funcionar lo más rápido posible. Uno de los impedimentos para la velocidad del sistema son los dispositivos lentos. Las impresoras son un buen ejemplo. Incluso la impresora más rápida es extremadamente lenta según los estándares de la computadora. Una computadora sería muy lenta si tuviera que detenerse y esperar a que una impresora termine de imprimir una página. En los primeros días de las PC (antes de la multitarea), este era un problema real. Si estaba trabajando en una hoja de cálculo o documento de texto, la computadora se detendría y no estaría disponible cada vez que imprima. La computadora enviaba los datos a la impresora tan rápido como la impresora podía aceptarlos, pero era muy lenta porque las impresoras no imprimen muy rápido. Este problema se resolvió con el advenimiento del búfer de la impresora, un dispositivo que contiene memoria RAM que se ubicaría entre la computadora y la impresora. Con el búfer de la impresora en su lugar, la computadora enviaría la salida de la impresora al búfer, y se almacenaría rápidamente en la RAM rápida para que la computadora pudiera volver al trabajo sin esperar. Mientras tanto, el buffer de la impresora sería poco a poco poner en cola los datos a la impresora de la memoria del búfer a la velocidad a la que la impresora podría aceptarlo.

Esta idea de almacenamiento en búfer se usa ampliamente en las computadoras para hacerlas más rápidas. No permita que la necesidad de leer o escribir datos ocasionalmente desde o hacia dispositivos lentos impida la velocidad del sistema. Los sistemas operativos almacenan los datos que se han leído y se escribirán en los dispositivos de almacenamiento en la memoria durante el mayor tiempo posible antes de tener que interactuar realmente con el dispositivo más lento. En un sistema Linux, por ejemplo, notará que el sistema parece llenar memoria mientras más tiempo se usa. Esto no significa que Linux esté "usando" toda la memoria; significa que Linux está aprovechando toda la memoria disponible para hacer tanto almacenamiento en búfer como sea posible.

Este almacenamiento en búfer permite que la escritura en dispositivos de almacenamiento se realice muy rápidamente porque la escritura en el dispositivo físico se aplaza para un tiempo futuro. Mientras tanto, los datos destinados al dispositivo se acumulan en la memoria. De vez en cuando, el sistema operativo escribirá estos datos en el dispositivo físico.

Desmontar un dispositivo implica escribir todos los datos restantes en el dispositivo para que pueda eliminarse de forma segura. Si el dispositivo se retira sin desmontarlo primero, existe la posibilidad de que no se hayan transferido todos los datos destinados al dispositivo. En algunos casos, estos datos pueden incluir actualizaciones vitales del directorio, lo que conducirá a la *corrupción del sistema de archivos* (file system corruption), una de las peores cosas que pueden suceder en una computadora.

### Determinar nombres de dispositivos

A veces es difícil determinar el nombre de un dispositivo. En los viejos tiempos, no fue muy difícil. Un dispositivo siempre estaba en el mismo lugar y no cambiaba. A los sistemas tipo Unix les gusta así. Cuando se desarrolló Unix, "cambiar una unidad de disco" implicaba usar una carretilla elevadora para eliminar un lavadodispositivo del tamaño de una máquina desde la sala de computadoras. En los últimos años, la configuración típica de hardware de escritorio se ha vuelto bastante dinámica, y Linux ha evolucionado para ser más flexible que sus antepasados.

En los ejemplos de la sección anterior, aprovechamos la capacidad del escritorio moderno de Linux para montar "automáticamente" el dispositivo y luego determinar el nombre después del hecho. Pero, ¿qué sucede si estamos administrando un servidor o algún otro entorno donde esto no ocurre? ¿Cómo podemos resolverlo?

Primero, veamos cómo el sistema nombra los dispositivos. Si enumeramos el contenido del directorio `/dev` (donde viven todos los dispositivos), podemos ver que hay muchos dispositivos.

```sh
[me@linuxbox ~]$ ls /dev
```

El contenido de este listado revela algunos patrones de nombres de dispositivos. La Tabla 15-2 describe algunos de estos patrones.

**Tabla 15-2**: Nombres de dispositivos de almacenamiento de Linux

Pattern | Device
--------|-------
`/dev/fd*` | Floppy disk drives.
`/dev/hd*` | Discos IDE (PATA) en sistemas más antiguos. Las placas base típicas contienen dos conectores o canales *channels* IDE, cada uno con un cable con dos puntos de conexión para unidades. La primera unidad del cable se llama dispositivo maestro *master* y la segunda se llama dispositivo esclavo *slave*. Los nombres de los dispositivos están ordenados de manera que `/dev/hda` se refiere al dispositivo maestro en el primer canal, `/dev/hdb` es el dispositivo esclavo en el primer canal; `/dev/hdc` es el dispositivo maestro en el segundo canal, y así sucesivamente. Un dígito final indica el número de partición en el dispositivo. Por ejemplo, `/dev/hda1` se refiere a la primera partición en el primer disco duro del sistema, mientras que `/dev/hda` se refiere a todo el disco.
`/dev/lp*` | Printers.
`/dev/sd*` | Discos SCSI. En los sistemas Linux modernos, el núcleo trata todos los dispositivos con forma de disco (incluidos los discos duros PATA/SATA, unidades flash y dispositivos de almacenamiento masivo USB, como reproductores de música portátiles y cámaras digitales) como discos SCSI. El resto del sistema de nombres es similar al esquema de nombres anterior `/dev/hd*` descrito anteriormente.
`/dev/sr*` | Unidades ópticas (lectores y grabadoras de CD/DVD).

Además, a menudo vemos enlaces simbólicos como `/dev/cdrom`, `/dev/dvd` y `/dev/floppy`, que apuntan a los archivos reales del dispositivo, proporcionados como una conveniencia.

Si está trabajando en un sistema que no monta automáticamente dispositivos extraíbles, puede usar la siguiente técnica para determinar cómo se nombra el dispositivo extraíble cuando está conectado. Primero, inicie una vista en tiempo real del archivo `/var/log/messages` o `/var/log/syslog` (puede necesitar privilegios de superusuario para esto).

```sh
[me@linuxbox ~]$ sudo tail -f /var/log/messages
```
Se mostrarán las últimas líneas del archivo y luego se detendrá. A continuación, conecte el dispositivo extraíble. En este ejemplo, utilizaremos una unidad flash de 16 MB. Casi de inmediato, el núcleo notará el dispositivo y lo probará.

```sh
Jul 23 10:07:53 linuxbox kernel: usb 3-2: new full speed USB device using uhci_hcd and address 2
Jul 23 10:07:53 linuxbox kernel: usb 3-2: configuration #1 chosen from 1 choice
Jul 23 10:07:53 linuxbox kernel: scsi3 : SCSI emulation for USB Mass Storage devices
Jul 23 10:07:58 linuxbox kernel: scsi scan: INQUIRY result too short (5), using 36
Jul 23 10:07:58 linuxbox kernel: scsi 3:0:0:0: Direct-Access   Easy   Disk   .00 PQ: 0 ANSI: 2
Jul 23 10:07:59 linuxbox kernel: sd 3:0:0:0: [sdb] 31263 512-byte hardware sectors (16 MB)
Jul 23 10:07:59 linuxbox kernel: sd 3:0:0:0: [sdb] Write Protect is off
Jul 23 10:07:59 linuxbox kernel: sd 3:0:0:0: [sdb] Assuming drive cache: write through
Jul 23 10:07:59 linuxbox kernel: sd 3:0:0:0: [sdb] 31263 512-byte hardware sectors (16 MB)
Jul 23 10:07:59 linuxbox kernel: sd 3:0:0:0: [sdb] Write Protect is off
Jul 23 10:07:59 linuxbox kernel: sd 3:0:0:0: [sdb] Assuming drive cache: write through
Jul 23 10:07:59 linuxbox kernel:  sdb: sdb1
Jul 23 10:07:59 linuxbox kernel: sd 3:0:0:0: [sdb] Attached SCSI removable disk
Jul 23 10:07:59 linuxbox kernel: sd 3:0:0:0: Attached scsi generic sg3 type 0
```
Después de que la pantalla vuelva a detenerse, presione `CTRL-C` para recuperar el mensaje. Las partes interesantes de la salida son las referencias repetidas a `[sdb]`, que coincide con nuestra expectativa de un nombre de dispositivo de disco SCSI. Sabiendo esto, estas dos líneas se vuelven particularmente iluminantes:

```sh
Jul 23 10:07:59 linuxbox kernel:  sdb: sdb1
Jul 23 10:07:59 linuxbox kernel: sd 3:0:0:0: [sdb] Attached SCSI removable disk
```
Esto nos dice que el nombre del dispositivo es `/dev/sdb` para todo el dispositivo y `/dev/sdb1` para la primera partición en el dispositivo. Como hemos visto, ¡trabajar con Linux está lleno de interesantes trabajos de detective!

##### TIP

*Usar la técnica `tail -f /var/log/messages` es una excelente manera de ver lo que el sistema está haciendo en tiempo casi real.*

Con el nombre de nuestro dispositivo en la mano, ahora podemos montar la unidad flash.
```sh
[me@linuxbox ~]$ sudo mkdir /mnt/flash
[me@linuxbox ~]$ sudo mount /dev/sdb1 /mnt/flash
[me@linuxbox ~]$ df
Filesystem           1K-blocks      Used Available Use% Mounted on
/dev/sda2             15115452   5186944   9775164  35% /
/dev/sda5             59631908  31777376  24776480  57% /home
/dev/sda1               147764     17277    122858  13% /boot
tmpfs                   776808         0    776808   0% /dev/shm
/dev/sdb1                15560         0     15560   0% /mnt/flash
```
El nombre del dispositivo permanecerá igual mientras permanezca físicamente conectado a la computadora y la computadora no se reinicie.

## CREAR NUEVOS SISTEMAS DE ARCHIVOS

Supongamos que queremos formatear la unidad flash con un sistema de archivos nativo de Linux, en lugar del sistema FAT32 que tiene ahora. Esto implica dos pasos.

1. (opcional) Cree un nuevo diseño de partición si el existente no es de nuestro agrado.
2. Cree un nuevo sistema de archivos vacío en la unidad.

##### ADVERTENCIA

*En el siguiente ejercicio, vamos a formatear una unidad flash. ¡Utilice una unidad que no contenga nada que le importe porque se borrará! Nuevamente, **asegúrese de especificar el nombre de dispositivo correcto para su sistema, no el que se muestra en el texto. Si no se tiene en cuenta esta advertencia, podría formatear (es decir, borrar) la unidad incorrecta**.*

### Manipulación de particiones con fdisk

`fdisk` es uno de los muchos programas disponibles (tanto de línea de comandos como gráficos) que nos permite interactuar directamente con dispositivos similares a discos (como unidades de disco duro y unidades flash) a un nivel muy bajo. Con esta herramienta podemos editar, eliminar y crear particiones en el dispositivo. Para trabajar con nuestra unidad flash, primero debemos desmontarla (si es necesario) y luego invocar el programa `fdisk` de la siguiente manera:

```sh
[me@linuxbox ~]$ sudo umount /dev/sdb1
[me@linuxbox ~]$ sudo fdisk /dev/sdb
```

Tenga en cuenta que debemos especificar el dispositivo en términos de todo el dispositivo, no por número de partición. Después de que se inicie el programa, veremos el siguiente mensaje:

```sh
Command (m for help):
```
Al ingresar una `m` se mostrará el menú del programa.

```sh
Command action
   a   toggle a bootable flag
   b   edit bsd disklabel
   c   toggle the dos compatibility flag
   d   delete a partition
   l   list known partition types
   m   print this menu
   n   add a new partition
   o   create a new empty DOS partition table
   p   print the partition table
   q   quit without saving changes
   s   create a new empty Sun disklabel
   t   change a partition's system id
   u   change display/entry units
   v   verify the partition table
   w   write table to disk and exit
   x   extra functionality (experts only)
   
Command (m for help):

Acción de comando
   a alternar un indicador de arranque
   b editar bsd etiqueta de disco
   c alternar el indicador de compatibilidad de dos
   d eliminar una partición
   l enumerar tipos de particiones conocidas
   m imprimir este menú
   n agregar una nueva partición
   o crear una nueva tabla de particiones DOS vacía
   p imprimir la tabla de particiones
   q salir sin guardar los cambios
   s crear una nueva etiqueta de disco Sun vacía
   t cambiar la identificación del sistema de una partición
   u cambiar las unidades de visualización / entrada
   v verificar la tabla de particiones
   w escribir tabla en disco y salir de
   x funcionalidad adicional (solo expertos)
```
Lo primero que queremos hacer es examinar el diseño de partición existente. Hacemos esto ingresando `p` para imprimir la tabla de particiones para el dispositivo.

```sh
Command (m for help): p

Disk /dev/sdb: 16 MB, 16006656 bytes
1 heads, 31 sectors/track, 1008 cylinders
Units = cylinders of 31 * 512 = 15872 bytes

   Device Boot      Start         End      Blocks   Id  System
/dev/sdb1               2        1008       15608+   b  W95 FAT32
```
En este ejemplo, vemos un dispositivo de 16 MB con una única partición (1) que utiliza 1.006 de los 1.008 cilindros disponibles en el dispositivo. La partición se identifica como una partición FAT32 de Windows 95. Algunos programas usarán este identificador para limitar los tipos de operaciones que se pueden realizar en el disco, pero la mayoría de las veces no es crítico cambiarlo. Sin embargo, en interés de esta demostración, la cambiaremos para indicar una partición de Linux. Para hacer esto, primero debemos averiguar qué ID se usa para identificar una partición de Linux. En la lista anterior, vemos que el ID `b` se usa para especificar la partición existente. Para ver una lista de los tipos de particiones disponibles, consulte el menú del programa. Allí podemos ver la siguiente opción:

```sh
l   list known partition types
```
Si ingresamos `l` en el indicador, se muestra una gran lista de posibles tipos. Entre ellos vemos `b` para nuestro tipo de partición existente y `83` para Linux.

Volviendo al menú, vemos esta opción para cambiar una ID de partición:

```sh
t   change a partition's system id
```
Ingresamos `t` en la solicitud e ingresamos la nueva ID.
```sh
Command (m for help): t
Selected partition 1
Hex code (type L to list codes): 83
Changed system type of partition 1 to 83 (Linux)
```
Esto completa todos los cambios que necesitamos hacer. Hasta este punto, el dispositivo no ha sido tocado (todos los cambios se han almacenado en la memoria, no en el dispositivo físico), por lo que escribiremos la tabla de partición modificada en el dispositivo y saldremos. Para hacer esto, ingresamos `w` en el indicador.

```sh
Command (m for help): w
The partition table has been altered!

Calling ioctl() to re-read partition table.

WARNING: If you have created or modified any DOS 6.x
partitions, please see the fdisk manual page for additional
information.
Syncing disks.
[me@linuxbox ~]$
```
Si hubiéramos decidido dejar el dispositivo sin alterar, podríamos haber ingresado `q` en el indicador, que habría salido del programa sin escribir los cambios. Podemos ignorar con seguridad el mensaje de advertencia que suena ominoso.

### Crear un nuevo sistema de archivos con mkfs

Una vez realizada la edición de la partición (por ligera que haya sido), es hora de crear un nuevo sistema de archivos en nuestra unidad flash. Para hacer esto, usaremos `mkfs` (abreviatura de "make file system"), que puede crear sistemas de archivos en una variedad de formatos. Para crear un sistema de archivos `ext4` en el dispositivo, utilizamos la opción `-t` para especificar el tipo de sistema "ext4", seguido del nombre del dispositivo que contiene la partición que queremos formatear.

```sh
[me@linuxbox ~]$ sudo mkfs -t ext4 /dev/sdb1
mke2fs 2.23.2 (12-Jul-2011)
Filesystem label=
OS type: Linux
Block size=1024 (log=0)
Fragment size=1024 (log=0)
3904 inodes, 15608 blocks
780 blocks (5.00%) reserved for the super user
First data block=1
Maximum filesystem blocks=15990784
2 block groups
8192 blocks per group, 8192 fragments per group
1952 inodes per group
Superblock backups stored on blocks:
    8193

Writing inode tables: done
Creating journal (1024 blocks): done
Writing superblocks and filesystem accounting information: done

This filesystem will be automatically checked every 34 mounts or
180 days, whichever comes first. Use tune2fs -c or -i to override.
[me@linuxbox ~]$
```
El programa mostrará mucha información cuando ext4 sea el tipo de sistema de archivos elegido. Para volver a formatear el dispositivo a su sistema de archivos FAT32 original, especifique `vfat` como tipo de sistema de archivos.
```sh
[me@linuxbox ~]$ sudo mkfs -t vfat /dev/sdb1
```
Este proceso de particionamiento y formateo se puede usar en cualquier momento que se agreguen dispositivos de almacenamiento adicionales al sistema. Si bien trabajamos con una pequeña unidad flash, el mismo proceso se puede aplicar a los discos duros internos y a otros dispositivos de almacenamiento extraíbles, como los discos duros USB.

## PRUEBA Y REPARACIÓN DE SISTEMAS DE ARCHIVOS

En nuestra discusión anterior del archivo `/etc/fstab`, vimos algunos dígitos misteriosos al final de cada línea. Cada vez que se inicia el sistema, verifica rutinariamente la integridad de los sistemas de archivos antes de montarlos. Esto lo hace el programa `fsck` (abreviatura de "file system check (comprobación del sistema de archivos)"). El último número en cada entrada de `fstab` especifica el orden en que se deben verificar los dispositivos. En nuestro ejemplo anterior, vemos que el sistema de archivos raíz se comprueba en primer lugar, seguido de los de casa y el arranque de los sistemas de archivos. Los dispositivos con un cero como último dígito no se verifican rutinariamente.

Además de verificar la integridad de los sistemas de archivos, `fsck` también puede reparar sistemas de archivos corruptos con diversos grados de éxito, dependiendo de la cantidad de daño. En los sistemas de archivos tipo Unix, las partes recuperadas de los archivos se colocan en el directorio *lost+found*, ubicado en la raíz de cada sistema de archivos.

Para verificar nuestra unidad flash (que debe desmontarse primero), podríamos hacer lo siguiente:
```sh
[me@linuxbox ~]$ sudo fsck /dev/sdb1
fsck 1.40.8 (13-Mar-2016)
e2fsck 1.40.8 (13-Mar-2016)
/dev/sdb1: clean, 11/3904 files, 1661/15608 blocks
```
En estos días, la corrupción del sistema de archivos es bastante rara a menos que haya un problema de hardware, como una unidad de disco defectuosa. En la mayoría de los sistemas, la corrupción del sistema de archivos detectada en el momento del arranque hará que el sistema se detenga y le indique que ejecute `fsck` antes de continuar.

##### QUE ES EL FSCK?

*En la cultura Unix, la palabra `fsck` se usa a menudo en lugar de una palabra popular con la que comparte tres letras. Esto es especialmente apropiado, dado que probablemente pronunciará la palabra antes mencionada si se encuentra en una situación en la que se ve obligado a ejecutar `fsck`.*

## MOVER DATOS DIRECTAMENTE HACIA Y DESDE DISPOSITIVOS

Si bien generalmente pensamos que los datos en nuestras computadoras están organizados en archivos, también es posible pensar en los datos en forma "en bruto". Si observamos una unidad de disco, por ejemplo, vemos que consta de una gran cantidad de "bloques" de datos que el sistema operativo ve como directorios y archivos. Sin embargo, si pudiéramos tratar una unidad de disco como simplemente una gran colección de bloques de datos, podríamos realizar tareas útiles, como dispositivos de clonación.

El programa `dd` realiza esta tarea. Copia bloques de datos de un lugar a otro. Utiliza una sintaxis única (por razones históricas) y generalmente se usa de esta manera:

```sh
dd if=input_file of=output_file [bs=block_size [count=blocks]]
```

##### ADVERTENCIA

*El comando `dd` es muy poderoso. Aunque su nombre deriva de "data definition (definición de datos)", a veces se le llama "destroy disk(destruir disco)" porque los usuarios a menudo escriben mal el `if` o el `of` la especificación. **¡Siempre verifique sus especificaciones de entrada y salida antes de presionar ENTER!** *

Digamos que teníamos dos unidades flash USB del mismo tamaño y queríamos copiar exactamente la primera unidad a la segunda. Si conectamos ambas unidades a la computadora y se asignan a dispositivos `/dev/sdb` y `/dev/sdc`, respectivamente, podríamos copiar todo en la primera unidad a la segunda unidad con lo siguiente:
```sh
dd if=/dev/sdb of=/dev/sdc
```
Alternativamente, si solo el primer dispositivo estuviera conectado a la computadora, podríamos copiar su contenido a un archivo ordinario para su posterior restauración o copia.
```sh
dd if=/dev/sdb of=flash_drive.img
```
### Crear imágenes de CD-ROM

Escribir un CD-ROM grabable (ya sea un CD-R o CD-RW) consta de dos pasos.

1. Construir un archivo de imagen ISO que sea la imagen exacta del sistema de archivos del CD-ROM
2. Escribir el archivo de imagen en el CD-ROM

### Crear una copia de imagen de un CD-ROM

Si queremos hacer una imagen ISO de un CD-ROM existente, podemos usar `dd` para leer todos los bloques de datos del CD-ROM y copiarlos en un archivo local. Digamos que teníamos un CD de Ubuntu y queríamos hacer un archivo ISO que pudiéramos luego se usa para hacer más copias. Después de insertar el CD y determinar su nombre de dispositivo (asumiremos `/dev/cdrom` ), podemos hacer el archivo ISO de esta manera:

```sh
dd if=/dev/cdrom of=ubuntu.iso
```
Esta técnica también funciona para DVD de datos, pero no para CD de audio, ya que no utiliza un sistema de archivos para el almacenamiento. Para CD de audio, mire el comando `cdrdao`.

### Crear una imagen a partir de una colección de archivos

Para crear un archivo de imagen ISO que contenga el contenido de un directorio, utilizamos el programa `genisoimage`. Para hacer esto, primero creamos un directorio que contiene todos los archivos que queremos incluir en la imagen y luego ejecutamos el comando `genisoimage` para crear el archivo de imagen. Por ejemplo, si creamos un directorio llamado `~/cd-rom-files` y lo llenamos con archivos para nuestro CD-ROM, podríamos crear un archivo de imagen llamado `cd-rom.iso` con el siguiente comando:

```sh
genisoimage -o cd-rom.iso -R -J ~/cd-rom-files
```
La opción `-R` agrega metadatos para las extensiones de *Rock Ridge*, lo que permite el uso de nombres de archivo largos y permisos de archivo de estilo POSIX. Del mismo modo, la opción `-J` habilita las extensiones *Joliet*, que permiten nombres de archivo largos para Windows.

##### UN PROGRAMA POR CUALQUIER OTRO NOMBRE. . .

*Si mira tutoriales en línea para crear y grabar medios ópticos como CD-ROM y DVD, con frecuencia encontrará dos programas llamados `mkisofs` y `cdrecord`. Estos programas formaban parte de un paquete popular llamado  `cdrtools` creado por Jörg Schilling. En el verano de 2006, el Sr. Schilling realizó un cambio de licencia en una parte del paquete `cdrtools`, que, en opinión de muchos en la comunidad Linux, creó una incompatibilidad de licencia con la GNU GPL. Como resultado, se inició una bifurcación del proyecto `cdrtools` que ahora incluye programas de reemplazo para `cdrecord` y `mkisofs` llamados `wodim` y `genisoimage`, respectivamente.*

## ESCRIBIR IMÁGENES DE CD-ROM

Después de tener un archivo de imagen, podemos grabarlo en nuestro medio óptico. La mayoría de los comandos que discutiremos en las secciones siguientes se pueden aplicar a los medios de CD-ROM y DVD grabables.

### Montaje de una imagen ISO directamente

Hay un truco que podemos usar para montar una imagen ISO mientras todavía está en nuestro disco duro y tratarla como si ya estuviera en un medio óptico. Al agregar la opción -o loop para montar (junto con el tipo de sistema de archivos -t iso9660 requerido ), podemos montar el archivo de imagen como si fuera un dispositivo y adjuntarlo al árbol del sistema de archivos.

mkdir / mnt / iso_image
mount -t iso9660 -o loop image.iso / mnt / iso_image

En este ejemplo, creamos un punto de montaje llamado / mnt / iso_image y luego montamos el archivo de imagen image.iso en ese punto de montaje. Después de montar la imagen, puede tratarse como si fuera un CD-ROM o DVD real. Recuerde desmontar la imagen cuando ya no sea necesaria .

Poner en blanco un CD-ROM regrabable
Necesidades de medios regrabables CD-RW a ser borrados o borradas antes de que pueda ser reutilizado. Para hacer esto, podemos usar wodim , especificando el nombre del dispositivo para la grabadora de CD y el tipo de supresión que se realizará. El programa wodim ofrece varios tipos. El más mínimo (y más rápido) es el tipo "rápido".

wodim dev = / dev / cdrw blank = rápido

Escribir una imagen
Para escribir una imagen, nuevamente usamos wodim , especificando el nombre del dispositivo de escritura de medios ópticos y el nombre del archivo de imagen.

wodim dev = / dev / cdrw image.iso

Además del nombre del dispositivo y el archivo de imagen, wodim admite un gran conjunto de opciones. Dos comunes son -v para salida detallada y -dao , que escribe el disco en modo disco a la vez . Este modo debe usarse si está preparando un disco para reproducción comercial. El modo predeterminado para wodim es track-at-once , que es útil para grabar pistas de música.

RESUMIENDO
En este capítulo, analizamos las tareas básicas de administración de almacenamiento. Hay, por supuesto, muchos más. Linux admite una amplia gama de dispositivos de almacenamiento y esquemas de sistemas de archivos. También ofrece muchas características para la interoperabilidad con otros sistemas.

CRÉDITO ADICIONAL
A menudo es útil verificar la integridad de una imagen ISO que hemos descargado. En la mayoría de los casos, un distribuidor de una imagen ISO también proporcionará un archivo de suma de verificación . Una suma de comprobación es el resultado de un cálculo matemático exótico que da como resultado un número que representa el contenido del archivo de destino. Si el contenido del archivo cambia incluso un bit, la suma de comprobación resultante será muy diferente. El método más común de generación de suma de comprobación utiliza el programa md5sum . Cuando usa md5sum , produce un número hexadecimal único.

md5sum image.iso
34e354760f9bb7fbf85c96f6a3f94ece image.iso

Después de descargar una imagen, debe ejecutar md5sum contra ella y comparar los resultados con el valor de md5sum proporcionado por el editor.

Además de verificar la integridad de un archivo descargado, podemos usar md5sum para verificar los medios ópticos recién escritos. Para hacer esto, primero calculamos la suma de verificación del archivo de imagen y luego calculamos una suma de verificación para los medios. El truco para verificar los medios es limitar el cálculo a solo la parte de los medios ópticos que contiene la imagen. Hacemos esto determinando el número de bloques de 2,048 bytes que contiene la imagen (los medios ópticos siempre se escriben en bloques de 2,048 bytes) y leyendo esa cantidad de bloques de los medios. En algunos tipos de medios, esto no es obligatorio. Los discos CD-R y CD-RW escritos en modo de disco a la vez se pueden verificar de esta manera.

md5sum / dev / cdrom
34e354760f9bb7fbf85c96f6a3f94ece / dev / cdrom

Muchos tipos de medios, como los DVD, requieren un cálculo preciso de la cantidad de bloques. En el siguiente ejemplo, verificamos la integridad del archivo de imagen dvd-image.iso y el disco en el lector de DVD / dev / dvd . ¿Puedes imaginar cómo funciona esto?

md5sum dvd-image.iso; dd if = / dev / dvd bs = 2048 count = $ (( $ (stat -c "% s" dvd-image.iso) / 2048 ))
| md5sum

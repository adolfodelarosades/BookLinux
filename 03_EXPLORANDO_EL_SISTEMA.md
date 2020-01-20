# 3 EXPLORANDO EL SISTEMA

Ahora que sabemos cómo movernos por el sistema de archivos, es hora de una visita guiada a nuestro sistema Linux. Sin embargo, antes de comenzar, aprenderemos algunos comandos más que serán útiles en el camino.

`ls` contenidos de listas

`file` Determinar tipo de archivo

`less` Ver contenido del archivo

## MÁS DIVERSIÓN CON LS

El comando ls es probablemente el comando más utilizado, y por una buena razón. Con él, podemos ver el contenido del directorio y determinar una variedad de atributos importantes de archivos y directorios. Como hemos visto, simplemente podemos ingresar ls para obtener una lista de archivos y subdirectorios contenidos en el directorio de trabajo actual.

```sh
[me@linuxbox ~]$ ls
Desktop  Documents  Music  Pictures  Public  Templates  Videos
```

Además del directorio de trabajo actual, podemos especificar el directorio a enumerar, así:

me @ linuxbox ~] $ ls / usr
bin juegos incluyen lib local sbin share src

Incluso podemos especificar múltiples directorios. En el siguiente ejemplo, enumeramos el directorio de inicio del usuario (simbolizado por el carácter ~ ) y el directorio / usr :

[me @ linuxbox ~] $ ls ~ / usr
/ home / me:
Documentos de escritorio Música Imágenes Plantillas públicas Videos
/ usr: los
juegos bin incluyen lib local sbin share src

También podemos cambiar el formato de la salida para revelar más detalles.

[me @ linuxbox ~] $ ls -l
total 56
drwxrwxr-x 2 me me 4096 2017-10-26 17:20 Desktop
drwxrwxr-x 2 me me 4096 2017-10-26 17:20 Documentos
drwxrwxr-x 2 me me 4096 2017-10-26 17:20 Música
drwxrwxr-x 2 me me 4096 2017-10-26 17:20 Imágenes
drwxrwxr-x 2 me me 4096 2017-10-26 17:20 Público
drwxrwxr-x 2 me me 4096 2017 -10-26 17:20 Plantillas
drwxrwxr-x 2 me me 4096 2017-10-26 17:20 Videos

Al agregar -l al comando, cambiamos la salida al formato largo.

### Opciones y argumentos

Esto nos lleva a un punto muy importante sobre cómo funcionan la mayoría de los comandos. Los comandos suelen ir seguidos de una o más opciones que modifican su comportamiento y, además, de uno o más argumentos , los elementos sobre los que actúa el comando. Entonces, la mayoría de los comandos se ven así:

comando -opciones argumentos

La mayoría de los comandos usan opciones, que consisten en un solo carácter precedido por un guión, por ejemplo, -l . Sin embargo, muchos comandos, incluidos los del Proyecto GNU, también admiten opciones largas , que consisten en una palabra precedida por dos guiones. Además, muchos comandos permiten unir varias opciones cortas. En el siguiente ejemplo, el comando ls tiene dos opciones, que son la opción l para producir una salida de formato largo y la opción t para ordenar el resultado por el tiempo de modificación del archivo.

[yo @ linuxbox ~] $ ls -lt

Agregaremos la opción larga --reverse para invertir el orden de clasificación.

[yo @ linuxbox ~] $ ls -lt --reverse

NOTA

Las opciones de comando, como los nombres de archivo en Linux, distinguen entre mayúsculas y minúsculas.

El comando ls tiene una gran cantidad de opciones posibles, las más comunes se enumeran en la Tabla 3-1 .

Tabla 3-1: Opciones comunes de ls

Opción

Opción larga

Descripción

-un

--todos

Enumere todos los archivos, incluso aquellos con nombres que comienzan con un punto, que normalmente no están listados (es decir, ocultos).

-UN

--casi todos

Al igual que la opción -a , excepto que no aparece en la lista . (directorio actual) y .. (directorio padre).

-re

--directorio

Por lo general, si se especifica un directorio, ls mostrará el contenido del directorio, no el directorio en sí. Use esta opción junto con la opción -l para ver detalles sobre el directorio en lugar de su contenido.

-F

--clasificar

Esta opción agregará un carácter indicador al final de cada nombre listado. Por ejemplo, agregará una barra diagonal ( / ) si el nombre es un directorio.

-h

legible por humanos

En listados de formato largo, muestre los tamaños de archivo en formato legible por humanos en lugar de en bytes.

-l

 

Mostrar resultados en formato largo.

-r

--marcha atrás

Mostrar los resultados en orden inverso. Normalmente, ls muestra sus resultados en orden alfabético ascendente.

-S

 

Ordenar resultados por tamaño de archivo.

-t

 

Ordenar por tiempo de modificación.

### Una mirada más larga al formato largo

Como vimos anteriormente, la opción -l hace que ls muestre sus resultados en formato largo. Este formato contiene una gran cantidad de información útil. Aquí está el directorio de ejemplos de un sistema Ubuntu:

-rw-r - r-- 1 raíz raíz 3576296 03/04/2017 11:05 Experiencia ubuntu.ogg
-rw-r - r-- 1 raíz root 1186219 03/04/2017 11:05 kubuntu-leaflet. png
-rw-r - r-- 1 raíz raíz 47584 03/04/2017 11:05 logo-Edubuntu.png
-rw-r - r-- 1 raíz raíz 44355 03/04/2017 11:05 logo- Kubuntu.png
-rw-r - r-- 1 raíz raíz 34391 03/04/2017 11:05 logo-Ubuntu.png
-rw-r - r-- 1 raíz raíz 32059 2017-04-03 11:05 oo-cd-cover.odf
-rw-r - r-- 1 raíz raíz 159744 03/04/2017 11:05 oo-derivados.doc
-rw-r - r-- 1 raíz raíz 27837 2017-04-03 11:05 oo-maxwell.odt
-rw-r - r-- 1 raíz root 98816 2017-04-03 11:05 oo-trig .xls
-rw-r - r-- 1 raíz raíz 453764 03/04/2017 11:05 oo-welcome.odt
-rw-r - r-- 1 raíz raíz 358374 03/04/2017 11:05 ubuntu Sax.ogg

La Tabla 3-2 nos proporciona una mirada a los diferentes campos de uno de los archivos y sus significados.

Tabla 3-2: Campos de listado largos de ls

Campo

Sentido

-rw-r - r--

Derechos de acceso al archivo. El primer carácter indica el tipo de archivo. Entre los diferentes tipos, un guión inicial significa un archivo normal, mientras que una d indica un directorio. Los siguientes tres caracteres son los derechos de acceso para el propietario del archivo, los siguientes tres son para los miembros del grupo del archivo y los tres últimos son para todos los demás. El Capítulo 9 discute el significado completo de esto con más detalle.

1

Número de enlaces duros del archivo. Consulte "Enlaces simbólicos" en la página 21 y "Enlaces duros" en la página 22 .

raíz

El nombre de usuario del propietario del archivo.

raíz

El nombre del grupo propietario del archivo.

32059

Tamaño del archivo en bytes.

2017-04-03 11:05

Fecha y hora de la última modificación del archivo.

oo-cd-cover.odf

Nombre del archivo.

## DETERMINAR EL TIPO DE ARCHIVO CON ARCHIVO

A medida que exploremos el sistema, será útil saber qué archivos contienen. Para hacer esto, usaremos el comando de archivo para determinar el tipo de archivo. Como discutimos anteriormente, los nombres de archivo en Linux no están obligados a reflejar el contenido de un archivo. Mientras que normalmente se esperaría que un nombre de archivo como picture.jpg contenga una imagen comprimida en JPEG, no es obligatorio en Linux. Podemos invocar el comando de archivo de esta manera:

nombre de archivo

Cuando se invoca, el comando del archivo imprimirá una breve descripción del contenido del archivo. Por ejemplo:

[me @ linuxbox ~] $ archivo picture.jpg
picture.jpg: datos de imagen JPEG, estándar JFIF 1.01

Hay muchos tipos de archivos. De hecho, una de las ideas comunes en los sistemas operativos tipo Unix como Linux es que "todo es un archivo". A medida que avancemos con nuestras lecciones, veremos cuán cierta es esa declaración.

Si bien muchos de los archivos en nuestro sistema son familiares, por ejemplo, archivos MP3 y JPEG, hay muchos tipos que son un poco menos obvios y algunos que son bastante extraños.

### VER EL CONTENIDO DEL ARCHIVO CON LESS

El comando less es un programa para ver archivos de texto. En todo nuestro sistema Linux, hay muchos archivos que contienen texto legible por humanos. El programa menos proporciona una forma conveniente de examinarlos.

¿Por qué querríamos examinar los archivos de texto? Debido a que muchos de los archivos que contienen configuraciones del sistema (llamados archivos de configuración ) se almacenan en este formato, y poder leerlos nos da una idea de cómo funciona el sistema. Además, algunos de los programas reales que utiliza el sistema (llamados scripts ) se almacenan en este formato. En capítulos posteriores, aprenderemos cómo editar archivos de texto para modificar la configuración del sistema y escribir nuestros propios scripts, pero por ahora solo veremos su contenido.

El comando less se usa así:

menos nombre de archivo

¿Qué es el "texto"?

Hay muchas formas de representar información en una computadora. Todos los métodos implican definir una relación entre la información y algunos números que se utilizarán para representarla. Las computadoras, después de todo, entienden solo los números, y todos los datos se convierten en representación numérica.

Algunos de estos sistemas de representación son muy complejos (como archivos de video comprimidos), mientras que otros son bastante simples. Uno de los primeros y más simples se llama texto ASCII. ASCII (pronunciado "as-key") es la abreviatura de American Standard Code for Information Interchange. Este es un esquema de codificación simple que se utilizó por primera vez en máquinas de teletipo para asignar caracteres de teclado a números.

El texto es un mapeo simple uno a uno de caracteres a números. Es muy compacto. Cincuenta caracteres de texto se traducen en cincuenta bytes de datos. Es importante comprender que el texto solo contiene una asignación simple de caracteres a números. No es lo mismo que un documento de procesador de textos como uno creado por Microsoft Word o LibreOffice Writer. Esos archivos, en contraste con el texto ASCII simple, contienen muchos elementos que no son de texto que se utilizan para describir su estructura y formato. Los archivos de texto ASCII sin formato contienen solo los propios caracteres y algunos códigos de control rudimentarios, como pestañas, retornos de carro y avances de línea.

En todo un sistema Linux, muchos archivos se almacenan en formato de texto, y hay muchas herramientas de Linux que funcionan con archivos de texto. Incluso Windows reconoce la importancia de este formato. El conocido programa NOTEPAD.EXE es un editor de archivos de texto ASCII sin formato.

Una vez iniciado, el programa less nos permite avanzar y retroceder a través de un archivo de texto. Por ejemplo, para examinar el archivo que define todas las cuentas de usuario del sistema, ingrese el siguiente comando:

[yo @ linuxbox ~] $ menos / etc / passwd

Una vez que se inicia el programa less , podemos ver el contenido del archivo. Si el archivo tiene más de una página, podemos desplazarnos hacia arriba y hacia abajo. Para salir menos , presione q .

La Tabla 3-3 enumera los comandos de teclado más comunes utilizados por less .

Tabla 3-3: menos comandos

Mando

Acción

PÁGINA ARRIBA o b

Desplazarse hacia atrás una página

PÁGINA ABAJO o espacio

Desplazarse hacia adelante una página

Flecha arriba

Desplazarse hacia arriba una línea

Flecha hacia abajo

Desplazarse hacia abajo una línea

sol

Moverse al final del archivo de texto

1G o g

Ir al principio del archivo de texto

/caracteres

Busca la próxima aparición de personajes

norte

Busque la siguiente aparición de la búsqueda anterior.

h

Mostrar pantalla de ayuda

q

Dejar menos

##### MENOS ES MÁS

El programa less fue diseñado como un reemplazo mejorado de un programa anterior de Unix llamado more . El nombre menos es un juego de palabras con la frase "menos es más", un lema de arquitectos y diseñadores modernistas.

menos cae en la clase de programas llamados buscapersonas , programas que permiten la visualización fácil de documentos de texto largos de una página por página. Mientras que el programa más solo puede avanzar de página, el programa menos permite la paginación tanto hacia adelante como hacia atrás y tiene muchas otras características también.

## TOMANDO UNA VISITA GUIADA

El diseño del sistema de archivos en un sistema Linux es muy similar al que se encuentra en otros sistemas similares a Unix. El diseño se especifica realmente en un estándar publicado llamado Estándar de jerarquía del sistema de archivos de Linux . No todas las distribuciones de Linux se ajustan exactamente al estándar, pero la mayoría se acercan bastante.

##### ¡RECUERDA EL TRUCO DE COPIAR Y PEGAR!

Si está usando un mouse, puede hacer doble clic en un nombre de archivo para copiarlo y hacer clic con el botón central para pegarlo en los comandos.

A continuación, vamos a recorrer el sistema de archivos nosotros mismos para ver qué hace funcionar nuestro sistema Linux. Esto nos dará la oportunidad de practicar nuestras habilidades de navegación. Una de las cosas que descubriremos es que muchos de los archivos interesantes están en texto legible para humanos. A medida que avanzamos en nuestro recorrido, intente lo siguiente:

cd en un directorio dado.
Liste el contenido del directorio con ls -l .
Si ve un archivo interesante, determine su contenido con el archivo .
Si parece que puede ser texto, intente verlo con menos .
Si accidentalmente intentamos ver un archivo que no es de texto y codifica la ventana del terminal, podemos recuperarlo ingresando el comando de reinicio .

Mientras paseamos, no tengas miedo de mirar cosas. Los usuarios habituales tienen prohibido en gran medida arruinar las cosas. ¡Ese es el trabajo del administrador del sistema! Si un comando se queja de algo, simplemente pasa a otra cosa. Pase algo de tiempo mirando a su alrededor. El sistema es nuestro para explorar. ¡Recuerde, en Linux, no hay secretos!

La Tabla 3-4 enumera solo algunos de los directorios que podemos explorar. Puede haber algunas pequeñas diferencias dependiendo de nuestra distribución de Linux. ¡No tengas miedo de mirar alrededor y probar más!

Tabla 3-4: Directorios encontrados en sistemas Linux

Directorio

Comentarios

/ /

El directorio raíz, donde todo comienza.

/compartimiento

Contiene archivos binarios (programas) que deben estar presentes para que el sistema arranque y se ejecute.

/bota

Contiene el kernel de Linux, la imagen de disco RAM inicial (para los controladores necesarios en el momento del arranque) y el cargador de arranque. Los archivos interesantes incluyen /boot/grub/grub.conf o menu.lst , que se usa para configurar el cargador de arranque, y / boot / vmlinuz (o algo similar), el kernel de Linux.

/ dev

Este es un directorio especial que contiene nodos de dispositivo . "Todo es un archivo" también se aplica a los dispositivos. Aquí es donde el núcleo mantiene una lista de todos los dispositivos que comprende.

/ etc

El directorio / etc contiene todos los archivos de configuración de todo el sistema. También contiene una colección de scripts de shell que inician cada uno de los servicios del sistema en el momento del arranque. Todo en este directorio debe ser texto legible. Si bien todo en / etc es interesante, aquí hay algunos favoritos de todos los tiempos: / etc / crontab , un archivo que define cuándo se ejecutarán los trabajos automatizados; / etc / fstab , una tabla de dispositivos de almacenamiento y sus puntos de montaje asociados; y / etc / passwd , una lista de las cuentas de usuario.

/hogar

En configuraciones normales, cada usuario recibe un directorio en / home . Los usuarios comunes solo pueden escribir archivos en sus directorios personales. Esta limitación protege al sistema de la actividad errante del usuario.

/ lib

Contiene archivos de biblioteca compartida utilizados por los programas principales del sistema. Estos son similares a las bibliotecas de enlaces dinámicos (DLL) en Windows.

/ perdido + encontrado

Cada partición o dispositivo formateado que use un sistema de archivos Linux, como ext3, tendrá este directorio. Se utiliza en el caso de una recuperación parcial de un evento de corrupción del sistema de archivos. A menos que algo realmente malo le haya sucedido a su sistema, este directorio permanecerá vacío.

/medios de comunicación

En los sistemas Linux modernos, el directorio / media contendrá los puntos de montaje para medios extraíbles, como unidades USB, CD-ROM, etc., que se montan automáticamente en la inserción.

/ mnt

En sistemas Linux más antiguos, el directorio / mnt contiene puntos de montaje para dispositivos extraíbles que se han montado manualmente.

/optar

El directorio / opt se utiliza para instalar software "opcional". Esto se utiliza principalmente para almacenar productos de software comerciales que pueden instalarse en el sistema.

/ proc

El directorio / proc es especial. No es un sistema de archivos real en el sentido de los archivos almacenados en su disco duro. Más bien, es un sistema de archivos virtual mantenido por el kernel de Linux. Los "archivos" que contiene son mirillas en el núcleo mismo. Los archivos son legibles y le darán una imagen de cómo el núcleo ve su computadora.

/raíz

Este es el directorio de inicio de la cuenta raíz.

/ sbin

Este directorio contiene binarios del "sistema". Estos son programas que realizan tareas vitales del sistema que generalmente están reservadas para el superusuario.

/ tmp

El directorio / tmp está destinado al almacenamiento de archivos temporales y transitorios creados por varios programas. Algunas configuraciones hacen que este directorio se vacíe cada vez que se reinicia el sistema.

/ usr

El árbol de directorio / usr es probablemente el más grande en un sistema Linux. Contiene todos los programas y archivos de soporte utilizados por los usuarios habituales.

/ usr / bin

/ usr / bin contiene los programas ejecutables instalados por su distribución de Linux. No es raro que este directorio contenga miles de programas.

/ usr / lib

Las bibliotecas compartidas para los programas en / usr / bin .

/ usr / local

El árbol / usr / local es donde se instalan los programas que no están incluidos en su distribución pero que están destinados para uso en todo el sistema. Los programas compilados a partir del código fuente normalmente se instalan en / usr / local / bin . En un sistema Linux recién instalado, este árbol existe, pero estará vacío hasta que el administrador del sistema ponga algo en él.

/ usr / sbin

Contiene más programas de administración del sistema.

/ usr / share

/ usr / share contiene todos los datos compartidos utilizados por los programas en / usr / bin . Esto incluye cosas como archivos de configuración predeterminados, íconos, fondos de pantalla, archivos de sonido, etc.

/ usr / share / doc

La mayoría de los paquetes instalados en el sistema incluirán algún tipo de documentación. En / usr / share / doc , encontraremos archivos de documentación organizados por paquete.

/ var

Con la excepción de / tmp y / home , los directorios que hemos visto hasta ahora permanecen relativamente estáticos; es decir, sus contenidos no cambian. El árbol de directorio / var es donde se almacenan los datos que probablemente cambien. Aquí se encuentran varias bases de datos, archivos de spool, correo de usuario, etc.

/ var / log

/ var / log contiene archivos de registro, registros de diversas actividades del sistema. Estos son importantes y deben ser monitoreados de vez en cuando. Los más útiles son / var / log / messages y / var / log / syslog . Tenga en cuenta que, por razones de seguridad en algunos sistemas, debe ser el superusuario para ver los archivos de registro.


## ENLACES SIMBÓLICOS

Al mirar a nuestro alrededor, es probable que veamos una lista de directorios (por ejemplo, / lib ) con una entrada como esta:

lrwxrwxrwx 1 root root 11 2018-08-11 07:34 libc.so.6 -> libc-2.6.so

Observe cómo la primera letra de la lista es l y la entrada parece tener dos nombres de archivo. Este es un tipo especial de archivo llamado enlace simbólico (también conocido como enlace suave o enlace simbólico ). En la mayoría de los sistemas tipo Unix, es posible tener un archivo referenciado por múltiples nombres. Si bien el valor de esto puede no ser obvio, es realmente una característica útil.

Imagine este escenario: un programa requiere el uso de un recurso compartido de algún tipo contenido en un archivo llamado "foo", pero "foo" tiene cambios de versión frecuentes. Sería bueno incluir el número de versión en el nombre del archivo para que el administrador u otra parte interesada pueda ver qué versión de "foo" está instalada. Esto presenta un problema. Si cambiamos el nombre del recurso compartido, tenemos que rastrear cada programa que pueda usarlo y cambiarlo para buscar un nuevo nombre de recurso cada vez que se instala una nueva versión del recurso. Eso no suena divertido en absoluto.

Aquí es donde los enlaces simbólicos salvan el día. Supongamos que instalamos la versión 2.6 de "foo", que tiene el nombre de archivo "foo-2.6", y luego creamos un enlace simbólico simplemente llamado "foo" que apunta a "foo-2.6". Esto significa que cuando un programa abre el archivo " foo ", en realidad está abriendo el archivo" foo-2.6 ". Ahora todos están contentos. Los programas que dependen de "foo" pueden encontrarlo, y aún podemos ver qué versión real está instalada. Cuando llega el momento de actualizar a "foo-2.7", simplemente agregamos el archivo a nuestro sistema, eliminamos el enlace simbólico "foo" y creamos uno nuevo que apunta a la nueva versión. Esto no solo resuelve el problema de la actualización de la versión, sino que también nos permite mantener ambas versiones en nuestra máquina. Imagina que "foo-2.7" tiene un error (¡malditos sean los desarrolladores!), Y necesitamos volver a la versión anterior. De nuevo,

La lista de directorios al comienzo de esta sección (del directorio / lib de un sistema Fedora) muestra un enlace simbólico llamado libc.so.6 que apunta a un archivo de biblioteca compartida llamado libc-2.6.so . Esto significa que los programas que buscan libc.so.6 realmente obtendrán el archivo libc-2.6.so . Aprenderemos cómo crear enlaces simbólicos en el próximo capítulo.

## ENLACES DUROS

Mientras estamos en el tema de los enlaces, debemos mencionar que hay un segundo tipo de enlace llamado enlaces duros . Los enlaces duros también permiten que los archivos tengan múltiples nombres, pero lo hacen de una manera diferente. Hablaremos más sobre las diferencias entre enlaces simbólicos y duros en el próximo capítulo.

## RESUMIENDO

Con nuestro recorrido detrás de nosotros, hemos aprendido mucho sobre nuestro sistema. Hemos visto varios archivos y directorios y sus contenidos. Una cosa que debes sacar de esto es lo abierto que es el sistema. En Linux hay muchos archivos importantes que son texto legible para humanos. A diferencia de muchos sistemas propietarios, Linux pone todo a disposición para su examen y estudio.

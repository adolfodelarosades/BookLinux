# 2 NAVEGACIÓN

Lo primero que debemos aprender (además de cómo escribir) es cómo navegar por el sistema de archivos en nuestro sistema Linux. En este capítulo, presentaremos los siguientes comandos:

`pwd` Imprime el nombre del directorio de trabajo actual

`cd` Cambiar directorio

`ls` contenidos de listas

## COMPRENDER EL ÁRBOL DEL SISTEMA DE ARCHIVOS

Al igual que Windows, un sistema operativo similar a Unix como Linux organiza sus archivos en lo que se denomina una *estructura de directorio jerárquico*. Esto significa que están organizados en un patrón de directorios similar a un árbol (a veces llamados carpetas en otros sistemas), que pueden contener archivos y otros directorios. El primer directorio en el sistema de archivos se llama *directorio raíz*. El directorio raíz contiene archivos y subdirectorios, que contienen más archivos y subdirectorios, etc.

Tenga en cuenta que, a diferencia de Windows, que tiene un árbol de sistema de archivos separado para cada dispositivo de almacenamiento, los sistemas tipo Unix como Linux siempre tienen un solo árbol de sistema de archivos, independientemente de cuántas unidades o dispositivos de almacenamiento estén conectados a la computadora. Los dispositivos de almacenamiento se conectan (o más correctamente, se *montan*) en varios puntos del árbol de acuerdo con los caprichos del *administrador del sistema*, la persona (o personas) responsables del mantenimiento del sistema.

## EL DIRECTORIO DE TRABAJO ACTUAL

La mayoría de nosotros probablemente estamos familiarizados con un administrador de archivos gráfico que representa el árbol del sistema de archivos, como se ilustra en la Figura 2-1.

<img src="/images/directorios.png">

*Figura 2-1: Árbol del sistema de archivos como lo muestra un administrador de archivos gráfico*

Observe que el árbol generalmente se muestra volteado, es decir, con la raíz en la parte superior y las diversas ramas descendiendo hacia abajo.

Sin embargo, la línea de comando no tiene imágenes, por lo que para navegar por el árbol del sistema de archivos, debemos pensar de otra manera.

Imagine que el sistema de archivos es un laberinto con forma de árbol invertido y que podemos estar en medio de él. En cualquier momento, estamos dentro de un único directorio, y podemos ver los archivos contenidos en el directorio y la ruta al directorio que está encima de nosotros (llamado *directorio padre*) y cualquier subdirectorio debajo de nosotros. El directorio en el que estamos parados se llama el *directorio de trabajo actual*. Para mostrar el directorio de trabajo actual, usamos el comando `pwd` (imprimir directorio de trabajo).

```sh
[me@linuxbox ~]$ pwd
/home/me

[192:clientes-app adolfodelarosa$ pwd
/Users/adolfodelarosa/Documents/Udemy2020/Cursos/AngularYSpring5/Codigo/clientes-app
```

Cuando iniciamos sesión por primera vez en nuestro sistema (o iniciamos una sesión de emulador de terminal), nuestro directorio de trabajo actual se establece en nuestro *directorio home (de inicio)*. Cada cuenta de usuario tiene su propio directorio de inicio, y es el único lugar donde un usuario normal puede escribir archivos.

## LISTADO DE LOS CONTENIDOS DE UN DIRECTORIO

Para enumerar los archivos y directorios en el directorio de trabajo actual, utilizamos el comando ls .

```sh
[me@linuxbox ~]$ ls
Desktop  Documents  Music  Pictures  Public  Templates  Videos

192:clientes-app adolfodelarosa$ ls
README.md				                              browserslist				package.json
Seccion02_PrimerosPasosEnAngular.md	          e2e					        src
Seccion03_ComponenteClientes.md		            images					    tsconfig.app.json
Seccion04_Backend_SpringAPI_REST.md	          karma.conf.js				tsconfig.json
Seccion05_CRUDconSpringAPIRest.md	            node_modules				tsconfig.spec.json
angular.json				                          package-lock.json		tslint.json
```

En realidad, podemos usar el comando `ls` para enumerar el contenido de cualquier directorio, no solo el directorio de trabajo actual, y hay muchas otras cosas divertidas que también puede hacer. Pasaremos más tiempo con ls en el Capítulo 3 .

## CAMBIAR EL DIRECTORIO DE TRABAJO ACTUAL

Para cambiar nuestro directorio de trabajo (donde estamos parados en el laberinto en forma de árbol), usamos el comando `cd`. Para hacer esto, escriba `cd` seguido de la ruta del directorio de trabajo deseado. Un nombre de ruta es la ruta que tomamos a lo largo de las ramas del árbol para llegar al directorio que queremos. Podemos especificar nombres de ruta de una de dos maneras diferentes: como nombres de *ruta absolutos* o como *ruta relativos* (*absolute pathnames* or *relative pathnames*). Tratemos primero con los nombres de ruta absolutos.

### Absolute Pathnames (Nombres de ruta absolutos)

Un nombre de ruta absoluto comienza con el directorio raíz y sigue el árbol rama por rama hasta que se completa la ruta al directorio o archivo deseado. Por ejemplo, hay un directorio en su sistema en el que están instalados la mayoría de los programas del sistema. La ruta del directorio es `/usr/bin`. Esto significa que desde el directorio raíz (representado por la barra inclinada en la ruta) hay un directorio llamado `usr` que contiene un directorio llamado `bin`.

```sh
[me@linuxbox ~]$ cd /usr/bin
[me@linuxbox bin]$ pwd
/usr/bin
[me@linuxbox bin]$ ls
...Listing of many, many files ...


[192:clientes-app adolfodelarosa$ cd /usr/bin
[192:bin adolfodelarosa$ pwd
/usr/bin
[192:bin adolfodelarosa$ ls
...Listing of many, many files ...
```
Ahora podemos ver que hemos cambiado el directorio de trabajo actual a `/usr/bin` y que está lleno de archivos. ¿Te das cuenta de cómo ha cambiado el indicador de shell? Para su comodidad, generalmente está configurado para mostrar automáticamente el nombre del directorio de trabajo.

## Relative Pathnames (Nombres de ruta relativos)

Cuando un nombre de ruta absoluto comienza desde el directorio raíz y conduce a su destino, un nombre de ruta relativo comienza desde el directorio de trabajo. Para hacer esto, utiliza un par de notaciones especiales para representar posiciones relativas en el árbol del sistema de archivos. Estas notaciones especiales son . (punto) y .. (punto punto).

El notación . se refiere al directorio de trabajo, y la notación .. se refiere al directorio padre del directorio de trabajo. Así es como funciona. Cambiemos el directorio de trabajo a `/usr/bin` nuevamente.

```sh
[me@linuxbox ~]$ cd /usr/bin
[me@linuxbox bin]$ pwd
/usr/bin

[192:bin adolfodelarosa$ cd /usr/bin
[192:bin adolfodelarosa$ pwd
/usr/bin
```


Ahora digamos que queríamos cambiar el directorio de trabajo al padre de / usr / bin , que es / usr . Podríamos hacer eso de dos maneras diferentes, ya sea con un nombre de ruta absoluto:

[me @ linuxbox bin] $ cd / usr
[me @ linuxbox usr] $ pwd
/ usr

o con un nombre de ruta relativo:

[me @ linuxbox bin] $ cd ..
[me @ linuxbox usr] $ pwd
/ usr

Dos métodos diferentes con resultados idénticos. ¿Cuál deberíamos usar? ¡El que requiere menos tipeo!

Del mismo modo, podemos cambiar el directorio de trabajo de / usr a / usr / bin de dos maneras diferentes, ya sea utilizando un nombre de ruta absoluto:

[me @ linuxbox usr] $ cd / usr / bin
[me @ linuxbox bin] $ pwd
/ usr / bin

o usando un nombre de ruta relativo:

[me @ linuxbox usr] $ cd ./bin
[me @ linuxbox bin] $ pwd
/ usr / bin

Ahora, hay algo importante que señalar aquí. En casi todos los casos, podemos omitir la parte ./ porque está implícita. Escribir lo siguiente hace lo mismo:

[yo @ linuxbox usr] $ cd bin

En general, si no especificamos un nombre de ruta a algo, se asumirá el directorio de trabajo.

HECHOS IMPORTANTES SOBRE LOS NOMBRES DE ARCHIVO

En los sistemas Linux, los archivos se nombran de manera similar a la de otros sistemas como Windows, pero existen algunas diferencias importantes.

Los nombres de archivo que comienzan con un carácter de punto están ocultos. Esto solo significa que ls no los enumerará a menos que usted diga ls -a . Cuando se creó su cuenta, se colocaron varios archivos ocultos en su directorio de inicio para configurar cosas para su cuenta. En el Capítulo 11, veremos más de cerca algunos de estos archivos para ver cómo puede personalizar su entorno. Además, algunas aplicaciones colocan sus archivos de configuración y configuración en su directorio de inicio como archivos ocultos.
Los nombres de archivo y comandos en Linux, como Unix, distinguen entre mayúsculas y minúsculas. Los nombres de archivo File1 y file1 se refieren a diferentes archivos.
Aunque Linux admite nombres de archivo largos que pueden contener espacios incrustados y caracteres de puntuación, limite los caracteres de puntuación en los nombres de los archivos que cree a punto, guión y guión bajo. Lo más importante, no incrustar espacios en los nombres de archivo. Si desea representar espacios entre palabras en un nombre de archivo, use caracteres de subrayado. Te lo agradecerás más tarde.
Linux no tiene el concepto de una "extensión de archivo" como algunos otros sistemas operativos. Puede nombrar archivos de la forma que desee. El contenido o el propósito de un archivo se determina por otros medios. Aunque los sistemas operativos tipo Unix no usan extensiones de archivo para determinar el contenido / propósito de los archivos, muchos programas de aplicación lo hacen.
Algunos atajos útiles
La Tabla 2-1 muestra algunas formas útiles de cambiar rápidamente el directorio de trabajo actual.

Tabla 2-1: Atajos de CD

Atajo

Resultado

discos compactos

Cambia el directorio de trabajo a su directorio de inicio.

discos compactos -

Cambia el directorio de trabajo al directorio de trabajo anterior.

cd ~ nombre_usuario

Cambia el directorio de trabajo al directorio de inicio de user_name . Por ejemplo, escribir cd ~ bob cambiará el directorio al directorio de inicio del usuario "bob".

RESUMIENDO
Este capítulo explica cómo el shell trata la estructura de directorios del sistema. Aprendimos sobre nombres de ruta absolutos y relativos y los comandos básicos que usamos para movernos alrededor de esa estructura. En el próximo capítulo, utilizaremos este conocimiento para realizar un recorrido por un sistema Linux moderno.

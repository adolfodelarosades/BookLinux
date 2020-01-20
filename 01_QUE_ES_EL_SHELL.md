# 1 ¿QUÉ ES EL SHELL?

Cuando hablamos de la línea de comando, realmente nos estamos refiriendo al *shell*. El shell es un programa que toma comandos del teclado y los pasa al sistema operativo para llevarlos a cabo. Casi todas las distribuciones de Linux suministran un programa shell del Proyecto GNU llamado `bash`. El nombre es un acrónimo de *b*ourne-*a*gain *sh*ell, una referencia al hecho de que `bash`  es un reemplazo mejorado para `sh`, el programa original Unix shell escrito por Steve Bourne.

## EMULADORES DE TERMINAL

Cuando se usa una interfaz gráfica de usuario (GUI), necesitamos otro programa llamado *emulador de terminal* para interactuar con el shell. Si miramos a través de nuestros menús de escritorio, probablemente encontraremos uno. KDE usa konsole y GNOME usa gnome-terminal, aunque probablemente se llame simplemente Terminal en su menú. Hay varios otros emuladores de terminal disponibles para Linux, pero todos básicamente hacen lo mismo: darnos acceso al shell. Probablemente desarrolle una preferencia por uno u otro emulador de terminal en función del número de campanas y silbatos que tenga.

## HACIENDO SUS PRIMERAS PULSACIONES DE TECLAS

Entonces empecemos. Inicia el emulador de terminal. Una vez que aparezca, deberíamos ver algo como esto:

```
[me@linuxbox ~]$
```

Esto se llama un *shell prompt (indicador de comandos de shell)* y aparecerá cada vez que el shell esté listo para aceptar entradas. Si bien puede variar en apariencia, dependiendo de la distribución, generalmente incluirá su *username@machinename*, seguido del directorio de trabajo actual (más sobre eso en un momento) y un signo de dólar.

Si el último carácter de la solicitud es una marca hash ( # ) en lugar de un signo de dólar, la sesión de terminal tiene privilegios de superusuario. Esto significa que hemos iniciado sesión como usuario root o hemos seleccionado un emulador de terminal que proporciona privilegios de superusuario (administrativos).

Suponiendo que las cosas estén bien hasta ahora, intentemos escribir. Ingrese algunas galimatías en el indicador de la siguiente manera:

```sh
[me@linuxbox ~]$ kaekfjaeifj
```

Debido a que este comando no tiene sentido, el shell nos lo dice y nos da otra oportunidad.

```linux
bash: kaekfjaeifj: command not found
[me@linuxbox ~]$
```

### Command History

Si presionamos la flecha hacia arriba, veremos que el comando anterior ingresado, kaekfjaeifj , vuelve a aparecer después de la solicitud. Esto se llama historial de comandos . La mayoría de las distribuciones de Linux recuerdan los últimos 1,000 comandos por defecto. Presione la flecha hacia abajo y el comando anterior desaparece.

Movimiento del cursor
Recupere el comando anterior presionando la flecha hacia arriba nuevamente. Si intentamos las flechas izquierda y derecha, veremos que podemos colocar el cursor en cualquier lugar de la línea de comando. Esto facilita la edición de comandos.

Algunas palabras sobre ratones y enfoque

Si bien el shell se trata del teclado, también puede usar un mouse con su emulador de terminal. Un mecanismo integrado en el sistema X Window (el motor subyacente que hace funcionar la GUI) admite una técnica rápida de copiar y pegar. Si resalta un texto manteniendo presionado el botón izquierdo del mouse y arrastrando el mouse sobre él (o haciendo doble clic en una palabra), se copia en un búfer mantenido por X. Al presionar el botón central del mouse, el texto se pegará en La ubicación del cursor. Intentalo.

No caigas en la tentación de usar CTRL -C y CTRL -V para copiar y pegar dentro de una ventana de terminal. Ellos no trabajan Estos códigos de control tienen diferentes significados para el shell y se asignaron muchos años antes del lanzamiento de Microsoft Windows.

Su entorno de escritorio gráfico (muy probablemente KDE o GNOME), en un esfuerzo por comportarse como Windows, probablemente tenga su política de enfoque establecida en "hacer clic para enfocar". Esto significa que para que una ventana se enfoque (se active), debe hacer clic en eso. Esto es contrario al comportamiento tradicional de X de "el foco sigue al mouse", lo que significa que una ventana obtiene el foco simplemente pasando el mouse sobre ella. La ventana no aparecerá en primer plano hasta que haga clic en ella, pero podrá recibir información. Establecer la política de enfoque en "el foco sigue al mouse" hará que la técnica de copiar y pegar sea aún más útil. Pruébelo si puede (algunos entornos de escritorio como la Unidad de Ubuntu ya no lo admiten). Creo que si le das una oportunidad, la preferirás. Encontrará esta configuración en el programa de configuración de su administrador de ventanas.

PRUEBE ALGUNOS COMANDOS SIMPLES
Ahora que hemos aprendido a ingresar texto en el emulador de terminal, intentemos algunos comandos simples. Comencemos con el comando de fecha , que muestra la hora y fecha actuales.

[yo @ linuxbox ~] $ fecha
viernes 2 de febrero 15:09:41 EST 2018

Un comando relacionado es cal , que, de forma predeterminada, muestra un calendario del mes actual.

[me @ linuxbox ~] $ cal
    Febrero 2018   
Su Mo Tu We Th Fr Sa
             1 2 3
 4 5 6 7 8 9 10
11 12 13 14 15 16 17
18 19 20 21 22 23 24
25 26 27 28      

LA CONSOLA DETRÁS DE LA CORTINA

Incluso si no tenemos ningún emulador de terminal en ejecución, varias sesiones de terminal continúan ejecutándose detrás del escritorio gráfico. Podemos acceder a estas sesiones, llamadas consolas virtuales , presionando CTRL - ALT -F1 a través de CTRL - ALT -F6 en la mayoría de las distribuciones de Linux. Cuando se accede a una sesión, presenta un mensaje de inicio de sesión en el que podemos ingresar nuestro nombre de usuario y contraseña. Para cambiar de una consola virtual a otra, presione ALT -F1 a través de ALT -F6. En la mayoría de los sistemas, podemos volver al escritorio gráfico presionando ALT -F7.

Para ver la cantidad actual de espacio libre en nuestras unidades de disco, ingrese df .

[me @ linuxbox ~] $ df
Sistema de archivos 1K-blocks Usado Disponible Uso% Montado en
/ dev / sda2 15115452 5012392 9949716 34% /
/ dev / sda5 59631908 26545424 30008432 47% / home
/ dev / sda1 147764 17370 122765 13% / boot
tmpfs 256856 0 256856 0% / dev / shm

Del mismo modo, para mostrar la cantidad de memoria libre, ingrese el comando libre .

[me @ linuxbox ~] $ gratis
         total usado libre almacenado en
memoria intermedia almacenado en caché Mem: 513712 503976 9736 0 5312 122916
- / + almacenamientos intermedios / caché: 375748 137964
Intercambio: 1052248 104712 947536

FINALIZAR UNA SESIÓN DE TERMINAL
Podemos finalizar una sesión de terminal cerrando la ventana del emulador de terminal, ingresando el comando de salida en el indicador de comandos de la shell o presionando CTRL -D.

[yo @ linuxbox ~] $ salir

RESUMIENDO
Este capítulo marcó el comienzo de nuestro viaje hacia la línea de comandos de Linux, con una introducción al shell, un vistazo a la línea de comandos y una breve lección sobre cómo iniciar y finalizar una sesión de terminal. También vimos cómo emitir algunos comandos simples y realizar una pequeña edición de línea de comandos. Eso no fue tan aterrador, ¿verdad?

En el próximo capítulo, aprenderemos algunos comandos más y recorreremos el sistema de archivos Linux.

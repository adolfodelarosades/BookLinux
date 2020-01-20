# INTRODUCCIÓN

Quiero contarte una historia. No la historia de cómo, en 1991, Linus Torvalds escribió la primera versión del kernel de Linux. Puedes leer esa historia en muchos libros de Linux. Tampoco voy a contarte la historia de cómo, algunos años antes, Richard Stallman comenzó el Proyecto GNU para crear un sistema operativo gratuito similar a Unix. Esa es una historia importante también, pero la mayoría de los otros libros de Linux también tienen esa.

No, quiero contarte la historia de cómo retomas el control de tu computadora.

Cuando comencé a trabajar con computadoras como estudiante universitario a fines de la década de 1970, estaba ocurriendo una revolución. La invención del microprocesador ha hecho posible que personas comunes como usted y yo poseamos una computadora. Para muchas personas hoy es difícil imaginar cómo sería el mundo cuando solo las grandes empresas y el gran gobierno manejaban todas las computadoras. Digamos que no se puede hacer mucho.

Hoy, el mundo es muy diferente. Las computadoras están en todas partes, desde pequeños relojes de pulsera hasta centros de datos gigantes y todo lo demás. Además de las computadoras ubicuas, también tenemos una red ubicua que las conecta entre sí. Esto ha creado una maravillosa nueva era de empoderamiento personal y libertad creativa, pero en las últimas décadas ha sucedido algo más. Algunas corporaciones gigantes han estado imponiendo su control sobre la mayoría de las computadoras del mundo y decidiendo qué puedes hacer con ellas. Afortunadamente, personas de todo el mundo están haciendo algo al respecto. Están luchando para mantener el control de sus computadoras escribiendo su propio software. Están construyendo Linux.

Mucha gente habla de "libertad" con respecto a Linux, pero no creo que la mayoría de la gente sepa lo que realmente significa esta libertad. La libertad es el poder de decidir qué hace su computadora, y la única forma de tener esta libertad es saber qué está haciendo su computadora. Freedom es una computadora sin secretos, una en la que todo se puede saber si te importa lo suficiente como para descubrirlo.

## ¿POR QUÉ USAR LA LÍNEA DE COMANDO?

¿Alguna vez has notado en las películas cuando el "superhacker", ya sabes, el tipo que puede entrar en la computadora militar ultrasegura en menos de 30 segundos, se sienta en la computadora, nunca toca un mouse? ¡Es porque los cineastas se dan cuenta de que nosotros, como seres humanos, instintivamente sabemos que la única forma de hacer algo en una computadora es escribiendo en un teclado!

La mayoría de los usuarios de computadoras hoy están familiarizados solo con *la interfaz gráfica de usuario ( GUI )* y los vendedores y expertos les han enseñado que *la interfaz de línea de comandos ( CLI )* es algo aterrador del pasado. Esto es lamentable porque una buena interfaz de línea de comandos es una forma maravillosamente expresiva de comunicarse con una computadora de la misma manera que la palabra escrita es para los seres humanos. Se ha dicho que "las interfaces gráficas de usuario facilitan las tareas, mientras que las interfaces de línea de comandos hacen posibles las tareas difíciles", y esto todavía es muy cierto hoy en día.

Dado que Linux se basa en la familia de sistemas operativos Unix, comparte la misma herencia de herramientas de línea de comandos que Unix. Unix se hizo prominente a principios de la década de 1980 (aunque se desarrolló por primera vez una década antes), antes de la adopción generalizada de la interfaz gráfica de usuario y, como resultado, desarrolló una amplia interfaz de línea de comandos. De hecho, una de las razones más fuertes por las que los primeros usuarios de Linux lo eligieron, por ejemplo, Windows NT fue la poderosa interfaz de línea de comandos que hizo posible las "tareas difíciles".

## DE QUÉ TRATA ESTE LIBRO

Este libro es una visión general de "vivir" en la línea de comandos de Linux. A diferencia de algunos libros que se concentran en un solo programa, como el programa shell `bash`, este libro intentará transmitir cómo llevarse bien con la interfaz de línea de comando (command line interface) en un sentido más amplio. ¿Cómo funciona todo? ¿Qué puede hacer? ¿Cuál es la mejor manera de usarlo?

**Este no es un libro sobre la administración del sistema Linux**. Si bien cualquier discusión seria sobre la línea de comando invariablemente conducirá a temas de administración del sistema, este libro aborda solo unos pocos problemas de administración. Sin embargo, preparará al lector para un estudio adicional al proporcionar una base sólida en el uso de la línea de comando, una herramienta esencial para cualquier tarea seria de administración del sistema.

**Este libro está centrado en Linux**. Muchos otros libros intentan ampliar su atractivo al incluir otras plataformas como Unix genérico y macOS. Al hacerlo, "diluyen" su contenido para presentar solo temas generales. Este libro, por otro lado, cubre solo las distribuciones contemporáneas de Linux. El noventa y cinco por ciento del contenido es útil para los usuarios de otros sistemas similares a Unix, pero este libro está muy dirigido al usuario moderno de la línea de comandos de Linux.

## QUIÉN DEBERÍA LEER ESTE LIBRO

Este libro es para nuevos usuarios de Linux que han migrado desde otras plataformas. Lo más probable es que sea un "usuario avanzado" de alguna versión de Microsoft Windows. Tal vez su jefe le ha dicho que administre un servidor Linux, o está entrando en el nuevo y emocionante mundo de las computadoras de placa única (SBC) como la Raspberry Pi. Es posible que solo sea un usuario de escritorio que esté cansado de todos los problemas de seguridad y quiera probar Linux. Esta bien. Todos son bienvenidos aquí.

Dicho esto, no hay un atajo para la iluminación de Linux. Aprender la línea de comando es un desafío y requiere un esfuerzo real. No es que sea tan difícil, sino que es tan vasto . El sistema Linux promedio tiene literalmente miles de programas que puede emplear en la línea de comando. Considérate advertido; aprender la línea de comando no es un esfuerzo casual.

Por otro lado, aprender la línea de comandos de Linux es extremadamente gratificante. Si crees que eres un "usuario avanzado" ahora, solo espera. Aún no sabes qué es el poder real. Y, a diferencia de muchas otras habilidades informáticas, el conocimiento de la línea de comando es duradero. Las habilidades aprendidas hoy seguirán siendo útiles dentro de 10 años. La línea de comando ha sobrevivido a la prueba del tiempo.

También se supone que no tienes experiencia en programación, pero no te preocupes, también te guiaremos por ese camino.

## ¿QUÉ HAY EN ESTE LIBRO?

Este material se presenta en una secuencia cuidadosamente elegida, muy similar a un tutor sentado a su lado que lo guía. Muchos autores tratan este material de manera "sistemática", cubriendo exhaustivamente cada tema en orden. Esto tiene sentido desde la perspectiva de un escritor, pero puede ser muy confuso para los nuevos usuarios.

Otro objetivo es familiarizarlo con la forma de pensar de Unix, que es diferente de la forma de pensar de Windows. En el camino, realizaremos algunos viajes paralelos para ayudarlo a comprender por qué ciertas cosas funcionan de la manera en que hacer y cómo llegaron de esa manera. Linux no es solo una pieza de software; También es una pequeña parte de la cultura Unix más grande, que tiene su propio idioma e historia. También podría lanzar un parloteo o dos.

Este libro está dividido en cuatro partes, cada una de las cuales cubre algunos aspectos de la experiencia de la línea de comandos.

**La Parte 1, "Aprendiendo el Shell"**, comienza nuestra exploración del lenguaje básico de la línea de comandos, incluyendo elementos como la estructura de los comandos, la navegación del sistema de archivos, la edición de la línea de comandos y la búsqueda de ayuda y documentación para los comandos.

**La Parte 2, "Configuración y entorno"**, cubre la edición de archivos de configuración que controlan el funcionamiento de la computadora desde la línea de comandos.

**La Parte 3, “Tareas comunes y herramientas esenciales”**, explora muchas de las tareas ordinarias que se realizan comúnmente desde la línea de comandos. Los sistemas operativos tipo Unix, como Linux, contienen muchos programas de línea de comandos "clásicos" que se utilizan para realizar operaciones potentes en los datos.

**La Parte 4, “Escritura de scripts de Shell”**, presenta la programación de shell, una técnica ciertamente rudimentaria pero fácil de aprender para automatizar muchas tareas informáticas comunes. Al aprender la programación de shell, se familiarizará con los conceptos que se pueden aplicar a muchos otros lenguajes de programación.

## CÓMO LEER ESTE LIBRO

Comienza al principio del libro y síguelo hasta el final. No está escrito como un trabajo de referencia; Es realmente más como una historia con un principio, un medio y un final.

### Prerrequisitos

Para usar este libro, todo lo que necesitará es una instalación de Linux que funcione. Puede obtener esto de dos maneras:

**Instale Linux en una computadora (no tan nueva)**. No importa qué distribución elija, aunque la mayoría de las personas hoy en día comienzan con Ubuntu, Fedora u OpenSUSE. En caso de duda, pruebe Ubuntu primero. Instalar una distribución moderna de Linux puede ser ridículamente fácil o ridículamente difícil dependiendo de su hardware. Sugiero una computadora de escritorio que tenga un par de años y que tenga al menos 2 GB de RAM y 6 GB de espacio libre en el disco duro. Evite las computadoras portátiles y las redes inalámbricas si es posible, ya que a menudo son más difíciles de conseguir.

**Utilice un “live CD” o USB flash drive**. Una de las mejores cosas que puede hacer con muchas distribuciones de Linux es ejecutarlas directamente desde un CD-ROM o unidad flash USB sin instalarlas. Simplemente vaya a la configuración de su BIOS y configure su computadora para que arranque desde una unidad de CD-ROM o dispositivo USB y reinicie. Usar este método es una excelente manera de probar una computadorapara compatibilidad con Linux antes de la instalación. La desventaja es que puede ser lento en comparación con tener Linux instalado en su disco duro. Tanto Ubuntu como Fedora (entre otros) tienen versiones en vivo.

Independientemente de cómo instale Linux, necesitará tener privilegios ocasionales de superusuario (es decir, administrativos) para llevar a cabo las lecciones de este libro.

Después de tener una instalación que funcione, comience a leer y siga junto con su propia computadora. La mayor parte del material de este libro es "práctico", ¡así que siéntate y comienza a escribir!

```
## POR QUÉ NO LO LLAMO "GNU/LINUX"

En algunos sectores, es políticamente correcto llamar al sistema operativo Linux el "sistema operativo GNU/Linux". El problema con "Linux" es que no hay una forma completamente correcta de nombrarlo porque fue escrito por muchas personas diferentes en un vasto, esfuerzo de desarrollo distribuido. Técnicamente hablando, Linux es el nombre del núcleo del sistema operativo, nada más. El núcleo es muy importante, por supuesto, ya que hace que el sistema operativo funcione, pero no es suficiente para formar un sistema operativo completo.

Ingrese Richard Stallman, el genio-filósofo que fundó el movimiento de Software Libre, comenzó la Free Software Foundation, formó el Proyecto GNU, escribió la primera versión del Compilador C GNU (gcc), creó la Licencia Pública General de GNU (GPL), etc., etc., etc. Él insiste en que lo llame "GNU/Linux" para reflejar adecuadamente las contribuciones del Proyecto GNU. Si bien el Proyecto GNU es anterior al kernel de Linux y las contribuciones del proyecto son extremadamente merecedoras de reconocimiento, colocarlas en el nombre es injusto para todos los demás que hicieron contribuciones significativas. Además, creo que "Linux/GNU" sería más preciso desde el punto de vista técnico, ya que el núcleo se inicia primero y todo lo demás se ejecuta sobre él.

En el uso popular, Linux se refiere al kernel y al resto del software libre y de código abierto que se encuentra en la distribución típica de Linux, es decir, todo el ecosistema de Linux, no solo los componentes de GNU. El mercado del sistema operativo parece preferir nombres de una palabra como DOS, Windows, macOS, Solaris, Irix y AIX. Elegí usar el formato popular. Sin embargo, si prefiere usar "GNU/Linux" en su lugar, realice una búsqueda y reemplazo mental mientras lee este libro.
```

## LO NUEVO EN LA SEGUNDA EDICIÓN

Si bien la estructura básica y el contenido siguen siendo los mismos, esta edición de The Linux Command Line está salpicada de varios refinamientos, aclaraciones y modernizaciones, muchos de los cuales se basan en los comentarios de los lectores. Además, se destacan dos mejoras particulares. Primero, el libro ahora asume la versión bash 4.x , que no se usaba ampliamente en el momento del manuscrito original. Esta cuarta versión principal de bash agregó varios características útiles nuevas  ahora cubiertas en esta edición. En segundo lugar, la Parte 4 , "Shell Scripting", se ha mejorado para proporcionar mejores ejemplos de buenas prácticas de scripting. Las secuencias de comandos incluidas en la Parte 4 se han revisado para que sean más robustas, y también solucioné algunos errores ;-).

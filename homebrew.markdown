## Homebrew

Una de las herramientas que como desarrolladores, sysadmins, o simplemente usuarios adeptos debemos conocer es la línea de comandos. MacOS X es un Unix de pura cepa, y como tal, puede ejecutar una amplia gama de herramientas pensadas para llevar a cabo toda clase de tareas.

Sin embargo, pocas de estas herramientas se encuentran disponibles de serie en nuestros sistemas. Para poder utilizarlas, debemos instalarlas manualmente, lo cual es cuando menos engorroso, y cuando más, puede significar horas y horas perdidas tratando de resolver dependencias de las herramientas que utilizamos.

Para solucionar este problema, nacieron los gestores de paquetes. Del mismo modo que en linux disponemos de `rpm` y `dpkg` (o `yum` y `apt`), en MacOS X existen también varios sistemas de gestión de paquetes. En este artículo me centraré en el que considero más avanzado y más útil de ellos, **Homebrew**, ya que las alternativas, mayormente **Macports** (inspirado en el sistema de ports de FreeBSD) y **dfink** (inspirado a su vez en apt), han ido poco a poco quedándose atrás en cuanto a frecuencia de actualización y catálogo de software disponible. Homebrew es obra del prolífico [Max Howell](http://methylblue.com/), autor entre otras herramientas de las versiones Android e iOS de [TweetDeck](http://www.tweetdeck.com/), y de los clientes de [last.fm](http://last.fm).  

Homebrew es un sistema  que goza de las simpatías de muchos desarrolladores y, por tanto, de un ímpetu, que lo ha convertido rápidamente en una elección muy popular entre los usuarios más técnicos. No en vano es el proyecto con [mayor número de forks en GitHub](https://github.com/popular/forked) (hecho que, al contrario de lo que pueda parecer, no es un indicativo de fragmentación, si no de participación popular, ya que cada persona que quiere añadir un paquete, o actualizarlo, simplemente crea un fork, añade su *receta*, y envía una solicitud de *pull* al mantenedor para que lo incorpore al proyecto principal).

Una de las principales características que tiene homebrew frente a otros gestores de proyectos, es que no necesitas ser root para instalar un paquete. Homebrew normalmente instala los ejecutables en `/usr/local/`, nada de `/opt/`, `/sw/` ni tonterías similares. Y por tanto, espera que ese directorio sea escribible sin permisos de root. Al respecto ha habido cierta controversia, a mi al menos al principio no me parecía del todo correcto. Pero si nos ponemos a mirar, veremos que casi cualquier aplicación que bajamos de internet, y que se instale a pelo, es decir sin instalador, la copiamos tranquilamente a `/Applications/` sin pensárnoslo dos veces. Y al contrario, cuando ejecutamos un script con `sudo`, estamos corriendo más riesgo del que pensamos.

Para instalar homebrew, simpemente vamos a [https://github.com/mxcl/homebrew](https://github.com/mxcl/homebrew) y vemos que hay que ejecutar un pequeño script en ruby:

    /usr/bin/ruby -e "$(curl -fsSL https://raw.github.com/gist/323731)"

Si ejecutar un script obtenido a partir de una URL te pone los pelos de punta (debería), puedes echar un vistazo primero.

Una vez hecho esto, ya tenemos homebrew listo para instalar paquetes. Podemos probarlo de la siguiente manera:

    brew install wget

Esto instruirá a brew a descargarse el código fuente de la utilidad **wget**, compilarlo, e instalarlo en `/usr/local/`. Homebrew en este sentido, es más parecido a los *ports* o al *emerge* de Gentoo. No lo he mencionado, dado que doy por sentado que todos tenemos Xcode, pero evidentemente necesitaremos las herramientas de desarrollo, no necesariamente Xcode al completo, pero si los paquetes de compiladores y demás.

Por cierto, por algún extraño motivo, en MacOS X, `/usr/local/bin` no está antes en el `$PATH` que `/usr/local/`, con lo cual, si instalamos alguna herramienta de las que vengan con el sistema o con Xcode, nos vamos a encontrar que la del sistema tiene prioridad sobre la versión que nosotros hemos instalado explícitamente (con lo cual, es como si no hubieramos hecho nada). En cualquier otro sistema UNIX (o linux), lo normal es que las customizaciones tomen prioridad, así que no acabo de entender muy bien esta política. Podemos solucionar esto de dos maneras, editando nuestro fichero `~/.bash_profile` para modificar la variable de entorno `$PATH`, o bien cambiando el orden en `/etc/paths` (lo cual va a requerir permisos de root, al aplicarse a todo el sistema).

Las definiciones de los paquetes en homebrew siguen la metáfora de una cervecera. Cada paquete es una *receta* o *fórmula* para elaborar un… bueno, en castellano no tenemos una buena traducción para *brew*, en el sentido cervecero. Podríamos abusar del lenguaje y usar la palabra caldo, que los vinateros usan, pero lo dejaremos como *brew*. *To brew* en inglés es también un verbo, que es lo que representa el comando que usamos.

En fin, después de esta digresión, paso a enumerar los comandos más habituales de que disponemos para gestionar los paquetes. Lo mejor es que echemos un vistazo al manual de `brew` y los veamos cuando tengamos dudas:


    brew search paquete

Este comando buscará un match en los nombres de los paquetes y nos listará los que coincidan (si el nombre de paquete está cerrado entre barras, se interpretará como una expresión regular). Una vez creemos haber encontrado el que nos puede interesar, lo podemos comprobar con

    brew home paquete

Esto nos abrirá el navegador por defecto que tengamos en el sistema, en la página web del producto en cuestión. Estamos en el s. XXI y usamos Macs, así que no hay excusa para andarnos con cosas que recuerden a `gnu info`. Cualquier producto debería tener un website canónico.

    brew install paquete

Este comando instalará el paquete, bajándose el código fuente si no está cacheado, y compilándolo. Del mismo modo,

    brew remove paquete

lo eliminará del sistema. Si necesitamos saber qué fórmulas tenemos instaladas, pediremos un listado:

    brew list

Si a este comando le añadimos además el nombre de una fórmula, lo que veremos son los ficheros instalados por esa fórmula.

Cuando queramos actualizar los paquetes, primero ejecutaremos

    brew update

que actualizará brew y sus fórmulas, y luego podemos hacer algo como

    brew install $(brew outdated)

para que nos instale las fórmulas nuevas de las que tengamos instaladas. Normalmente, al actualizar una fórmula, nos instala la versión nueva, pero no retira las antiguas, que siguen instaladas (aunque fuera del `$PATH`). Para limpiar estas versiones antiguas, el comando que necesitamos es

    brew cleanup

Bueno, hasta aquí esta breve introducción. Hay unos cuantos comandos más, pero son menos frecuentemente utilizados, así que dad un vistazo a `man brew` para dar con ellos.

##Configuracion inicial de identidad

    git config --global user.name "Yoswer"
    git config --global user.email yoswer@example.com

##Configurar editor de texto

    git config --global core.editor emacs

Nota: El editor de texto varia de las preferencias del usuario.

##Comprobando configuración

    git config --list

##Recibiendo ayuda

    git help <verb>
    git <verb> --help
    man git-<verb>

##Inicializando un repositorio en un directorio existente

    git init

##Clonando un repositorio existente

    git clone [url]

Ejemplo:

    git clone https://github.com/libgit2/libgit2

Si quieres clonar el repositorio a un directorio con otro nombre que no sea libgit2, puedes especificarlo con la siguiente opción de línea de comandos:

    git clone https://github.com/libgit2/libgit2 mylibgit

Git te permite usar distintos protocolos de transferencia. El ejemplo anterior usa el protocolo `https://`, pero también puedes utilizar `git://` o `usuario@servidor:ruta/del/repositorio.git` que utiliza el protocolo de transferencia SSH.

##Guardando cambios en el Repositorio
![Ciclo de vida del estado de tus archivos](img/lifecycle.png)
Ciclo de vida del estado de tus archivos.

###Revisando el Estado de tus Archivos

    git status

###Rastrear Archivos Nuevos y Preparar Archivos Modificados

    git add (files)

El comando git add puede recibir tanto una ruta de archivo como de un directorio; si es de un directorio, el comando añade recursivamente los archivos que están dentro de él.

###Estado Abreviado

    git status -s

Los archivos nuevos que no están rastreados tienen un ?? a su lado, los archivos que están preparados tienen una A y los modificados una M. El estado aparece en dos columnas - la columna de la izquierda indica el estado preparado y la columna de la derecha indica el estado sin preparar.

###Ignorar Archivos
En estos casos, puedes crear un archivo llamado `.gitignore` que liste patrones a considerar.

Las reglas sobre los patrones que puedes incluir en el archivo `.gitignore` son las siguientes:

1. Ignorar las líneas en blanco y aquellas que comiencen con #.

2. Aceptar patrones glob estándar.

3. Los patrones pueden terminar en barra (/) para especificar un directorio.

4. Los patrones pueden negarse si se añade al principio el signo de exclamación (!).

Los patrones glob son una especie de expresión regular simplificada usada por los terminales. Un asterisco (\*) corresponde a cero o más caracteres; [abc] corresponde a cualquier carácter dentro de los corchetes (en este caso a, b o c); el signo de interrogación (?) corresponde a un carácter cualquier; y los corchetes sobre caracteres separados por un guión ([0-9]) corresponde a cualquier carácter entre ellos (en este caso del 0 al 9). También puedes usar dos asteriscos para indicar directorios anidados; a/\*\*/z coincide con a/z, a/b/z, a/b/c/z, etc.

> Tip:
> GitHub mantiene una extensa lista de archivos .gitignore adecuados a docenas de proyectos y lenguajes en https://github.com/github/gitignore en caso de que quieras tener un punto de partida para tu proyecto.

###Ver los Cambios Preparados y No Preparados

    git diff

Si quieres ver lo que has preparado y será incluido en la próxima confirmación, puedes usar `git diff --staged`. Este comando compara tus cambios preparados con la última instantánea confirmada.

> Note:
> Git Diff como Herramienta Externa
> A lo largo del libro, continuaremos usando el comando `git diff` de distintas maneras. Existe otra forma de ver estas diferencias si prefieres utilizar una interfaz gráfica u otro programa externo. Si ejecutas `git difftool` en vez de `git diff`, podrás ver los cambios con programas de este tipo como Araxis, emerge, vimdiff y más. Ejecuta `git difftool --tool-help` para ver qué tienes disponible en tu sistema.

###Confirmar tus Cambios

    git commit

Para obtener una forma más explícita de recordar qué has modificado, puedes pasar la opción `-v` a `git commit`. Al hacerlo se incluirá en el editor el diff de tus cambios para que veas exactamente qué cambios estás confirmando.
Otra alternativa es escribir el mensaje de confirmación directamente en el comando `commit` utilizando la opción `-m`

###Saltar el Área de Preparación
A pesar de que puede resultar muy útil para ajustar los commits tal como quieres, el área de preparación es a veces un paso más complejo a lo que necesitas para tu flujo de trabajo. Si quieres saltarte el área de preparación, Git te ofrece un atajo sencillo. Añadiendo la opción `-a` al comando `git commit` harás que Git prepare automáticamente todos los archivos rastreados antes de confirmarlos, ahorrándote el paso de `git add`.

###Eliminar Archivos
Para eliminar archivos de Git, debes eliminarlos de tus archivos rastreados (o mejor dicho, eliminarlos del área de preparación) y luego confirmar. Para ello existe el comando `git rm`, que además elimina el archivo de tu directorio de trabajo de manera que no aparezca la próxima vez como un archivo no rastreado.
Otra cosa que puedas querer hacer es mantener el archivo en tu directorio de trabajo pero eliminarlo del área de preparación. En otras palabras, quisieras mantener el archivo en tu disco duro pero sin que Git lo siga rastreando. Esto puede ser particularmente útil si olvidaste añadir algo en tu archivo `.gitignore` y lo preparaste accidentalmente, algo como un gran archivo de trazas a un montón de archivos compilados `.a`. Para hacerlo, utiliza la opción `--cached`.
Al comando git rm puedes pasarle archivos, directorios y patrones glob. Lo que significa que puedes hacer cosas como:

    git rm log/\*.log

###Cambiar el Nombre de los Archivos
Al contrario que muchos sistemas VCS, Git no rastrea explícitamente los cambios de nombre en archivos. Si renombras un archivo en Git, no se guardará ningún metadato que indique que renombraste el archivo. Sin embargo, Git es bastante listo como para detectar estos cambios luego que los has hecho - más adelante, veremos cómo se detecta el cambio de nombre.

Por esto, resulta confuso que Git tenga un comando mv. Si quieres renombrar un archivo en Git, puedes ejecutar algo como:

    git mv file_from file_to

##Ver el Historial de Confirmaciones
Después de haber hecho varias confirmaciones, o si has clonado un repositorio que ya tenía un histórico de confirmaciones, probablemente quieras mirar atrás para ver qué modificaciones se han llevado a cabo. La herramienta más básica y potente para hacer esto es el comando `git log`.
El comando `git log` proporciona gran cantidad de opciones para mostrarte exactamente lo que buscas. Aquí veremos algunas de las más usadas.

Una de las opciones más útiles es `-p`, que muestra las diferencias introducidas en cada confirmación. También puedes usar la opción `-2`, que hace que se muestren únicamente las dos últimas entradas del historial.
La opción `--stat` imprime tras cada confirmación una lista de archivos modificados, indicando cuántos han sido modificados y cuántas líneas han sido añadidas y eliminadas para cada uno de ellos, y un resumen de toda esta información.
Otra opción realmente útil es `--pretty`, que modifica el formato de la salida. Tienes unos cuantos estilos disponibles. La opción `oneline` imprime cada confirmación en una única línea, lo que puede resultar útil si estás analizando gran cantidad de confirmaciones. Otras opciones son `short`, `full` y `fuller`, que muestran la salida en un formato parecido, pero añadiendo menos o más información, respectivamente:

    git log --pretty=oneline

La opción más interesante es `format`, que te permite especificar tu propio formato. Esto resulta especialmente útil si estás generando una salida para que sea analizada por otro programa —como especificas el formato explícitamente, sabes que no cambiará en futuras actualizaciones de Git—:

    git log --pretty=format:"%h - %an, %ar : %s"

Opciones útiles de `git log --pretty=format`
Opción | Descripción de la salida
------------ | -------------
`%H` | Hash de la confirmación
`%h` | Hash de la confirmación abreviado
`%T` | Hash del árbol
`%t` | Hash del árbol abreviado
`%P` | Hashes de las confirmaciones padre
`%p` | Hashes de las confirmaciones padre abreviados
`%an` | Nombre del autor
`%ae` | Dirección de correo del autor
`%ad` | Fecha de autoría (el formato respeta la opción `-–date`)
`%ar` | Fecha de autoría, relativa
`%cn` | Nombre del confirmador
`%ce` | Dirección de correo del confirmador
`%cd` | Fecha de confirmación
`%cr` | Fecha de confirmación, relativa
`%s` | Asunto

Las opciones `oneline` y `format` son especialmente útiles combinadas con otra opción llamada `--graph`. Ésta añade un pequeño gráfico ASCII mostrando tu historial de ramificaciones y uniones

Opciones típicas de `git log`
Opción | Descripción de la salida
------------ | -------------
`-p` | Muestra el parche introducido en cada confirmación.
`--stat` | Muestra estadísticas sobre los archivos modificados en cada confirmación.
`--shortstat` | Muestra solamente la línea de resumen de la opción `--stat`.
`--name-only` | Muestra la lista de archivos afectados.
`--name-status` | Muestra la lista de archivos afectados, indicando además si fueron añadidos, modificados o eliminados.
`--abbrev-commit` | Muestra solamente los primeros caracteres de la suma SHA-1, en vez de los 40 caracteres de que se compone.
`--relative-date` | Muestra la fecha en formato relativo (por ejemplo, “2 weeks ago” (“hace 2 semanas”)) en lugar del formato completo.
`--graph` | Muestra un gráfico ASCII con la historia de ramificaciones y uniones.
`--pretty` | Muestra las confirmaciones usando un formato alternativo. Posibles opciones son `oneline`, `short`, `full`, `fuller` y `format` (mediante el cual puedes especificar tu propio formato).

###Limitar la Salida del Historial
La opción `--author` te permite filtrar por autor, y `--grep` te permite buscar palabras clave entre los mensajes de confirmación. (Ten en cuenta que si quieres aplicar ambas opciones simultáneamente, tienes que añadir `--all-match`, o el comando mostrará las confirmaciones que cumplan cualquiera de las dos, no necesariamente las dos a la vez.).
La última opción verdaderamente útil para filtrar la salida de `git log` es especificar una ruta. Si especificas la ruta de un directorio o archivo, puedes limitar la salida a aquellas confirmaciones que introdujeron un cambio en dichos archivos. Ésta debe ser siempre la última opción, y suele ir precedida de dos guiones (`--`) para separar la ruta del resto de opciones.

Opciones para limitar la salida de `git log`
Opción | Descripción de la salida
------------ | -------------
`-(n)` | Muestra solamente las últimas n confirmaciones
`--since, --after` | Muestra aquellas confirmaciones hechas después de la fecha especificada.
`--until, --before` | Muestra aquellas confirmaciones hechas antes de la fecha especificada.
`--author` | Muestra solo aquellas confirmaciones cuyo autor coincide con la cadena especificada.
`--committer` | Muestra solo aquellas confirmaciones cuyo confirmador coincide con la cadena especificada.
`-S` | Muestra solo aquellas confirmaciones que añadan o eliminen código que corresponda con la cadena especificada.

##Deshacer Cosas
Uno de las acciones más comunes a deshacer es cuando confirmas un cambio antes de tiempo y olvidas agregar algún archivo, o te equivocas en el mensaje de confirmación. Si quieres rehacer la confirmación, puedes reconfirmar con la opción `--amend`:

    git commit --amend

###Deshacer un Archivo Preparado

    git reset HEAD <file>

###Deshacer un Archivo Modificado

    git checkout -- <file>

Es importante entender que `git checkout -- [archivo]` es un comando peligroso. Cualquier cambio que le hayas hecho a ese archivo desaparecerá - acabas de sobreescribirlo con otro archivo. Nunca utilices este comando a menos que estés absolutamente seguro de que ya no quieres el archivo.

##Trabajar con Remotos
###Ver Tus Remotos
Para ver los remotos que tienes configurados, debes ejecutar el comando `git remote`. Mostrará los nombres de cada uno de los remotos que tienes especificados. También puedes pasar la opción `-v`, la cual muestra las URLs que Git ha asociado al nombre y que serán usadas al leer y escribir en ese remoto.

###Añadir Repositorios Remotos
Para añadir un remoto nuevo y asociarlo a un nombre que puedas referenciar fácilmente, ejecuta `git remote add [nombre] [url]`.

###Traer y Combinar Remotos
Como hemos visto hasta ahora, para obtener datos de tus proyectos remotos puedes ejecutar:

    git fetch [remote-name]

El comando irá al proyecto remoto y se traerá todos los datos que aun no tienes de dicho remoto. Luego de hacer esto, tendrás referencias a todas las ramas del remoto, las cuales puedes combinar e inspeccionar cuando quieras.
Es importante destacar que el comando `git fetch` solo trae datos a tu repositorio local - ni lo combina automáticamente con tu trabajo ni modifica el trabajo que llevas hecho. La combinación con tu trabajo debes hacerla manualmente cuando estés listo.
Si has configurado una rama para que rastree una rama remota, puedes usar el comando `git pull` para traer y combinar automáticamente la rama remota con tu rama actual. Es posible que este sea un flujo de trabajo mucho más cómodo y fácil para ti; y por defecto, el comando `git clone` le indica automáticamente a tu rama maestra local que rastree la rama maestra remota (o como se llame la rama por defecto) del servidor del que has clonado. Generalmente, al ejecutar `git pull` traerás datos del servidor del que clonaste originalmente y se intentará combinar automáticamente la información con el código en el que estás trabajando.

###Enviar a Tus Remotos
Cuando tienes un proyecto que quieres compartir, debes enviarlo a un servidor. El comando para hacerlo es simple: `git push [nombre-remoto] [nombre-rama]`. Si quieres enviar tu rama master a tu servidor origin (recuerda, clonar un repositorio establece esos nombres automáticamente), entonces puedes ejecutar el siguiente comando y se enviarán todos los commits que hayas hecho al servidor:

    git push origin master

Este comando solo funciona si clonaste de un servidor sobre el que tienes permisos de escritura y si nadie más ha enviado datos por el medio. Si alguien más clona el mismo repositorio que tú y envía información antes que tú, tu envío será rechazado. Tendrás que traerte su trabajo y combinarlo con el tuyo antes de que puedas enviar datos al servidor.

###Inspeccionar un Remoto
Si quieres ver más información acerca de un remoto en particular, puedes ejecutar el comando `git remote show [nombre-remoto]`.

###Eliminar y Renombrar Remotos
Si quieres cambiar el nombre de la referencia de un remoto puedes ejecutar `git remote rename`.
Si por alguna razón quieres eliminar un remoto - has cambiado de servidor o no quieres seguir utilizando un mirror, o quizás un colaborador ha dejado de trabajar en el proyecto - puedes usar `git remote rm`.

##Etiquetado
Como muchos VCS, Git tiene la posibilidad de etiquetar puntos específicos del historial como importantes. Esta funcionalidad se usa típicamente para marcar versiones de lanzamiento (v1.0, por ejemplo). En esta sección, aprenderás cómo listar las etiquetas disponibles, cómo crear nuevas etiquetas y cuáles son los distintos tipos de etiquetas.

###Listar Tus Etiquetas
git tag
También puedes buscar etiquetas con un patrón particular. El repositorio del código fuente de Git, por ejemplo, contiene más de 500 etiquetas. Si solo te interesa ver la serie 1.8.5, puedes ejecutar:

    git tag -l 'v1.8.5*'

###Crear Etiquetas
Git utiliza dos tipos principales de etiquetas: ligeras y anotadas.

Una etiqueta ligera es muy parecido a una rama que no cambia - simplemente es un puntero a un commit específico.

Sin embargo, las etiquetas anotadas se guardan en la base de datos de Git como objetos enteros. Tienen un checksum; contienen el nombre del etiquetador, correo electrónico y fecha; tienen un mensaje asociado; y pueden ser firmadas y verificadas con GNU Privacy Guard (GPG). Normalmente se recomienda que crees etiquetas anotadas, de manera que tengas toda esta información; pero si quieres una etiqueta temporal o por alguna razón no estás interesado en esa información, entonces puedes usar las etiquetas ligeras.

###Etiquetas Anotadas
Crear una etiqueta anotada en Git es sencillo. La forma más fácil de hacer es especificar la opción `-a` cuando ejecutas el comando tag.
La opción `-m` especifica el mensaje de la etiqueta, el cual es guardado junto con ella. Si no especificas el mensaje de una etiqueta anotada, Git abrirá el editor de texto para que lo escribas.
Puedes ver la información de la etiqueta junto con el commit que está etiquetado al usar el comando `git show`.

###Etiquetas Ligeras
La otra forma de etiquetar un commit es mediante una etiqueta ligera. Una etiqueta ligera no es más que el checksum de un commit guardado en un archivo - no incluye más información. Para crear una etiqueta ligera, no pases las opciones `-a`, `-s` ni `-m`.
Esta vez, si ejecutas `git show` sobre la etiqueta, no verás la información adicional. El comando solo mostrará el commit.

###Etiquetado Tardío
También puedes etiquetar commits mucho tiempo después de haberlos hecho.
Para etiquetar un commit, debes especificar el checksum del commit (o parte de él) al final del comando:

    git tag -a v1.2 9fceb02

###Compartir Etiquetas
Por defecto, el comando `git push` no transfiere las etiquetas a los servidores remotos. Debes enviar las etiquetas de forma explícita al servidor luego de que las hayas creado. Este proceso es similar al de compartir ramas remotas - puedes ejecutar `git push origin [etiqueta]`.
Si quieres enviar varias etiquetas a la vez, puedes usar la opción `--tags` del comando `git push`. Esto enviará al servidor remoto todas las etiquetas que aun no existen en él.

###Sacar una Etiqueta
En Git, no puedes sacar (check out) una etiqueta, pues no es algo que puedas mover. Si quieres colocar en tu directorio de trabajo una versión de tu repositorio que coincida con alguna etiqueta, debes crear una rama nueva en esa etiqueta:

    git checkout -b version2 v2.0.0

Obviamente, si haces esto y luego confirmas tus cambios, tu rama version2 será ligeramente distinta a tu etiqueta v2.0.0 puesto que incluirá tus nuevos cambios; así que ten cuidado.

##Alias de Git
Git no deduce automáticamente tu comando si lo tecleas parcialmente. Si no quieres teclear el nombre completo de cada comando de Git, puedes establecer fácilmente un alias para cada comando mediante git config. Aquí tienes algunos ejemplos que te pueden interesar:

    git config --global alias.co checkout
    git config --global alias.br branch
    git config --global alias.ci commit
    git config --global alias.st status

Esta técnica también puede resultar útil para crear comandos que en tu opinión deberían existir. Por ejemplo, para corregir el problema de usabilidad que encontraste al quitar del área de preparación un archivo, puedes añadir tu propio alias a Git:

    git config --global alias.unstage 'reset HEAD --'

Esto hace que los dos comandos siguientes sean equivalentes:

    git unstage fileA
    git reset HEAD fileA

Esto parece un poco más claro. También es frecuente añadir un comando last, de este modo:

    git config --global alias.last 'log -1 HEAD'

De esta manera, puedes ver fácilmente cuál fue la última confirmación.
Como puedes ver, Git simplemente sustituye el nuevo comando por lo que sea que hayas puesto en el alias. Sin embargo, quizás quieras ejecutar un comando externo, en lugar de un subcomando de Git. En ese caso, puedes comenzar el comando con un carácter !. Esto resulta útil si escribes tus propias herramientas para trabajar con un repositorio de Git.

##Ramas
###Crear una Rama Nueva

    git branch [branch-name]

Pued ejecutar el comando `git log` para que te muestre a dónde apunta cada rama. Esta opción se llama `--decorate`.

###Cambiar de Rama
Para saltar de una rama a otra, tienes que utilizar el comando `git checkout [branch-name]`.

También puedes ver esto fácilmente utilizando el comando git log. Si ejecutas `git log --oneline --decorate --graph --all` te mostrará el historial de tus confirmaciones, indicando dónde están los apuntadores de tus ramas y como ha divergido tu historial.

###Procedimientos Básicos de Ramificación
Para crear una nueva rama y saltar a ella, en un solo paso, puedes utilizar el comando git checkout con la opción -b:

    git checkout -b [branch-name]

Hemos de tener en cuenta que si tenemos cambios aún no confirmados en el directorio de trabajo o en el área de preparación, Git no nos permitirá saltar a otra rama con la que podríamos tener conflictos. Lo mejor es tener siempre un estado de trabajo limpio y despejado antes de saltar entre ramas.

###Fusionar ramas
Simplemente, activa (checkout) la rama donde deseas fusionar y lanza el comando `git merge`:
Para fusionar distintas ramas se utiliza el comando:

    git merge [branch-name-to-fusion]

###Borrar ramas

    git branch -d [branch-name]

###Principales Conflictos que Pueden Surgir en las Fusiones
En algunas ocasiones, los procesos de fusión no suelen ser fluidos. Si hay modificaciones dispares en una misma porción de un mismo archivo en las dos ramas distintas que pretendes fusionar, Git no será capaz de fusionarlas directamente.
Todo aquello que sea conflictivo y no se haya podido resolver, se marca como "sin fusionar" (unmerged). Git añade a los archivos conflictivos unos marcadores especiales de resolución de conflictos que te guiarán cuando abras manualmente los archivos implicados y los edites para corregirlos.
Si en lugar de resolver directamente prefieres utilizar una herramienta gráfica, puedes usar el comando `git mergetool`, el cual arrancará la correspondiente herramienta de visualización y te permitirá ir resolviendo conflictos con ella.

###Gestión de Ramas
El comando `git branch` tiene más funciones que las de crear y borrar ramas. Si lo lanzas sin parámetros, obtienes una lista de las ramas presentes en tu proyecto. Para ver la última confirmación de cambios en cada rama, puedes usar el comando `git branch -v`. Si deseas ver las ramas que han sido fusionadas en la rama activa, puedes lanzar el comando `git branch --merged`. Para mostrar todas las ramas que contienen trabajos sin fusionar, puedes utilizar el comando `git branch --no-merged`.

###Ramas Remotas
Para sincronizarte, puedes utilizar el comando `git fetch origin`. Este comando localiza en qué servidor está el origen, recupera cualquier dato presente allí que tú no tengas, y actualiza tu base de datos local, moviendo tu rama origin/master para que apunte a la posición más reciente.

###Publicar
Cuando quieres compartir una rama con el resto del mundo, debes llevarla (push) a un remoto donde tengas permisos de escritura. Tus ramas locales no se sincronizan automáticamente con los remotos en los que escribes, sino que tienes que enviar (push) expresamente las ramas que desees compartir. De esta forma, puedes usar ramas privadas para el trabajo que no deseas compartir, llevando a un remoto tan solo aquellas partes que deseas aportar a los demás.

###Hacer Seguimiento a las Ramas
Al activar (checkout) una rama local a partir de una rama remota, se crea automáticamente lo que podríamos denominar una “rama de seguimiento” (tracking branch). Las ramas de seguimiento son ramas locales que tienen una relación directa con alguna rama remota. Si estás en una rama de seguimiento y tecleas el comando `git pull`, Git sabe de cuál servidor recuperar (fetch) y fusionar (merge) datos.
Cuando clonas un repositorio, este suele crear automáticamente una rama master que hace seguimiento de origin/master. Sin embargo, puedes preparar otras ramas de seguimiento si deseas tener unas que sigan ramas de otros remotos o no seguir la rama master. El ejemplo más simple es el que acabas de ver al lanzar el comando `git checkout -b [rama] [nombreremoto]/[rama]`. Esta operación es tan común que git ofrece el parámetro `--track`. Para preparar una rama local con un nombre distinto a la del remoto, puedes utilizar la primera versión con un nombre de rama local diferente.
Si ya tienes una rama local y quieres asignarla a una rama remota que acabas de traerte, o quieres cambiar la rama a la que le haces seguimiento, puedes usar en cualquier momento las opciones `-u` o `--set-upstream-to` del comando `git branch`.
> Note:
Atajo al upstream
Cuando tienes asignada una rama de seguimiento, puedes hacer referencia a ella mediante `@{upstream}` o mediante el atajo `@{u}`. De esta manera, si estás en la rama master y esta sigue a la rama origin/master, puedes hacer algo como `git merge @{u}` en vez de `git merge origin/master`.

Si quieres ver las ramas de seguimiento que tienes asignado, puedes usar la opción `-vv` con `git branch`. Esto listará tus ramas locales con más información, incluyendo a qué sigue cada rama y si tu rama local está por delante, por detrás o ambas.

###Traer y Fusionar
A pesar de que el comando `git fetch` trae todos los cambios del servidor que no tienes, este no modifica tu directorio de trabajo. Simplemente obtendrá los datos y dejará que tú mismo los fusiones. Sin embargo, existe un comando llamado `git pull`, el cuál básicamente hace `git fetch` seguido por `git merge` en la mayoría de los casos. Si tienes una rama de seguimiento configurada como vimos en la última sección, bien sea asignándola explícitamente o creándola mediante los comandos `clone` o `checkout`, `git pull` identificará a qué servidor y rama remota sigue tu rama actual, traerá los datos de dicho servidor e intentará fusionar dicha rama remota.

Normalmente es mejor usar los comandos `fetch` y `merge` de manera explícita pues la magia de `git pull` puede resultar confusa.

###Eliminar Ramas Remotas
Puedes borrar la rama remota utilizando la opción `--delete` de `git push`. Básicamente lo que hace es eliminar el apuntador del servidor. El servidor Git suele mantener los datos por un tiempo hasta que el recolector de basura se ejecute, de manera que si la has borrado accidentalmente, suele ser fácil recuperarla.

###Reorganización Básica
Con el comando `git rebase`, puedes coger todos los cambios confirmados en una rama, y reaplicarlos sobre otra. Haciendo que Git vaya al ancestro común de ambas ramas (donde estás actualmente y de donde quieres reorganizar), saque las diferencias introducidas por cada confirmación en la rama donde estás, guarde esas diferencias en archivos temporales, reinicie (reset) la rama actual hasta llevarla a la misma confirmación en la rama de donde quieres reorganizar, y, finalmente, vuelva a aplicar ordenadamente los cambios.
Imagina que decides incorporar tus cambios del lado cliente sobre el proyecto principal para hacer un lanzamiento de versión; pero no quieres lanzar aún los cambios del lado servidor porque no están aún suficientemente probados. Puedes coger los cambios del cliente que no están en server (C8 y C9) y reaplicarlos sobre tu rama principal usando la opción `--onto` del comando `git rebase`:

    git rebase --onto master server client
Esto viene a decir: “Activa la rama client, averigua los cambios desde el ancestro común entre las ramas client y server, y aplicalos en la rama master”. Puede parecer un poco complicado, pero los resultados son realmente interesantes.
Puedes reorganizar (rebase) la rama server sobre la rama master sin necesidad siquiera de comprobarlo previamente, usando el comando `git rebase [rama-base] [rama-puntual]`, el cual activa la rama puntual (server en este caso) y la aplica sobre la rama base (master en este caso):

    git rebase master server
Esto vuelca el trabajo de server sobre el de master, tal y como se muestra en Reorganizando la rama server sobre la rama master.

###Los Peligros de Reorganizar
Ahh…​, pero la dicha de la reorganización no la alcanzamos sin sus contrapartidas, las cuales pueden resumirse en una línea:

**Nunca reorganices confirmaciones de cambio (commits) que hayas enviado (push) a un repositorio público.**

Si sigues esta recomendación, no tendrás problemas. Pero si no lo haces, la gente te odiará y serás despreciado por tus familiares y amigos.

###Reorganizar una Reorganización
Puedes simplificar el proceso si ejecutas `git pull --rebase` en vez del tradicional `git pull`. O, en este caso, puedes hacerlo manualmente con un `git fetch` primero, seguido de un `git rebase teamone/master`.

Si sueles utilizar `git pull` y quieres que la opción `--rebase` esté activada por defecto, puedes asignar el valor de configuración `pull.rebase` haciendo algo como esto `git config --global pull.rebase true`.

Si consideras la reorganización como una manera de limpiar tu trabajo y tus confirmaciones antes de enviarlas (push), y si solo reorganizas confirmaciones (commits) que nunca han estado disponibles públicamente, no tendrás problemas. Si reorganizas (rebase) confirmaciones (commits) que ya estaban disponibles públicamente y la gente había basado su trabajo en ellas, entonces prepárate para tener problemas, frustrar a tu equipo y ser despreciado por tus compañeros.

Si tu compañero o tú ven que aun así es necesario hacerlo en algún momento, asegúrense que todos sepan que deben ejecutar `git pull --rebase` para intentar aliviar en lo posible la frustración.

##Git en el Servidor
###Los Protocolos
Git puede usar cuatro protocolos principales para transferir datos: Local, HTTP, Secure Shell (SSH) y Git. Vamos a ver en qué consisten y las circunstancias en que querrás (o no) utilizar cada uno de ellos.

####Local Protocol
Para clonar un repositorio como estos, o para añadirlo como remoto a un proyecto ya existente, usa el camino (path) del repositorio como su URL. Por ejemplo, para clonar un repositorio local, puedes usar algo como:

    git clone /opt/git/project.git
O como:

    git clone file:///opt/git/project.git
Para añadir un repositorio local a un proyecto Git existente, puedes usar algo como:

    git remote add local_proj /opt/git/project.git
Con lo que podrás enviar (push) y recibir (pull) desde dicho remoto exactamente de la misma forma a como lo harías a través de una red.

####Protocolos HTTP
El protocolo HTTP “Inteligente” funciona de forma muy similar a los protocolos SSH y Git, pero se ejecutan sobre puertos estándar HTTP/S y pueden utilizar los diferentes mecanismos de autenticación HTTP. Esto significa que puede resultar más fácil para los usuarios, puesto que se pueden identificar mediante usuario y contraseña (usando la autenticación básica de HTTP) en lugar de usar claves SSH.

####El Procotolo SSH
SSH es un protocolo muy habitual para alojar repositorios Git en hostings privados. Esto es así porque el acceso SSH viene habilitado de forma predeterminada en la mayoría de los servidores, y si no es así, es fácil de habilitarlo. Además, SSH es un protocolo de red autenticado, y es sencillo de utilizar.

####El protocolo Git
El protocolo Git es un demonio (daemon) especial, que viene incorporado con Git. Escucha por un puerto dedicado (9418), y nos da un servicio similar al del protocolo SSH; pero sin ningún tipo de autentificación.

###Configurando Git en un servidor
Para configurar por primera vez un servidor de Git, hay que exportar un repositorio existente en un nuevo repositorio vacío - un repositorio que no contiene un directorio de trabajo. Esto es generalmente fácil de hacer. Para clonar el repositorio con el fin de crear un nuevo repositorio vacío, se ejecuta el comando `clone` con la opción `--bare`. 

###Colocando un Repositorio Vacío en un Servidor
Ahora que tienes una copia vacía de tú repositorio, todo lo que necesitas hacer es ponerlo en un servidor y establecer sus protocolos.

###Git en un alojamiento externo
Actualmente hay bastantes opciones de alojamiento para elegir, cada una con sus ventajas e inconvenientes. Para ver una lista actualizada, mira la página acerca de alojamiento Git en el wiki principal de Git, en [esta pagina](https://git.wiki.kernel.org/index.php/GitHosting).

##Git en entornos distribuidos
###Pautas de confirmación
En primer lugar, no desea enviar ningún error de espacios en blanco. Git proporciona una manera fácil de verificar esto: antes de comprometerse, ejecute `git diff --check`, que identifica posibles errores de espacio en blanco y los enumera por usted.
Si algunos de los cambios modifican el mismo archivo, intente utilizar `git add --patch` para representar parcialmente los archivos (se detalla en << _ interactive_staging >>).
Intenta ejecutar `git log --no-merges` allí para ver cómo se ve un historial de commit de proyecto muy bien formateado.
###Contribuyendo a un proyecto
El comando `request-pull` toma la rama base en la que desea que se saque su rama de tema y la URL del repositorio de Git de la que desea que extraigan, y genera un resumen de todos los cambios que está solicitando.
Utiliza `git format-patch` para generar los archivos con formato mbox que puedes enviar por correo electrónico a la lista: convierte cada confirmación en un mensaje de correo electrónico con la primera línea del mensaje de confirmación como tema y el resto de el mensaje más el parche que introduce el compromiso como el cuerpo. Lo bueno de esto es que la aplicación de un parche de un correo electrónico generado con `format-patch` conserva toda la información de compromiso correctamente. El comando `format-patch` imprime los nombres de los archivos de parche que crea. El modificador `-M` le dice a Git que busque cambios de nombre.

it proporciona una herramienta para ayudarlo a enviar parches con formato correcto a través de IMAP, que puede ser más fácil para usted. Demostraremos cómo enviar un parche a través de Gmail, que es el agente de correo electrónico que mejor conocemos; puede leer instrucciones detalladas para una cantidad de programas de correo al final del archivo Documentation / SubmittingPatches antes mencionado en el código fuente de Git.

Primero, necesitas configurar la sección imap en tu archivo `~ / .gitconfig`. Puede establecer cada valor por separado con una serie de comandos `git config`, o puede agregarlos manualmente, pero al final su archivo de configuración debería verse más o menos así:

    [imap]
        folder = "[Gmail]/Drafts"
        host = imaps://imap.gmail.com
        user = user@gmail.com
        pass = p4ssw0rd
        port = 993
        sslverify = false
Si su servidor IMAP no usa SSL, las dos últimas líneas probablemente no sean necesarias, y el valor del host será `imap: //` en lugar de `imaps: //`. Cuando esté configurado, puede usar `git send-email` para colocar la serie de parches en la carpeta Borradores del servidor IMAP especificado:
    git send-email *.patch

###Manteniendo un proyecto
Si recibes por e-mail un parche que necesitas integrar en tu proyecto, deberías aplicarlo en tu rama puntual para evaluarlo. Hay dos formas de aplicar un parche enviado por e-mail: con `git apply` o `git am`.

Si recibiste el parche de alguien que lo generó con git diff o con el comando Unix diff, puedes aplicarlo con el comando `git apply`. También puedes usar `git apply` para comprobar si un parche se aplica de forma limpia antes de aplicarlo realmente – puedes ejecutar `git apply --check` indicando el parche.

Para aplicar un parche generado con format-patch, usa `git am`. Técnicamente, `git am` se construyó para leer de un archivo mbox (buzón de correo). Es un formato de texto plano simple para almacenar uno o más mensajes de correo en un archivo de texto.

En el contexto del comando `diff`, puedes poner tres puntos tras el nombre de una rama para hacer un `diff` entre el último commit de la rama en la que estás y su ancestro común con otra rama:

    git diff master...contrib
Este comando sólo muestra el trabajo introducido en tu rama puntual actual desde su ancestro común con la rama master. Es una sintaxis muy útil a recordar.

Otra forma de mover trabajo de una rama a otra es entresacarlo (cherry-pick). En Git, "entresacar" es como hacer un rebase para un único commit. Toma el parche introducido en un commit e intenta reaplicarlo en la rama en la que estás actualmente. Esto es útil si tienes varios commits en una rama puntual y sólo quieres integrar uno de ellos, o si sólo tienes un commit en una rama puntual y prefieres entresacarlo en lugar de hacer una reorganización (rebase).

Rerere significa “reuse recorded resolution” (reutilizar resolución grabada) – es una forma de simplificar la resolución de conflictos. Cuando rerere está activo, Git mantendrá un conjunto de imágenes anteriores y posteriores a las integraciones correctas, de forma que si detecta que hay un conflicto que parece exactamente igual que otro ya corregido previamente, usará esa misma corrección sin causarte molestias.
Esta funcionalidad consta de dos partes: un parámetro de configuración y un comando. El parámetro de configuración es rerere.enabled y es bastante útil ponerlo en tu configuración global:

    git config --global rerere.enabled true
Ahora, cuando hagas una integración que resuelva conflictos, la resolución se grabará en la caché por si la necesitas en un futuro.

Como Git no genera una serie de números monótonamente creciente como v123 o similar con cada commit, si quieres tener un nombre más comprensible para un commit, puedes ejecutar el comando `git describe` sobre dicho commit. Git devolverá el nombre de la etiqueta más próxima junto con el número de commits sobre esa etiqueta y una parte del valor SHA-1 del commit que estás describiendo.

Ahora quieres lanzar una versión. Una cosa que querrás hacer será crear un archivo con la última instantánea del código para esas pobres almas que no usan Git. El comando para hacerlo es `git archive`:

    git archive master --prefix='project/' | gzip > `git describe master`.tar.gz
    ls *.tar.gz
    v1.6.2-rc1-20-g8c5b85c.tar.gz
Si alguien abre el archivo tar, obtiene la última instantánea de tu proyecto bajo un directorio project. También puedes crear un archivo zip de la misma manera, pero añadiendo la opción `--format=zip` a `git archive`:

    git archive master --prefix='project/' --format=zip > `git describe master`.zip
Ahora tienes tanto un archivo tar como zip con la nueva versión de tu proyecto, listos para subirlos a tu sitio web o para enviarlos por e-mail.

Es el momento de enviar un mensaje a tu lista de correo informando sobre el estado de tu proyecto. Una buena opción para obtener rápidamente una especie de lista con los cambios introducidos en tu proyecto desde la última versión o e-mail es usar el comando `git shortlog`. Dicho comando resume todos los commits en el rango que se le indique.
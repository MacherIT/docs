#Control de versiones y repositorios en el equipo de Diseño

## Git Large File Storage
Se trabajará con **Git LFS**, un sistema que permite incluir archivos grandes, típicos del trabajo de diseño, en los repositorios, de manera ágil y sencilla.

### Instalación
Supondremos que estamos en una PC con Windows sin ninguna herramienta particular

#### Git
Primero instalaremos Git for Windows, una herramienta para usar *git* tanto de forma gráfica como desde una consola de texto.
1. [Descargar git](https://git-scm.com/download/win) e intalarlo.
2. Otra posibilidad es descargar e instalar [GitHub Desktop](https://desktop.github.com/)
3. En caso de que no se lo tenga ya, [crear un usuario de GitHub](https://github.com/login), que es donde almacenaremos remotamente nuestros respositorios.
4. Abrir GitHub (el programa que acabamos de instalar en nuestra PC), e incluir el nombre de usuario o email y la contraseña del usuario que acabamos de crear. 
5. En el paso siguiente, configurar el nombre de usuario con el que aparecerán nuestras acciones en el repositorio y demás.
6. El programa buscará en la PC todos los repositorios Git existentes. Luego se debe seleccionar los que se desea que se incluyan para manejar con esta herramienta. 

#### Git LFS (Large File Storage)
Esta es la herramienta que nos permitirá incluir archivos grandes en los repositorios de manera eficiente, evitando subir y bajar la totalidad de estos archivos cada vez que sucedan cambios en el repositorio.
1. En la [página de Git LFS](https://git-lfs.github.com/) hacer clic en el vínculo *Download* para descargarlo.
2. Instalarlo el archivo que se acaba de descargar.

#### Flujo de trabajo
A la hora de empezar a trabajar con esta metodología, existen dos posibilidades. La primera es empezar a aplicarla en un proyecto ya existente, mientras que la segunda es empezar a trabajar desde el comienzo del proyecto de esta manera (lo cual es lo más recomendable).

##### Crear un repositorio a partir de un proyecto ya existente
1. En el programa GitHub, hacer clic en el **+** de la izquierda arriba.
2. En *Create* incluir el nombre del proyecto en *Name*.
3. Elegir la carpeta del proyecto en *Local path*.
4. Hacer clic en **Create**. El respositorio para el proyecto se habrá creado.
5. Hacer clic en **Publish** (arriba a la derecha). Con esto, automáticamente se subirá el proyecto a GitHub (servidor).
*Esta versión de los archivos, que están en el servidor de GitHub, se denomina **remoto**.*
Ya tenemos un repositorio funcionando en la carpeta seleccionada, y ya lo hemos conectado al "repo" remoto alojado en GitHub. Pero todavía no se ha añadido ningún archivo al repo. Esto es lo que falta hacer, además de configurar el LFS. 
6. Para usar las funcionalidades del LFS (herramienta específica para los archivos grandes usados en el diseño), hacer clic en el engranaje de configuración (arriba a la derecha), y en *Repository Settings*.
7. En *Git LFS*, *File Pattern*, escribir el la extensión del tipo de archivos que se quiera incluir, precedida por un *, para indicar que se incluya todos los archivos de ese tipo. Por ej.: * *.ai * para incluir los archivos de Adobe Illustrator. Luego cliquear **Ok**.
Al volver al Escritorio de GitHub (el programa), notaremos que hay diversos archivos seleccionados (todos los que están en la carpeta donde se creó el repo). Estos están seleccionados para ser añadidos al repo (lo que se conoce como hacerles un *add*).
8. Excepto que alguno por razones específicas no se quiera incluir en el repo, dejar todos seleccionados, incluir un *comentario* acerca de lo que se está por hacer en el *commit* actual (por ejemplo, "Se incluyen los archivos del proyecto en el repositorio."), y hacer clic en **Commit to master**.
Haciendo esto se ha creado un *commit*. Lo que también se había hecho cuando se creó inicialmente el repo. Ahora se querrá subir este commit al repo remoto, lo que se conoce como *hacer un push* o *pushear*.
9. Hacer clic en **Sync**. Esto envía los cambios introducidos en el último commit, pero antes de esto baja del repositorio remoto todos los cambios que alguien más pueda haber *pusheado* al repo. Con esto, se evita pisar cambios de otra persona con los propios, o situaciones del estilo, lo que se conoce como *conflictos*.


##### Crear el repositorio para comenzar a trabajar en el proyecto.
Si no se ha creado aún ni carpetas ni archivos para el proyecto, se lo puede hacer con GitHub. 
1. Hacer clic en **+** a la izquierda arriba, darle un nombre al proyecto, y elegir la carpeta donde se quiere que **se cree la carpeta** para el proyecto.
2. Ya se ha creado un commit automáticamente. Sólo resta subirlo al remoto haciendo clic en **Publish**.

### Cambios a adoptar en el flujo de trabajo.
Se puede seguir trabajando normalmente, aún cuando se trabaja dentro de una carpeta que corresponde a un repositorio. Habrá dos casos en los que tendremos que hacer un paso adicional, a cambio de tener la posibilidad de controlar todo lo que se ha hecho (uno mismo así como todos los compañeros de equipo) en el proyecto.

#### Cambios en archivos ya existentes
Cuando se hayan hecho cambios en un archivo que ya existe en el repositorio (se lo ha incluido en un commit que se ha pusheado) y queramos incluir estos cambios en el repositorio, se deberá:
1. Confirmar que el archivo se encuentra listado en el programa GitHub, donde se listan todos los archivos que tienen cambios locales (que muestran un ícono gris a su derecha), así como los nuevos.
2. Confirmar que se encuentra tildado para ser incluido en un commit nuevo, así como también todos otros que queramos incluir.
3. Escribir una descripción de los cambios que se hayan hecho.
4. Hacer clic en *Commit to master*.

#### Archivos nuevos a incluir en el repositorio
Para incluir nuevos archivos, se deberá hacer lo mismo que con los archivos con cambios locales. Puede notarse la diferencia de que los archivos nuevos muestran un ícono verde, distinto de los archivos que tienen cambios.
Se pueden incluir archivos nuevos y archivos con cambios en el mismo commit, siempre y cuando el mismo comentario les sea adecuado. Esto se debe a que es una buena práctica hacer un commit por cada actividad que se haga en el proyecto.
# Plugins

*Tiempo estimado de lectura: 10min*

Un **plugin** es un componente que proporciona tareas reutilizables, extendiendo así la funcionalidad de **Justo**.
Hay *plugins* para trabajar con **Docker**, administradores de paquetes de sistema operativo (como **APT** y **APK**), **NPM**, archivos y directorios, etc.
Los *plugins* son una pieza clave de reutilización y de aumento de productividad.

En **Justo/JS**, los *plugins* se desarrollan como módulos **NPM**.
Siendo así el desarrollo y su utilización muy sencillas.

Se clasifican en simples y compuestos.
Un **plugin simple** (*simple plugin*) es aquel que proporciona una única tarea.
Mientras que un **plugin compuesto** (*composite plugin*), varias.

## Plugins oficiales

Los *plugins* oficiales se encuentran en módulos NPM cuyo nombre es similar a `justo.plugin.nombrePlugin`.
No se recomienda utilizar este convenio de nombres para *plugins* no oficiales.

Entre otros, encontramos:

- [justo.plugin.babel](https://www.npmjs.com/package/justo.plugin.babel). *Plugin* simple para ejecutar el compilador **Babel**.

- [justo.plugin.chrome](https://www.npmjs.com/package/justo.plugin.chrome). *Plugin* compuesto que proporciona tareas para trabajar con **Google Chrome** como, por ejemplo, abrirlo.

- [justo.plugin.cli](https://www.npmjs.com/package/justo.plugin.cli). *Plugin* simple para ejecutar comandos desde el *shell* local y remotamente mediante **SSH**.

- [justo.plugin.docker](https://www.npmjs.com/package/justo.plugin.docker). *Plugin* compuesto que proporciona tareas para trabajar con **Docker**.
  Por ejemplo, para construir imágenes, iniciar o detener contenedores, etc.

- [justo.plugin.eslint](https://www.npmjs.com/package/justo.plugin.eslint). *Plugin* simple para ejecutar **ESLint**.

- [justo.plugin.fs](https://www.npmjs.com/package/justo.plugin.fs). *Plugin* compuesto que proporciona tareas para trabajar con el sistema de ficheros local.
  Por ejemplo, para crear directorios, copiar archivos, añadir contenido a archivos, comprobar la existencia de una entrada en el sistema de ficheros, etc.

- [justo.plugin.group](https://www.npmjs.com/package/justo.plugin.group). *Plugin* compuesto para trabajar con grupos de usuarios del sistema operativo.
  Proporciona tareas locales y compatibles con SSH.
  Soporta **Alpine** y **Ubuntu**.

- [justo.plugin.handlebars](https://www.npmjs.com/package/justo.plugin.handlebars). *Plugin* compuesto que proporciona tareas para trabajar con el sistema de plantillas **Handlebars**.

- [justo.plugin.kill](https://www.npmjs.com/package/justo.plugin.kill). *Plugin* simple para enviar la señal KILL a uno o más procesos mediante los comandos `kill` o `pkill`.

- [justo.plugin.less](https://www.npmjs.com/package/justo.plugin.less). *Plugin* compuesto para trabajar con hojas de estilo **Less**.

- [justo.plugin.npm](https://www.npmjs.com/package/justo.plugin.npm). *Plugin* compuesto para trabajar con el administrador de paquetes **NPM** de **Node.js**.

- [justo.plugin.ping](https://www.npmjs.com/package/justo.plugin.ping). *Plugin* simple para realizar `ping`s.

- [justo.plugin.pkg](https://www.npmjs.com/package/justo.plugin.pkg). *Plugin* compuesto que proporciona tareas para trabajar con administradores de paquetes de sistema operativo como **APK** y **APT**.
Estas tareas se pueden utilizar tanto local como remotamente mediante SSH.

- [justo.plugin.user](https://www.npmjs.com/package/justo.plugin.user). *Plugin* compuesto para trabajar con usuarios.
  Proporciona tareas locales y compatibles con SSH.
  Soporta **Alpine** y **Ubuntu**.

- [justo.plugin.webpack](https://www.npmjs.com/package/justo.plugin.webpack). *Plugin* simple para trabajar con `webpack`.

## Definición de plugins

En primer lugar, hay que crear la estructura de un *plugin*.
Lo mejor es utilizar el generador `justo.generator.plugin`, tal como muestra el siguiente ejemplo:

```
$ justojs g plugin
```
Recordemos que para mostrar los comandos proporcionados por un generador, se usa la opción `-h`:

```
$ justojs g plugin -h

 Command   Description
-------------------------------------------------------
 add task  Generate a new task for the current plugin.
 dflt      Generate the Justo plugin scaffold.

$
```

Este generador despliega una batería de preguntas, a partir de las cuales crea la estructura de directorios que mejor se adapta a la información recolectada.
Y además, rellena el contenido de algunos archivos.
Entre las preguntas encontramos:

- Quién es el autor, la organización responsable del *plugin*.
- Quién es el primer colaborador, la persona que está desarrollando el *plugin*.
- En qué lenguaje lo escribiremos. Soportándose **Dogma** y **JavaScript**.
- Qué tipo de *plugin* crear: simple o compuesto.
  Tenga muy claro qué tipo de *plugin* va a usar.
  En caso de duda, utilice uno compuesto.
- Cuál es su repositorio **Git**.

### Archivos importantes

Cuando se define un *plugin*, los archivos más importantes son:

- `package.json`. Contiene los metadatos del paquete NPM.
- `src/plugin.js` o `src/plugin.dog`. Expone las tareas del *plugin*.
- `src/tarea.js` o `src/tarea.dog`. Contiene la definición de una función de tarea.

Lo mejor es consultar el código de algún *plugin* oficial como, por ejemplo:

- [justo.plugin.eslint](https://bitbucket.org/justorocks/justo-plugin-eslint). Ejemplo de *plugin* simple redactado en **JavaScript**.
- [justo.plugin.chrome](https://bitbucket.org/justorocks/justo-plugin-chrome). Ejemplo de *plugin* compuesto redactado en **JavaScript**.
- [justo.plugin.cli](https://bitbucket.org/justorocks/justo-plugin-cli). Ejemplo de *plugin* simple redactado en **Dogma**.
- [justo.plugin.user](https://bitbucket.org/justorocks/justo-plugin-user). Ejemplo de *plugin* compuesto redactado en **Dogma**.

### Añadidura de tareas a plugins compuestos

La mejor manera para añadir una tarea a un *plugin* compuesto es utilizar el comando `add task` del generador `justo.generator.plugin`, tal como se muestra a continuación:

```
$ justojs g plugin add task
```

Una vez contestadas las preguntas del generador, éste creará:

- Una entrada en el archivo `src/plugin.dog` o `src/plugin.js` para la nueva tarea.
  Se recomienda ir al archivo y modificar la propiedad `fmt` para personalizar el título.
- Un archivo para la función de la nueva tarea, en el directorio `src`.

He aquí un ejemplo ilustrativo:

```
justo-plugin-docker$ justojs g plugin add task
? Task id images
? Task description List Docker images.
? Is it SSH-compatible? No
? Programming language Dogma

Generate
  Generate src/images.dog from template OK (20 ms)
  Append content to src/plugin.dog OK (1 ms)

 OK       2
 FAILED   0
 TOTAL    2

 29 ms

justo-plugin-docker$
```

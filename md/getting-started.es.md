# Introducción a Justo

*Tiempo estimado de lectura: 25min*

**Justo** es una **plataforma de automatización IT** que presenta las siguientes características:

- Es **fácil de aprender y de usar**.
- Proporciona un **motor de tareas** (*task runner*) al igual que hacen otros como, por ejemplo, **Grunt**, **Gulp** o **Puppet**.
- Proporciona **distintos tipos de tarea**, para **adaptarse a las necesidades** de los usuarios como, por ejemplo, tareas simples, macros y flujos de trabajo.
- Proporciona un **framework de pruebas de software** (*software testing framework*) al igual que hacen **Jasmine** y **Mocha**.
- Proporciona una **biblioteca de aserciones** (*assertion library*) al igual que hacen **Should** y **Chai**.
- Tiene soporte integrado para **SSH**, lo que permite ejecutar tareas en **máquinas remotas** tal como hacen **Ansible** y **Salt**.
- Proporciona **generadores** (*generators*) al igual que hace **Yeoman**.
- Se puede **extender su funcionalidad** mediante **JavaScript** y **Dogma**, y próximamente mediante **Python**, usando **generadores**, **plugins** y **módulos**.

Aunque puede utilizarse en varios ámbitos, está dirigido principalmente a:

- **Desarrolladores de software** (*software developers*).
- **Administradores de sistemas** (*system administrators*).
- **DevOps**.
- **Administradores de bases de datos** (*DBAs*).

Para la edición **Justo/JS**, el usuario tiene que conocer **JavaScript** y **Node.js**.
Mientras que para **Justo/Py**, pendiente de publicación, **Python**.

## Qué es la automatización IT

La **automatización** (*automation*) no es más que el proceso mediante el cual se convierte tareas manuales repetitivas en automáticas.
Al automatizarlas, podemos ejecutarlas más fácil y rápidamente que si tuviéramos que hacerlo una y otra vez manualmente.
Por su parte, la **automatización IT** (*IT automation*) es aquella que se centra en automatizar tareas y/o procesos de IT.

Entre los principales usos de la automatización IT encontramos:

- Construcción de software.
- Despliegue o instalación de software.
- Realización de pruebas.
- Administración y monitorización de configuración.
- Realización de empaquetados.
- Extracción, transformación y carga de datos.
- Generación de archivos y/o directorios a partir de plantillas.

Y entre las principales ventajas:

- Aumento de la productividad.
- Aumento de la calidad.
- Aumento de la precisión o exactitud.
- Aumento de la robustez.
- Reducción de costes.
- Reducción de errores.
- Reducción de riesgos.
- Reducción de tareas manuales.
- Reducción de tiempos de ejecución, de operación, de despliegue...
- Reducción de mantenimiento.
- Reducción de la duplicidad de trabajos.
- Reducción de la complejidad.

**Justo** se ha desarrollado en **Dogma**, [dogmalang.com](http://dogmalang.com), y se ha compilado a **JavaScript**, conociéndose esta edición formalmente como **Justo/JS**.
A lo largo de **2018**, se compilará también a **Python**, publicándose la edición **Justo/Py**.
Así, **Justo** proporcionará la **misma API** y la **misma forma de trabajo** en las dos plataformas de *scripting* más conocidas y utilizadas hoy en día.
Facilitando considerablemente la migración de una plataforma a otra con menor esfuerzo y menor curva de aprendizaje.
No siendo necesario aprender otra herramienta de automatización IT por el mero hecho de cambiar de Python a Node.js o viceversa.

## Arquitectura

La **arquitectura** (*architecture*) describe el conjunto y la estructura de componentes de algo, en nuestro caso, de **Justo**.
La arquitectura de **Justo** está formada básicamente por los siguientes elementos:

- Las **tareas** (*tasks*), las cuales representan trabajos o actividades a ejecutar.
- El **motor de tareas** (*task runner*), la aplicación que ejecuta y supervisa las tareas.
- El **informador de resultados** (*result reporter*), aquel que informa de las tareas ejecutadas, mostrando el tiempo que tardaron en ejecutarse y el estado de su terminación.
- El **catálogo de tareas** (*task catalog*), un índice de tareas disponibles para su ejecución desde la línea de comandos.

Veamos unos ejemplos introductorios.
En primer lugar, vamos a ver cómo mostrar las tareas del catálogo asociado a un proyecto de software:

```
$ justojs

 Id.          Type      Desc.                      
---------------------------------------------------
 build        Macro     Build package              
 dflt         Macro     Lint, build and test       
 install      Simple()  Install package globally   
 lint         Macro     Lint source code           
 make         Macro     Lint and build             
 test         Macro     Unit testing               
 trans-dogma  Macro     Transpile from Dogma to JS
 trans-js     Macro     Transpile from JS to JS    

$
```

Y a continuación, cómo ejecutar la tarea predeterminada, aquella cuyo identificador es `dflt`:

```
$ justojs r

dflt
----
Lint, build and test
  Lint source code
    Check src folder OK (127 ms)
    Check test folder OK (434 ms)
  Build package
    Remove ./build OK (4 ms)
    Remove ./dist OK (1 ms)
    Transpile from Dogma to JS
      Transpile src OK (132 ms)
      Transpile test OK (412 ms)
    Transpile from JS to JS
      Transpile src OK (1548 ms)
    Copy package.json to dist/justo.spy/package.json OK (2 ms)
    Copy README.js.md to dist/justo.spy/README.md OK (1 ms)
  Unit testing
    justo.spy
      api
        test(fn) OK (1 ms)
      spy() - not being spied
        test(fn) OK (1 ms)
    Call
      constructor
        constructor(value)
          test(fn) OK (1 ms)
        constructor(nil, error)
          test(fn) OK (0 ms)
    field spy
      spy.obj(obj, mem)
        test(fn) OK (6 ms)
      spy(obj, mems)
        test(fn) OK (0 ms)
      read
        test(fn) OK (4 ms)
      write
        test(fn) OK (1 ms)
      r/w
        test(fn) OK (1 ms)
      not being spied
        call()
          init(Create spy) OK (0 ms)
          test(fn) OK (0 ms)
        called()
          init(Create spy) OK (0 ms)
          test(fn) OK (0 ms)
        returned()
          init(Create spy) OK (0 ms)
          test(fn) OK (0 ms)
        alwaysReturned()
          init(Create spy) OK (0 ms)
          test(fn) OK (0 ms)
        raised()
          init(Create spy) OK (0 ms)
          test(fn) OK (0 ms)
        alwaysRaised()
          init(Create spy) OK (0 ms)
          test(fn) OK (0 ms)
    function spy
      spy.func(func) - never called
        test(fn) OK (1 ms)
      spy.func()
        test(fn) OK (5 ms)
      spy.func(fn) - always returned
        test(fn) OK (2 ms)
      spy.func(fn) - always returned the same
        test(fn) OK (1 ms)
      spy.func(fn) - always called with the same
        test(fn) OK (1 ms)
      spy.func(fn) - always raised
        test(fn) OK (1 ms)
      spy.func(fn) - always raised the same
        test(fn) OK (1 ms)
      (always)calledWith()
        calledWith() - always the same
          test(fn) OK (1 ms)
        calledWith() - always not the same
          test(fn) OK (2 ms)
    method spy
      check private attributes
        spy.obj(object, member)
          test(fn) OK (1 ms)
        spy.obj(object, members)
          test(fn) OK (0 ms)
      Call to method spy - always returning
        test(fn) OK (2 ms)
      Call to method spy - always raising
        test(fn) OK (2 ms)
      Call to method spy - always raising
        test(fn) OK (1 ms)
      Not being spied
        call()
          init(Create spy) OK (0 ms)
          test(fn) OK (0 ms)
        called()
          init(Create spy) OK (0 ms)
          test(fn) OK (0 ms)
        returned()
          init(Create spy) OK (0 ms)
          test(fn) OK (0 ms)
        alwaysReturned()
          init(Create spy) OK (1 ms)
          test(fn) OK (0 ms)
        raised()
          init(Create spy) OK (0 ms)
          test(fn) OK (0 ms)
        alwaysRaised()
          init(Create spy) OK (0 ms)
          test(fn) OK (0 ms)
    object spy
      invalid member format
        test(fn) OK (1 ms)
      spying fields and methods
        test(fn) OK (1 ms)
      alwaysCalledWith()
        alwaysCalledWith() : true
          test(fn) OK (0 ms)
        alwaysCalledWith() : false
          test(fn) OK (0 ms)
        alwaysCalledWith() - not being spied
          test(fn) OK (1 ms)
      calledWith() - not being spied
        test(fn) OK (0 ms)

 OK       62
 FAILED   0
 TOTAL    62

 2807 ms

$
```

## Paquete justo

El paquete `justo` representa el punto de entrada a **Justo** por parte de los usuarios.
En **Justo/JS**, se encuentra disponible para su instalación mediante `npm`.

## Tareas

Una **tarea** (*task*) representa un trabajo o actividad a ejecutar como, por ejemplo:

- El proceso de compilación.
- La minimización de código JavaScript, HTML o CSS.
- La realización de una copia de seguridad.
- El alta de un nuevo usuario.
- La ejecución de un comando APT o APK en una máquina remota.
- El inicio de uno o más contenedores Docker.

Se distingue entre tareas simples y compuestas.

### Tareas simples

Una **tarea simple** (*simple task*) es la unidad de operación más pequeña que tiene como objeto realizar un determinado trabajo indivisible.
En **Justo/JS**, se implementa mediante una función **JavaScript**.
Su ejecución la lanza el ejecutor de tareas de **Justo**, el cual recopilará información sobre su duración y su resultado final.
Cualquier operación o función, cuya ejecución deseamos sea supervisada por el motor de **Justo**, debe definirse como una tarea simple.

#### Definición de tareas simples

Una tarea simple se define mediante la función `simple()` del paquete `justo`:

```
justo.simple(props, fn) : Simple
```

El parámetro `props` puede ser el identificador de la tarea, en forma de texto, o bien un objeto con las propiedades de la tarea:

- `id` (string, obligatorio). Identificador de la tarea.
- `title` (string). Título predeterminado de la tarea que se utilizará en el informe final.
- `fmt` (function). Función que debe utilizar el motor para obtener el título dinámicamente: `function(params) : string`.
- `desc` (string). Descripción del objeto de la tarea.
- `tags` (string[]). Etiquetas asociadas a la tarea.
  Se utilizan para filtrar tareas a ejecutar.
- `delay` (number). Indica cuánto tiempo, en milisegundos, debe esperar el motor de tareas antes de comenzar su ejecución.

Por su parte, el parámetro `fn` contiene la **función de tarea** (*task function*), aquella que contiene la lógica que realiza el trabajo asociado a la tarea.
Esta función puede tener varios parámetros:

- `params` (object). Parámetros pasados a la tarea, en la invocación del usuario.
  Las tareas que presentan este parámetro se conocen formalmente como **tareas parametrizadas** (*parameterized tasks*).
- `ssh` (function). Función que debe utilizar la tarea para ejecutar un comando mediante SSH en una máquina remota.
  Las tareas que presentan este parámetro se conocen formalmente como **tareas compatibles con SSH** (*SSH-compatible tasks*).
- `done` (function). Función que debe invocar la tarea para indicar que ha finalizado.
  Las tareas que presentan este parámetro se conocen formalmente como **tareas asíncronas** (*asyncrhonrous tasks*).
  En otro caso, como **tareas síncronas** (*synchronous tasks*).
- `log` (function). Función a través de la cual escribir mensajes de *log* en la consola.

Ninguno de los parámetros es obligatorio.
Y cuando son necesarios, se pueden indicar en cualquier orden.
Las funciones de tarea utilizan parámetros nominales, no ordinales.
Y por lo tanto, el orden de los parámetros no alterará el producto.

A continuación, se muestra un ejemplo ilustrativo mediante el cual se crea una tarea simple para la supresión de un archivo local:

```
rm = justo.simple("rm", function(params) {
  fsx.removeSync(params.path);
});
```

Adicionalmente, se puede utilizar varios métodos de la tarea para definir algunas de sus propiedades, todos ellos devolviendo la propia tarea para facilitar su encadenamiento:

- `fmt(function) : Task`. Indica la función con la que obtener dinámicamente el título de una invocación a partir de los parámetros pasados por el usuario: `function(params) : string`.
- `title(string) : Task`. Establece el título predeterminado de la tarea.
- `desc(string) : Task`. Establece la descripción de la tarea.
- `tags(string[]) : Task`. Establece las etiquetas de la tarea.

Vamos a ver unos ejemplos ilustrativos:

```
//una manera
rm = justo.simple(
  {
    id: "rm",
    desc: "Remove a file or a folder.",
    fmt: function(params) { return "Remove " + params.path; }
  },

  function(params) {
    fsx.removeSync(params.path);
  }
);

//otra manera
rm = justo.simple("rm", function(params) {
  fsx.removeSync(params.path);
}).desc("Remove a file or a folder.").fmt(function(params) {
  return "Remove " + params.path;
});
```

#### Tareas simples asícnronas

Las tareas simples se clasifican en síncronas o asíncronas.
De manera predeterminada, son síncronas.
Para definirlas como asíncronas no hay más que indicar el parámetro `done` en la función de tarea.

En una tarea síncrona, cuando la función de tarea finaliza, lo hace también su trabajo asociado.
En cambio, en una asíncrona, la tarea no finaliza con la función, sino cuando ha finalizado su trabajo asíncrono.
Por esta razón, el motor de tareas se queda esperando hasta que la función de tarea le indique que ha finalizado.
Y para este fin, le pasa una función mediante el parámetro `done`, la cual debe usar para indicarle dos cosas: por un lado, que ha terminado; y por otro lado, cómo ha finalizado.
Presenta las siguientes sobrecargas:

```
done()                //he finalizado bien y no devuelvo nada
done(error)           //he finalizado con el siguiente error
done(null, resultado) //he finalizado bien y he aquí el valor a devolver
```

### Invocación de tareas

La invocación de tarea es independiente de su tipo.
Se invocan siempre como funciones, con un único parámetro de tipo objeto.
He aquí un ejemplo ilustrativo mediante el cual se invoca la tarea `rm` definida anteriormente:

```
rm({path: "myfile.txt"});
```

Algunos de los campos del objeto `params` son internos de **Justo** y no deben ser usados ni esperados por las funciones de tarea:

- `title` (string). Define el título explícito a utilizar en el informe de resultados.
  Si no se indica, el motor de tareas lo determinará consultando las propiedades de la tarea en el siguiente orden: `fmt`, `title` e `id`.
- `ignore` (bool). Indica si debe omitirse la invocación.
  Es muy útil para omitir ejecuciones de tarea, por ejemplo, atendiendo al sistema operativo.
- `target` (string). Indica el *target* en el que debe ejecutarse la tarea.

He aquí otro ejemplo:

```
docker.run({
  image: "justo/alpinessh:latest",
  container: "alpine22",
  detach: true,
  publish: "127.0.0.1:22222:22",
  rm: true
});
```

#### Invocación de tareas asíncronas

De cara al usuario, no es importante si la tarea es síncrona o asíncrona.
Es una cuestión interna del motor de tareas y de la propia tarea.
Para el usuario, lo único que debe recordar es que hasta que no finalice la tarea, no se pasará a lo siguiente.

### Macros

Recordemos que las tareas se clasifican en simples y compuestas.
La simples son aquellas que son indivisibles y representan la unidad más pequeña de trabajo.
Por su parte, una **tarea compuesta** (*composite task*) es aquella que está formada por cero, una o más tareas.
Se clasifican en macros y flujos de trabajo.

Una **macro** es una secuencia ordenada de cero, una o más tareas.
Cada vez que se invoca la macro, se ejecuta cada una de sus tareas en el orden indicado.

#### Definición de macros

Para definir una macro, se utiliza la función `macro()` del paquete `justo`:

```
justo.macro(props, tasks) : Macro
```

El parámetro `props` es similar al presentado en las tareas simples.
Mientras que el parámetro `tasks` consiste en una lista o *array* de las tareas asociadas a la macro.
He aquí un ejemplo ilustrativo:

```
dflt = justo.macro("dflt", [
  catalog.get("lint"),
  catalog.get("build"),
  catalog.get("test")
]).title("Lint, build and test");
```

Cada tarea se puede especificar de dos maneras distintas.
Por un lado, mediante la propia tarea.
Es el caso del ejemplo anterior, el cual utiliza el catálogo para obtener cada una de las subtareas.

Por otro lado, podemos indicar un objeto que contiene la información sobre la invocación de la tarea:

- `title` (string). Contiene el título de la invocación de la tarea. Se usa en el informe final.
- `task` (Task, obligatorio). Contiene la tarea a invocar.
- `params` (object). Contiene los parámetros a pasar a la tarea cuando la invoque la macro.

Ejemplo:

```
catalog.macro("lint", [
  {title: "Check Dogma code", task: cli, params: {cmd: "dogmac lint -l src test"}},
  {title: "Check JavaScript code", task: eslint, params: {src: "."}}
]).title("Lint source code");
```

Recordemos que una macro se invoca de manera similar a una tarea simple o cualquier otro tipo, mediante una llamada a función.
Si en esta invocación pasamos un objeto `params`, la macro lo pasará a sus subtareas; no usará el campo `params` definido.
Éste sólo se utiliza cuando no se pasa parámetros a la macro.

### Macros diferidas

Una **macro diferida** (*deferred macro*) es aquella cuyas subtareas asociadas se determinan cuando se invoca por primera vez.
Estas tareas a su vez proceden de un directorio, donde cada uno de sus archivos representa un módulo, el cual debe exportar una definición de tarea.

#### Definición de macros diferidas

Para definir una macro diferida, se utiliza la función `macro()`, pero ahora se pasa el directorio en el que se encuentran sus tareas, en vez de una lista de tareas:

```
justo.macro(props, dir) : DeferredMacro
```

Ejemplo:

```
catalog.macro("test", `./dist/${PKG}/test/unit`).title("Unit testing");
```

Cuando el directorio comienza por punto (`.`) o por dos puntos (`..`), la macro asume que es relativo al directorio actual de trabajo del proceso.
Lo anterior es lo mismo que:

```
catalog.macro("test", path.join(process.cwd(), `dist/${PKG}/test/unit`)).title("Unit testing");
```

### Flujos de trabajo

Un **flujo de trabajo** (*workflow*) es una tarea compuesta que permite ejecutar unas tareas u otras atendiendo a un determinado flujo de ejecución.
En **Justo/JS**, este flujo se define mediante una función **JavaScript**, donde se puede usar sentencias `if`, `while`, `for` o cualquiera otra y donde se puede invocar cualquier tarea.
Cada tarea invocada dentro del flujo de trabajo se asociará a él y así se mostrará en el informe final.

Observe que a diferencia de las macros, donde sus tareas se invocan una detrás de otra, según el orden en el que se indican, en un flujo de trabajo las tareas se invocan cuando es necesario y si es necesario.

#### Definición de flujos de trabajo

Para definir un flujo de trabajo, se usa la función `workflow()` del paquete `justo`:

```
justo.workflow(props, fn) : Workflow
```

El parámetro `props` es similar al presentado en las tareas simples y macros.
Mientras que el parámetro `fn` consiste en la función de tarea que describe el flujo de trabajo.
Esta función puede tener varios parámetros:

- `params` (object). Parámetros pasados a la tarea, en la invocación del usuario.
- `target` (object). Parámetro que contiene la descripción del *target* SSH.
- `done` (function). Función que debe invocarse para indicar que la tarea asíncrona ha finalizado.
- `log` (function). Función a través de la cual escribir mensajes de *log* en la consola.

No olvide que, en **Justo**, los parámetros son nominales y no posicionales.
Respete los nombres indicados y ubique los parámetros donde mejor le venga.

A continuación, se muestra cómo instalar **Lua** en una máquina mediante SSH y un flujo de trabajo:

```
installLua = justo.workflow("install-lua", function(target) {
  apk.update({target});

  if (!apk.installed({target, pkg: "lua5.3"})) {
    if (apk.available({target, pkg: "lua5.3"})) {
      apk.add({target, pkg: "lua5.3"});
    }
  }
}).title("Install Lua 5.3");
```

He aquí otro ejemplo con el que detener varios contenedores **Docker** mediante un flujo de trabajo:

```
stop = justo.workflow("stop", function() {
  for (let container of ["contenedor1", "contenedor2"]) {
    docker.stop({container});
  }
}).title("Stop the containers");
```

### Llamadas

Una **llamada** (*call*) es un tipo especial de tarea que representa una invocación a una determinada tarea con unos determinados parámetros.
Cada vez que se invoque la tarea, se invocará su tarea adjunta con los parámetros indicados en su definición.

#### Definición de llamadas

Para definir una llamada, se utiliza la función `call()` del paquete `justo`:

```
justo.call(props, task, params) : Call
```

El parámetro `props` es similar al presentado en las tareas simples, macros y flujos de trabajo.
Mediante el parámetro `task` se indica la tarea a invocar.
Y con `params`, los parámetros a pasarle a la tarea a invocar.

Vamos a ver un ejemplo ilustrativo:

```
install = justo.call("install", npm.install, {
  pkg: `./dist/${PKG}`,
  global: true
}).title("Install package globally");
```

### Tareas iterativas

Una **tarea iterativa** (*iterative task*) es aquella que define una secuencia de parámetros con los que ejecutar la tarea cada vez que se invoque.
Es una manera muy sencilla de ejecutar una misma acción con distintos parámetros.
Para este fin, las tareas proporcionan el método `forEach()`, el cual devuelve la propia tarea para su encadenamiento:

```
forEach(...args) : Task
```

Veamos un ejemplo:

```
rmfiles = justo.simple("rm", function(params) {
  fsx.removeSync(params.path);
}).fmt(function(params) { return "Remove " + params.path; }).forEach(
  {path: "file1.txt"},
  {path: "file2.txt"},
  {path: "file3.txt"}
);
```

Cada vez que el motor de tareas ejecute una tarea iterativa, la invocará una vez para cada uno de los parámetros indicados mediante su método `forEach()`.

Los argumentos del método `forEach()` deben de ser objetos, con dos posibles campos:

- `subtitle` (string). El texto a añadir al título de la invocación.
  Si no se indica, se utilizará una representación en formato JSON de `params`.
- `params` (object). Los parámetros a pasar en su invocación.

Como es muy común pasar sólo los parámetros, el método ha relajado su especificación.
De tal manera que si no se pasa un objeto, el método lo convertirá a un objeto como el siguiente: `{params: {value: valorPasado}}`.
Así pues, `forEach(123, "hola")`, es lo mismo que `forEach({params: {value: 123}}, {params: {value: "hola"}})`.

Por otra parte, si se pasa un objeto, pero sin campo `params`, el método lo convertirá a `{params: objeto}`.
Por lo tanto, `forEach({path: "file1.txt"}, {path: "file2.txt"})` es lo mismo que `forEach({params: {path: "file1.txt"}}, {params: {path: "file2.txt"}})`.

## Módulos

Un **módulo** (*module*) es uno de los medios a través de los cuales extender la funcionalidad de **Justo**.
Se distingue entre módulo de catálogo y módulo de flujo de trabajo.

### Módulos de catálogo

Un **módulo de catálogo** (*catalog module*) o simplemente **catálogo** (*catalog*) es un contenedor donde se ha registrado un conjunto de tareas para su invocación desde la línea de comandos.
Un **catálogo integrado** (*embedded catalog*) es aquel que se asocia a un determinado proyecto, conteniendo tareas específicas suyas.
Mientras que un **catálogo NPM** (*NPM catalog*) es aquel que se implementa mediante un módulo **NPM** y se puede publicar e instalar con `npm`.

#### Definición de un catálogo integrado

Para definir un catálogo integrado, no hay más que crear su archivo de catálogo.
La manera más fácil de hacerlo es mediante el generador `justo.generator.catalog`:

```
$ justojs g catalog embedded
? Catalog file name Justo.cat.js
? Would you like to generate the package.json file? Yes
? Run 'npm install' Yes
? Would you like to generate the .eslintrc file? Yes

Generate
  Generate Justo.cat.js OK (2 ms)
  Generate .eslintignore OK (0 ms)
  Generate .eslintrc OK (0 ms)
  Generate package.json OK (0 ms)
  Run 'npm install' OK (11982 ms)

 OK       5
 FAILED   0
 TOTAL    5

 11990 ms

$
```

El generador solicita datos al usuario y, a continuación, con la información recolectada, genera el archivo de catálogo y otros si fuera necesario.

Cosas a tener en cuenta:

- El archivo de catálogo puede tener cualquier nombre, tal como veremos en breve.
- El archivo `package.json` debe tener como dependencia el paquete `justo` y cualquier otro relacionado con *plugins* usados en el archivo de catálogo.
  Cuando no se dispone de este archivo, se puede indicar al generador que lo genere y, entonces, lo hará por nosotros.

Si en algún momento olvida el nombre exacto del comando del generador, puede mostrar sus comandos disponibles como sigue:

```
$ justojs g catalog -h

 Command   Description
------------------------------------------------
 embedded  Generate an embedded catalog module.
 npm       Generate a NPM catalog module.

$
```

#### Definición de un catálogo NPM

Para definir un catálogo NPM, hay que crear un proyecto NPM y, si lo deseamos, también subirlo a este repositorio.
La manera más fácil de crear la estructura de este tipo de proyecto es mediante el generador `justo.generator.catalog`:

```
$ justojs g catalog npm
```

Para hacer el mejor trabajo posible, el generador realizará una batería de preguntas para recolectar información.
Una vez terminado, creará la estructura inicial del módulo NPM.

Este tipo de catálogo tiene varios archivos importantes:

- `package.json`, en el que se proporciona metadatos del catálogo.
  Utiliza la sintaxis de NPM.
  No añade nada especial.
- El archivo del catálogo, que presentaremos en breve.

La propiedad `main` del archivo `package.json` debe indicar el archivo del catálogo.
Por convenio y buenas prácticas, se recomienda `Justo.cat.js`.

Como **Justo** puede trabajar con varios catálogos, observará dos archivos de catálogo en la estructura de directorios creada:

- `Justo.cat.js`. Catálogo principal del módulo y expuesto para su utilización por parte de los usuarios.
- `Justo.dev.js`. Catálogo de tareas internas del proyecto.
  Para su uso sencillo, `justojs` dispone de la opción `-d`.
  Para acceder a él, puede usar `-d` o bien `-c ./Justo.dev.js`.
  Son equivalentes.

#### Comando justojs

En **Justo/JS**, el comando `justojs` es el punto de entrada del usuario para ejecutar tareas desde la línea de comandos.
Se encuentra disponible mediante el paquete **NPM** `justojs`.

Para listar las tareas del catálogo integrado predeterminado, esto es, el definido en el archivo `Justo.cat.js` del directorio actual:

```
$ justojs

 Id.          Type      Desc.                      
---------------------------------------------------
 build        Macro     Build package              
 dflt         Macro     Lint, build and test       
 install      Simple()  Install package globally   
 lint         Macro     Lint source code           
 make         Macro     Lint and build             
 test         Macro     Unit testing               
 trans-dogma  Macro     Transpile from Dogma to JS
 trans-js     Macro     Transpile from JS to JS    

$
```

Para indicar otro catálogo, usar la opción `-c módulo`.
A continuación, se muestra cómo listar las tareas del catálogo integrado `Justo.dev.js` del directorio actual, aunque recordemos que para este catálogo se puede usar la opción más corta `-d`:

```
$ justojs -c ./Justo.dev.js

 Id.      Type      Desc.                    
---------------------------------------------
 install  Simple()  Install package globally
 lint     Macro     Lint source code         

$
```

Y si lo que deseamos es listar un catálogo NPM instalado con `npm`, indicar su nombre:

```
$ justojs -c luafordogma

 Id.                  Type      Desc.                             
------------------------------------------------------------------
 dflt                 Macro     Install Lua and LuaRocks          
 lua                  Macro     Install Lua                       
 lua-compi-test       Simple()  Test Lua compilation              
 lua-decompress       Simple()  Decompress Lua                    
 lua-download         Simple()  Download Lua                      
 lua-install          Simple()  Install Lua                       
 lua-make             Simple()  Make Lua                          
 lua-test             Simple()  Check whether Lua accessible      
 luarocks             Macro     Install LuaRocks                  
 luarocks-conf        Simple()  Configure LuaRocks for compiling  
 luarocks-decompress  Simple()  Decompress LuaRocks               
 luarocks-download    Simple()  Download LuaRocks                 
 luarocks-install     Simple()  Install LuaRocks                  
 luarocks-make        Simple()  Make LuaRocks                     
 luarocks-test        Simple()  Check whether LuaRocks accessible

$
```

#### Archivo de catálogo

El **archivo de catálogo** (*catalog file*) es aquel que define y registra las tareas del catálogo.
Para ello, hay que utilizar el objeto `catalog` del paquete `justo`, el cual proporciona métodos homónimos para cada tipo de tarea:

- `catalog.simple(props, fn) : Simple`, define y registra una tarea simple en el catálogo.
- `catalog.macro(props, tasks): Macro`, define y registra una macro en el catálogo.
- `catalog.workflow(props, fn) : Workflow`, define y registra un flujo de trabajo en el catálogo.
- `catalog.call(props, task, params) : Call`, define y registra una llamada en el catálogo.

Para acceder a una determinada tarea catalogada, se puede utilizar el método `get()` del catálogo:

```
catalog.get(id) : Task
```

A continuación, se muestra un ejemplo ilustrativo:

```
//imports
const {catalog} = require("justo");
const babel = require("justo.plugin.babel");
const cli = require("justo.plugin.cli");
const eslint = require("justo.plugin.eslint");
const fs = require("justo.plugin.fs");
const npm = require("justo.plugin.npm");

//internal data
const PKG = "justo.generator.catalog";

//catalog
catalog.macro("lint", [
  {title: "Check Dogma code", task: cli, params: {cmd: "dogmac lint -l src test"}},
  {title: "Check JavaScript code", task: eslint, params: {src: "."}}
]).title("Lint source code");

catalog.macro("trans-dogma", [
  {title: "Transpile src", task: cli, params: {cmd: "dogmac js -o build/src src"}},
  {title: "Transpile test", task: cli, params: {cmd: "dogmac js -o build/test/unit test/unit"}}
]).title("Transpile from Dogma to JS");

catalog.macro("trans-js", [
  {task: babel, params: {src: "build", dst: `dist/${PKG}/`}}
]).title("Transpile from JS to JS");

catalog.macro("build", [
  {task: fs.rm, params: {path: "./build"}},
  {task: fs.rm, params: {path: "./dist"}},
  {task: catalog.get("trans-dogma")},
  {task: catalog.get("trans-js")},
  {task: fs.move, params: {src: `dist/${PKG}/src/index.js`, dst: `dist/${PKG}/index.js`}},
  {task: fs.copy, params: {src: "package.json", dst: `dist/${PKG}/package.json`}},
  {task: fs.copy, params: {src: "README.js.md", dst: `dist/${PKG}/README.md`}},
  {task: fs.copy, params: {src: "tmpl", dst: `dist/${PKG}/tmpl`}},
]).title("Build package");

catalog.macro("make", [
  catalog.get("lint"),
  catalog.get("build")
]).title("Lint and build");

catalog.call("install", npm.install, {
  pkg: `dist/${PKG}`,
  global: true
}).title("Install package globally");

catalog.call("pub", npm.publish, {
  who: "justojs",
  path: `dist/${PKG}`
}).title("Publish in NPM");

catalog.macro("dflt", [
  catalog.get("lint"),
  catalog.get("build")
]).title("Lint and build");
```

#### Ejecución de tareas catalogadas

Para ejecutar una tarea catalogada, en **Justo/JS**, hay que utilizar el comando `justojs r`.
Para mostrar su ayuda, utilice `justojs r help`.

Groso modo, necesitamos el catálogo a utilizar, el cual se puede indicar mediante la opción `-c`; en caso de no indicarse, se utilizará `./Justo.cat.js`.
Y las tareas a ejecutar; que en caso de no indicarse, se ejecutará la **tarea predeterminada** (*default task*), aquella registrada en el catálogo como `dflt`.
La tarea a invocar debe tener alguna de las siguientes sintaxis:

```
id
id,param1=val1,param2=val2...
```

Observe que se puede pasar su objeto `params`, cuando sea necesario, mediante la línea de comandos.
Se adjuntan al identificador de la tarea, sin separarse en ningún momento por espacios, y usando la coma (`,`) como separador.

A continuación, se muestra algunos ejemplos ilustrativos, usando el catálogo integrado `./Justo.cat.js`:

```
justojs r                     #ejec. la tarea predeterminada (dflt)
justojs r uno dos             #ejec. las tareas uno y dos
justojs r uno,p1=123,p2=321   #ejec. la tarea uno pasando {p1: 123, p2: 321} a params
```

Y ahora, más ejemplos, pero usando un catálogo instalado con `npm`:

```
justojs r -c luafordogma      #ejec. la tarea predeterminada (dflt)
justojs r lua -c luafordogma  #ejec. la tarea lua
```

### Módulos de flujo de trabajo

Un **módulo de flujo de trabajo** (*workflow module*) es aquel que define un flujo de trabajo, en vez de un catálogo de tareas.
En **Ansible**, sería similar al concepto de *playbook*.
En **Justo/JS**, se define como un módulo **NPM**.

#### Definición de un módulo de flujo de trabajo

Para definir un módulo de este tipo, se recomienda utilizar el generador `justo.generator.workflow`:

```
$ justojs g workflow
```

En **Justo/JS**, la propiedad `main` del archivo `package.json` debe referenciar al módulo que define el flujo de trabajo.
El cual se definirá mediante la función `workflow()` del paquete `justo`.
Veamos un ejemplo de plantilla con la que comenzar, si lo definimos mediante la especificación ES2015:

```
//imports
import {workflow} from "justo";

//flujo de trabajo
export default workflow("id", function(params) {

});
```

Ahora bien, si lo hacemos mediante Node.js:

```
//imports
const {workflow} = require("justo");

//flujo de trabajo
module.exports = exports = workflow("id", function(params) {

});
```

#### Ejecución de módulo de flujo de trabajo

Para ejecutar el flujo de trabajo, en **Justo/JS**, usaremos `justojs r`, indicando el módulo mediante la opción `-w`:

```
$ justojs r -w módulo
```

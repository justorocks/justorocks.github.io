# Automatización de pruebas con Justo

*Tiempo estimado de lectura: 10min*

**Justo** viene de fábrica con un *framework de pruebas de software*.
Proporcionando varios tipos de tarea que tienen como objeto facilitar las pruebas de componentes de software.
De esta manera, **Justo** tiene capacidades similares a otros productos dedicados a las pruebas como, por ejemplo, **Jasmine** o **Mocha**.
En **Justo**, con la misma interfaz y forma de trabajo, podemos definir varios tipos de tarea de automatización.
Consiguiendo así una reducción considerable de la curva de aprendizaje y del número de productos a usar.
Mejorando nuestra productividad con objeto de poder dedicar más tiempo a las pruebas o a otras cosas.

Para la parte de pruebas, **Justo** proporciona los siguientes tipos de tarea de prueba:

- **Suite**. Un grupo de tareas relacionadas con un mismo aspecto.
- **Prueba**. Una tarea que realiza una determinada operación de prueba.
- **Inicializador**. Una tarea simple que tiene como objeto inicializar el entorno para una o más pruebas.
- **Finalizador**. Una tarea simple que tiene como objeto poner fin a algo.

## Suites

Una **suite** es una tarea compuesta que define un grupo de tareas de prueba relacionadas con un determinado aspecto como, por ejemplo, la prueba de una determinada clase, método o función.
Es similar a los `describe()` de **Jasmine** y **Mocha**.
Está formada por:

- Cero, una o más subsuites.
- Cero, una o más pruebas.
- Cero, uno o más inicializadores.
- Cero, uno o más finalizadores.

### Definición de suites

Para definir una suite, hay que utilizar la función `suite()` del paquete `justo`:

```
justo.suite(props, fn) : Suite
```

Las propiedades son similares a las de las tareas simples, las macros o los flujos de trabajo.
Mientras que el parámetro `fn` es una función **JavaScript** que define el contenido de la suite.
Esta función se conoce formalmente como **función de definición de la suite** (*suite definition function*).

Cuando una suite se define dentro de otra, ésta se asigna a su suite contenedora.

A continuación, se muestra un ejemplo ilustrativo para abrir boca:

```
//imports
import {suite, test} from "justo";
import assert from "justo.assert";
import Call from "../../../justo.spy/src/Call";

//Suite.
export default suite("Call", function() {
  suite("constructor", function() {
    test("constructor(value)", function() {
      assert(new Call(123)).has({
        value: 123,
        error: null
      });
    });

    test("constructor(null, error)", function() {
      assert(new Call(null, 123)).has({
        value: null,
        error: 123
      });
    });
  });
});
```

## Pruebas

Una **prueba** (*test*) representa una operación que hace algo con objeto de comprobar que lo hace bien, generalmente consultando los resultados de sus acciones.

### Definición de pruebas

Se define mediante la función `test()` del paquete `justo` y es similar a las funciones `it()` de **Jasmine** y **Mocha**:

```
justo.test(props, fn) : Test
```

El parámetro `props` define las propiedades de la tarea de prueba.
Es similar al parámetro homónimo de las tareas simples y compuestas.
Y el parámetro `fn` contiene la operación que debe ejecutar el motor de tareas cada vez que se realice la prueba.
Su signatura es similar a la de las funciones de las tareas simples, pudiendo tener los parámetros nominales siguientes:

- `params` (object). Los parámetros pasados a la prueba.
  Generalmente, se pasan cuando la prueba es iterativa para cada uno de los parámetros definidos por su método `forEach()`.
- `done` (function). La función que debe invocar la función de prueba para indicar que ha terminado, si es una prueba asíncrona.
- `log` (function). La función con la que escribir en la consola.

En el ejemplo anterior, puede observar la función `test()` para probar el constructor de una clase `Call`.

## Inicializadores

Un **inicializador** (*initializer*) es una tarea simple de prueba que tiene como objeto realizar algún tipo de preparación del entorno de prueba.
Se distingue tres tipos de inicializadores:

- Los **inicializadores de suite** (*suite initializers*), los cuales se ejecutan cuando se ejecuta una suite.
  Se ejecutan una única vez para todas las subsuites y pruebas de la suite.
  Son similares a los inicializadores `beforeAll()` de **Jasmine**.

- Los **inicializadores &ast;** (*&ast; initializers*), los cuales se ejecutan una vez cada vez que se ejecuta una prueba de la suite.
  Son similares a los inicializadores `beforeEach()` de **Jasmine** y **Mocha**.

- Los **inicializadores de prueba** (*test initializers*), los cuales son específicos de una determinada prueba y sólo se ejecutan cuando se ejecuta esa prueba.

Los inicializadores son ideales para realizar solicitudes de recursos o para abrir recursos como, por ejemplo, conexiones a bases de datos o a servidores. O bien, para preparar el entorno que requiere la prueba a ejecutar.

### Definición de inicializadores

En **Justo**, los inicializadores se crean con `init()`.
Por un lado, tenemos los inicializadores de suite que se definen con la función `init()` del paquete `justo`, dentro de su suite.
La sobrecarga a utilizar es la siguiente:

```
justo.init(fn)
```

Su función asociada es la que contiene el código que debe ejecutar el motor de tareas.
Puede ser síncrona o asíncrona, marcándose su tipo por la ausencia o presencia del parámetro `done`.

Por otro lado, tenemos los inicializadores &ast;, los cuales también se definen con la función `init()` de `justo`, pero usando la siguiente sobrecarga:

```
justo.init("*", fn)
```

Cuando el identificador de la tarea de inicialización es el asterisco (`*`), se está definiendo un inicializador &ast;.

Finalmente, tenemos los inicializadores de prueba.
En este caso, se utiliza el método `init()` de la prueba en cuestión:

```
test("...", function() { ... }).init(fn)
```

He aquí un ejemplo:

```
suite("id de la suite", function() {
  init(function() {
    //inicializador de suite
  });

  init("*", function() {
    //inicializador *
  });

  test("id de la prueba", function() {
    //operación de prueba
  }).init(function() {
    //inicializador de la prueba
  });
});
```

Una suite puede presentar tantos inicializadores como sea necesario.
Y las pruebas también.

Recordemos que los inicializadores son tareas y, por lo tanto, disponen del método `title()` con el que personalizar el título de cara al informe de resultados.
Es buena práctica asociar siempre un título a los inicializadores y finalizadores.
Ejemplo:

```
init("*", function() {
  hbs = new Handlebars();
}).title("Create Handlebars instance");
```

## Finalizadores

Un **finalizador** (*finalizer*) es la contraparte de los inicializadores y se ejecuta al final.
Y de manera similar se distingue entre finalizadores de suite, * o de prueba.

### Definición de finalizadores

Se definen igual que los inicializadores, pero mediante `fin()`, en vez de usar `init()`.

## Módulos de suite

Generalmente, las suites se escriben en módulos específicos.
Groso modo, una determinada suite (formada por subsuites, inicializadores, pruebas y finalizadores) se definirá en su propio módulo.
Por convenio y buenas prácticas, estos módulos se ubican en el directorio `test/unit`.

Estos módulos deben definir la suite madre, siendo ésta lo que deben exportar.
En **JavaScript**, tendremos algo como:

```
export default suite("...", function() {
  //...
});
```

En **Dogma**:

```
export suite("...", fn()
  #...
end)
```

Recuerde que el módulo debe exportar la definición de la suite.

Puede usar el comando `add suite` del generador `justo.generator.justo` para añadir módulos de suite al directorio `test/unit`:

```
$ justojs g justo add suite
```

## Macros diferidas de prueba

Por lo general, se suele definir una macro diferida para las pruebas en el catálogo integrado de nuestro proyecto.
He aquí un ejemplo:

```
catalog.macro("test", `./dist/${PKG}/test/unit`).title("Unit testing");
```

Recordemos que una macro diferida es aquella cuyas tareas no se determinan hasta su primera ejecución.
Todas ellas procederán de los archivos del directorio indicado.
En nuestro caso, del directorio de pruebas, donde cada archivo contiene un módulo que exporta una tarea de tipo suite.

## Filtro de ejecución

En muchas ocasiones, se necesita ejecutar sólo determinadas pruebas.
Para este fin, se dispone de las etiquetas.
Recordemos que las tareas disponen de la propiedad `tags`, la cual contiene sus etiquetas.
Mediante la opción `--tags` o `-t` del comando `justojs r`, podemos indicar las pruebas a ejecutar.
Restringiendo la ejecución a aquellas que tienen alguna de las etiquetas indicadas.

He aquí un ejemplo de prueba etiquetada:

```
test("my test", function() {
  //...
}).tags("only");
```

Y ahora un ejemplo de cómo indicar que se ejecuten las pruebas con la etiqueta `only`:

```
$ justo r test -t only
```

## Ejemplos

A continuación, puede consultar algunos proyectos de software que utilizan **Justo** como plataforma de automatización, incluyendo las pruebas:

- [`justo.assert`](https://bitbucket.org/justorocks/justo-assert)
- [`justo.spy`](https://bitbucket.org/justorocks/justo-spy)

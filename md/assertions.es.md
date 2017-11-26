# Biblioteca de aserciones de Justo

*Tiempo estimado de lectura: 10min*

Además de proporcionar un motor de tareas de pruebas, **Justo** también pone a nuestra disposición una biblioteca de aserciones de manera similar a como hacen **Should** o **Chai**.

## Qué son las aserciones

Una **aserción** (*assertion*) es una proposición que afirma o da por cierto algo.
Se utilizan principalmente para confirmar, tras la ejecución de una o más acciones u operaciones, valores de variables o campos.
La idea que se esconde tras las aserciones es validar que el software responde como se espera ante las acciones u operaciones.

A continuación, se muestra un ejemplo:

```
test("spy.obj(obj, mem)", function() {
  var o = spy.obj({f1: "v1", f2: "v2"}, "@f1");
  var s = spy(o);

  assert(o).isMap();
  assert(s).isMap();
  assert(s.fields).eq(["f1"]);
  assert(s.methods).eq([]);
  assert(s.calls).has("@f1");
});
```

## justo.assert

**Justo/JS** proporciona esta biblioteca de aserciones mediante el paquete `justo.assert`, descargable mediante `npm`.
Esta biblioteca proporciona soporte para tres tipos de objetos:

- Valores de cualquier tipo como, por ejemplo, número, texto, objetos, etc.
- Directorios.
- Archivos.

Se importa como sigue:

```
const assert = require("justo.assert");
```

## Aserciones de valor

En primer lugar, tenemos las aserciones de valor, las cuales permiten validar, entre otras cosas, que un valor es de un determinado tipo, es igual a otro, etc.

Para utilizar este tipo de aserción, hay que utilizar la función `assert()`, el objeto devuelto por el módulo `justo.assert`.
Esta función lo que hace es envolver el valor a comprobar y proporcionar un objeto con el cual realizar las comprobaciones.
He aquí un ejemplo ilustrativo que muestra cómo comprobar si un valor es de tipo objeto:

```
assert(o).isObject();
```

### Métodos de aserciones de valor

A los métodos proporcionados por el envoltorio, se les conoce formalmente como **métodos de aserción** (*assertion methods*).
Cuando la comprobación que realizan no se cumple, propagarán un error.
En otro caso, no lo harán.
Lo ideal es que no propaguen el error, pues indica que todo ha ido como se esperaba.

Estos métodos, a su vez, devuelven el envoltorio para poder, así, encadenar varias comprobaciones.
Ejemplo:

```
assert(o).isObject().has({
  x: 123,
  y: 321
});
```

El ejemplo anterior muestra cómo comprobar si el valor de la variable `o` es de tipo objeto y, además, tiene su propiedad `x` a `123` e `y` a `321`.

### Aserciones de tipo

Para afirmar cuál debe ser el tipo de un valor, se puede utilizar los métodos de aserción siguientes:

```
//string
assert(val).isText()
assert(val).isNotText()

assert(val).isString()
assert(val).isNotString()

//boolean
assert(val).isBool()
assert(val).isNotBool()

//number
assert(val).isNum()
assert(val).isNotNum()

//null
assert(val).isNil()
assert(val).isNotNil()

assert(val).isNull()
assert(val).isNotNull()

//object
assert(val).isMap()
assert(val).isNotMap()

assert(val).isObject()
assert(val).isNotObject()

//array
assert(val).isList()
assert(val).isNotList()

assert(val).isArray()
assert(val).isNotArray()

//function
assert(val).isFn()
assert(val).isNotFn()

//callable object
assert(val).isCallable()
assert(val).isNotCallable()
```

Si lo que deseamos es comprobar si un objeto es instancia de una determinada clase, podemos usar:

```
assert(val).isInstanceOf(className)
assert(val).isInstanceOf(class)

assert(val).isNotInstanceOf(className)
assert(val).isNotInstanceOf(class)
```

### Aserciones de comparación

Para asegurar que un valor es igual, menor o mayor que otro, disponemos de los siguiente métodos de aserción:

```
assert(val1).eq(val2)   //val1 == val2
assert(val1).ne(val2)   //val1 != val2
assert(val1).lt(val2)   //val1 < val2
assert(val1).le(val2)   //val1 <= val2
assert(val1).gt(val2)   //val1 > val2
assert(val1).ge(val2)   //val1 >= val2
```

Para comprobar si un valor es un determinado objeto concreto, tenemos:

```
assert(val1).sameAs(val2)     //val1 === val2
assert(val1).notSameAs(val2)  //val1 !== val2
```

Para rangos, se puede usar:

```
assert(val).between(start, end)
assert(val).notBetween(start, end)
```

### Aserciones por similitud

El método de aserción `similarTo()` se puede usar para garantizar que un valor de tipo lista o array es igual que otro, sin importar el orden de sus elementos:

```
assert(array1).similarTo(array2)
assert(array1).notSimilarTo(array2)
```

Ejemplos:

```
assert([1, 2, 3]).similarTo([2, 1, 3])  //ok
assert([1, 2, 3]).similarTo([3, 2, 1])  //ok
assert([1, 2, 3]).similarTo([1, 3, 2])  //ok
assert([1, 2, 3]).similarTo([1, 2, 4])  //un error será propagado
```

### Aserciones de inclusión

Para garantizar que un valor se encuentra dentro de una lista o un carácter o subtexto en un determinado texto, tenemos:

```
assert(val).includes(item)
assert(val).notIncludes(item)
assert(val).doesNotInclude(item)
```

### Aserciones de miembros

Para asegurar que un determinado objeto tiene determinados miembros, podemos usar:

```
assert(val).has(field)
assert(val).has(fields)
assert(val).notHas(field)
assert(val).notHas(fields)
assert(val).doesNotHave(field)
assert(val).doesNotHave(fields)
```

Se puede indicar el nombre de los miembros a consultar, garantizando así que existen, o bien un objeto con los miembros y su valores:

```
assert({one: 1, two: 2, three: 3}).has("one")
assert({one: 1, two: 2, three: 3}).has(["one", "two"])
assert({one: 1, two: 2, three: 3}).has({
  one: 1,
  two: 2
})
```

### Aserciones de tamaño o longitud

Cuando necesitamos confirmar el tamaño de una lista o de una cadena, podemos utilizar:

```
assert(val).len(len)
assert(val).notLen(len)
```

Para comprobar la longitud cero, podemos usar `len(0)` o bien `isEmpty()`:

```
assert(val).isEmpty()
assert(val).isNotEmpty()
```

### Aserciones de patrón

Para comprobar si un valor cumple un determinado patrón, podemos usar el método de aserción `like()`:

```
assert(val).like(pattern)
assert(val).notLike(pattern)
```

### Aserciones de propagación de error

Se puede comprobar si una determinada invocación de función propaga un error o no lo hace, mediante los siguientes métodos:

```
assert(fn).raises()
assert(fn).raises(err)
assert(fn).notRaises()
assert(fn).notRaises(err)
assert(fn).doesNotRaise()
assert(fn).doesNotRaise(err)
```

El argumento pasado a la función `assert()` debe ser la función que debe invocar el método de aserción.

He aquí un ejemplo ilustrativo:

```
assert(function() {
  //code
}).raises()

assert(function() {
  //code
}).raises("My custom error.")
```

## Aserciones de archivo

En muchas ocasiones, necesitamos comprobar el contenido o la existencia de un determinado archivo.
Para este fin, se puede utilizar la función `assert.file()`:

```
assert.file(...path)
```

Ejemplos:

```
assert.file("/my/file.txt").exists();
assert.file("/my", "file.txt").doesNotExist();
```

### Aserciones de existencia

Para garantizar que un archivo existe:

```
assert.file(...path).exists()
assert.file(...path).notExists()
assert.file(...path).doesNotExist()
```

### Aserciones de archivo vacío

Para afirmar que un archivo debe estar vacío:

```
assert.file(...path).isEmpty()
assert.file(...path).isNotEmpty()
```

### Aserciones de contenido

Para comprobar que un archivo contiene un determinado contenido:

```
assert.file(...path).includes(txt)
assert.file(...path).notIncludes(txt)
assert.file(...path).doesNotInclude(txt)
```

Los métodos anteriores comprueban que el texto existe.
Si necesitamos comprobar que el contenido de un archivo es uno dado:

```
assert.file(...path).eq(txt)
assert.file(...path).ne(txt)
```

Y si lo que deseamos es garantizar que dos archivos contienen lo mismo:

```
assert.file(...path1).sameAs(path2)
assert.file(...path1).notSameAs(path2)
```

## Aserciones de directorio

Para trabajar con directorios, usar `assert.dir()`:

```
assert.dir(...path)
```

Ejemplo:

```
assert.dir("/my/dir").exists()
assert.dir("/my", "dir").doesNotExist()
```

### Aserciones de existencia

Para garantizar que un directorio existe:

```
assert.dir(...path).exists()
assert.dir(...path).notExists()
assert.dir(...path).doesNotExist()
```

### Aserciones de entrada

Para comprobar si un directorio contiene una determinada entrada hija (sea archivo o directorio):

```
assert.dir(...path).has(name)
assert.dir(...path).notHas(name)
assert.dir(...path).doesNotHave(name)
```

## Ejemplos de uso

A continuación, se muestra varios proyectos de ejemplo, donde se muestra la biblioteca `justo.assert` en acción:

- [justo.dummy](https://bitbucket.org/justorocks/justo-dummy).
- [justo.spy](https://bitbucket.org/justorocks/justo-spy).

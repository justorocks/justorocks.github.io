# Soporte SSH integrado en Justo

*Tiempo estimado de lectura: 10min*

**Justo** soporta de manera nativa **SSH**, lo que permite ejecutar comandos en máquinas remotas de manera muy sencilla.
Su soporte es muy similar al que hacen otros motores de automatización IT como, por ejemplo, **Ansible** o **Salt**.

## Inventario de targets

Un **target** es una máquina a la que se tiene acceso mediante **SSH**.
Por su parte, el **inventario de targets** (*target inventory*) o simplemente **inventario** (*inventory*) es el catálogo de máquinas o *hosts* accesibles por **Justo** mediante **SSH**.
Debe ser accesible en forma de archivo o recurso web.

### Archivo de inventario

El **archivo de inventario** (*inventory file*) es una archivo en formato **JSON** que contiene el catálogo de *targets* o *hosts*.
Por convenio y buenas prácticas, se recomienda utilizar el archivo `Justo.tgt.json`, el cual debe encontrarse de manera predeterminada en el directorio actual.
Aunque podemos usar otro, indicado mediante la opción `--targets` o `-g`, lo que permite utilizar distintos archivos de inventario según las necesidades de cada momento.

Comencemos con un ejemplo ilustrativo:

```
{
  "localhost22": {
    "host": "127.0.0.1",
    "port": 22222,
    "user": {
      "name": "justo",
      "password": "test123"
    },
    "tags": ["sys", "alpine"],
    "os": {
      "id": "alpine",
      "version": "3.6"
    }
  },

  "localhost23": {
    "host": "127.0.0.1",
    "port": 22223,
    "user": {
      "name": "justo",
      "password": "test123"
    },
    "tags": ["sys", "ubuntu"],
    "os": {
      "id": "ubuntu",
      "version": "16.04"
    }
  }
}
```

Cada *target* se representa mediante un objeto, el cual tiene un nombre conocido formalmente como **alias** con el que se puede hacer referencia a él explícitamente.
En el ejemplo anterior, los alias son `localhost22` y `localhost23`.
Por otra parte, para cada *target*, necesitamos saber:

- `host` (string), el nombre del *host* o la dirección de IP con la que acceder a él.
  No tiene por qué coincidir con su alias.
- `port` (number), el puerto en el que está escuchando el servidor **SSH**.
- `user` (object), información del usuario con el que conectar al *target*: `name` (string), nombre del usuario; `password` (string), contraseña.
- `tags` (string[]), una lista de etiquetas asociadas al *target*.
- `os` (object), información del sistema operativo: `id` (string), identificador del sistema operativo como, por ejemplo, `alpine` o `ubuntu`; `version` (string), versión del sistema operativo.
  Para rellenar esta información, se recomienda utilizar la información disponible en `/etc/os-release` de los *targets*.
  En estos momentos, muchos *plugins* oficiales reconocen los sistemas operativos **Alpine** y **Ubuntu**; lo que les permite adaptar sus comandos al *target* en cuestión.

A continuación, se muestra un ejemplo mediante el cual se solicita que se ejecute la tarea `install-lua` contra los *targets* que tienen el *tag* `sys`:

```
$ justojs r install-lua,target=tag:sys --targets ./Justo.tgt.json --ssh

install-lua
-----------
SSH: install-lua
  @127.0.0.1: Install Lua 5.3
    @127.0.0.1:22222(alpine): Update local index OK (730 ms)
    @127.0.0.1:22222(alpine): Check if lua5.3 installed OK (78 ms)
    @127.0.0.1:22222(alpine): Check if lua5.3 available OK (168 ms)
    @127.0.0.1:22222(alpine): Install lua5.3 OK (691 ms)
  @127.0.0.1: Install Lua 5.3
    @127.0.0.1:22223(ubuntu): Update local index OK (4085 ms)
    @127.0.0.1:22223(ubuntu): Check if lua5.3 installed OK (1324 ms)
    @127.0.0.1:22223(ubuntu): Check if lua5.3 available OK (1344 ms)
    @127.0.0.1:22223(ubuntu): Install lua5.3 OK (2240 ms)

 OK       8
 FAILED   0
 TOTAL    8

 10676 ms

$
```

Cuando se ejecuta una tarea simple contra un *target*, su título es precedido automáticamente de `@host:port(os.id):`.

#### Generación de archivo de inventario

Para generar un archivo de inventario vacío, se puede utilizar el generador `justo.generator.inventory`:

```
$ justojs g inventory
```

#### Generación de target

Para generar un *target*, puede utilizar el comando `tgt` del generador `justo.generator.inventory`:

```
$ justojs g inventory tgt
? Target alias alpinessh
? Target host localhost
? Target SSH port 22222
? User name for connecting justo
? User password for connecting test123
? Target OS id (ID from /etc/os-release) alpine
? Target OS version (VERSION_ID from /etc/os-release) 3.6

Generation
  Generate from template OK (17 ms)

 OK       1
 FAILED   0
 TOTAL    1

 22 ms

"alpinessh": {
  "host": "localhost",
  "port": 22222,
  "user": {
    "name": "justo",
    "password": "test123"
  },
  "tags": [],
  "os": {
    "id": "alpine",
    "version": "3.6"
  }
}

$
```

El generador solicita la información del *target* y muestra por consola su objeto JSON correspondiente.
En algunos sistemas, además, copia el objeto en el portapapeles para facilitar así su pega en un archivo de inventario.

### Archivo de inventario web

El archivo de inventario a utilizar puede ser local o bien accesible mediante **HTTP** o **HTTPS**.
En este último caso, se indicará su dirección mediante la opción `--targets` o `-g`, precedida del protocolo.
Ejemplo:

```
$ justojs r install-lua,target=tag:sys --targets http://localhost:8080/Justo.tgt.json --ssh
```

La generación del recurso web es independiente de **Justo**.
Esto permite que cada organización genere dinámicamente los *targets* a utilizar.
Eso sí, debe devolver un archivo de inventario válido.

## Uso del motor SSH

Para hacer uso del motor de tareas compatible con **SSH**, hay que indicar la opción `--ssh`, tal como se ha mostrado en los ejemplos anteriores.

## Tareas compatibles con SSH

Una **tarea compatible con SSH** (*SSH-compatible task*) o simplemente **tarea SSH** (*SSH task*) es aquella cuya función de tarea se ha escrito usando la especificación **SSH** de **Justo**.
Sólo las tareas simples y los flujos de trabajo son compatibles con **SSH**.

### Definición de tareas simples SSH

Para definir una tarea simple compatible con **SSH**, basta con definir el parámetro `ssh` en la función de tarea.
Este parámetro recibe la función que debe utilizar para pasar los comandos a ejecutar en el *target* actual:

```
ssh(cmd) : any
```

A continuación, se muestra unos ejemplos ilustrativos:

```
apk = {
  update = justo.simple("update", function(ssh) {
    (ssh or child_process.execSync)("sudo apk update");
  }),

  add = justo.simple("add", function(params, ssh) {
    var cmd = "sudo apk add";
    if (params.update) cmd += " --update";
    if (params.allowUntrusted) cmd += " --allowUntrusted";
    cmd += " " + (typeof(params.pkg) == "string" ? [params.pkg] : params.pkg).join(" ");
    (ssh or child_process.execSync)(cmd);
  })
};
```

El motor de **SSH** inyecta el campo `target` en el parámetro `params`, proporcionando así información del *target* a la tarea.
Esto le permite adaptarse al tipo de *target* en cuestión.
Así, por ejemplo, el *plugin* `justo.plugin.pkg` usa el comando `apt` cuando el *target* es **Ubuntu** y `apk` cuando es **Alpine**.

En el ejemplo anterior, se define una tarea simple que tiene la capacidad de ejecutar el comando `sudo apk update`, tanto local como remotamente.
Cuando se ejecuta mediante el motor de tareas **SSH**, el parámetro `ssh` recibe la función con la que ejecutar el comando `sudo apk update` en el *host* remoto.
En otro caso, `ssh` será `null` y, por lo tanto, la tarea se ejecutará localmente mediante la función `child_process.execSync()`.
De esta manera, es muy fácil definir tareas para su ejecución local o remota.

#### Declaración de los targets

Una tarea simple se puede ejecutar contra cero, uno o más *targets* del inventario.
Para este fin, se ha reservado el campo `target` de los parámetros de invocación.
Veamos primero unos ejemplos:

```
apk.update({target: "tag:sys"});
apk.add({target: "*", pkg: "lua5.3"});
```

En el primer ejemplo, estamos solicitando que se ejecute la tarea en los *targets* del inventario que tienen la etiqueta `sys`.
Recuerde que el parámetro `target` está reservado para su uso por parte de **Justo** y no debe utilizarse para otra cosa.
Utilice el parámetro como se ha indicado.

Los *targets* se pueden indicar como sigue:

- `target: "*"`, representa todos los *targets* del inventario en uso.
- `target: "alias"`, representa el *target* con el alias indicado.
- `target: "tag:etiqueta"`, representa los *targets* con la etiqueta indicada.

#### Funcionamiento

Cada vez que el motor de tareas **SSH** se encuentra con una tarea simple, comprueba si uno de los campos de `params` es `target`.
Si así es, entonces consulta el inventario y extrae los *targets* que cumplen lo especificado.
Y a continuación, para cada *target*, ejecuta la función de tarea, pasando la función `ssh()` que debe utilizar la tarea para indicarle el comando a ejecutar bajo **SSH**.

### Definición de flujos de trabajo SSH

A diferencia de una tarea simple, un flujo de trabajo se define como compatible con **SSH** mediante el parámetro `target`.
Recordemos que las tareas simples lo hacen mediante el parámetro `ssh` de la función de tarea.

¿No sé puede ejecutar comandos **SSH** en un flujo de trabajo? Sí, pero a través de tareas simples.

Veamos un ejemplo ilustrativo que muestra cómo instalar **Lua** en un *target* concreto:

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

Observe dos cosas. Por un lado, en los flujos de trabajo, el parámetro `target` **no** es un campo de `params`.
Es independiente.
En segundo lugar, cada vez que el flujo de trabajo desea ejecutar un comando mediante SSH en el *target*, debe pasarle a la tarea simple, como campo del `params`, el *target* del flujo de trabajo.
Por favor, vuelva a echar un vistazo al ejemplo anterior.

## Configuración SSH de los targets

Cada vez que una tarea simple necesite ejecutar un comando con `sudo` bajo **SSH**, no olvide añadirlo al comando pasado a la función `ssh()`.
Por su parte, el usuario que se usará para acceder al *target*, mediante **SSH**, debe tener concedido el privilegio de ejecución del comando `sudo` y del comando a ejecutar con `sudo`.

Por convenio y buenas prácticas, se recomienda:

- Crear la cuenta de usuario `justo`, en los *targets*, aunque puede utilizar cualquier otra.
  Es tan sólo un convenio.
- Crear el archivo `/etc/sudoers.d/justo`, en los *targets*, con los permisos para que la cuenta usada por **Justo** pueda usar `sudo`.
  La siguiente línea, en este archivo, permite a la cuenta `justo` ejecutar con `sudo` cualquier comando: `justo ALL=(ALL) NOPASSWD:ALL`.
  Es importante configurar que `sudo` no solicite la contraseña para confirmar la ejecución; esto se consigue con `NOPASSWD:ALL`.

## Ejemplos de plugins SSH

Para desarrollar *plugins* SSH, no hay nada como ver otros ya desarrollados.
Tras leer la documentación sobre *plugins*, puede consultar:

- [justo.plugin.pkg](https://bitbucket.org/justorocks/justo-plugin-pkg). Proporciona tareas para trabajar con administradores de paquetes de sistema operativo.
  En el momento de escribir estas líneas, el *plugin*, con la misma API, es capaz de trabajar con **APT** (si **Ubuntu**) y **APK** (si **Alpine**).

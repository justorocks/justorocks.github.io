# Instalación de Justo

*Tiempo estimado de lectura: 5min*

**Justo/JS** está desarrollado en **Dogma**, [dogmalang.com](http://dogmalang.com), y se ha compilado a **JavaScript** para **Node.js**.
Requiriendo **Node.js 8** o superior y **NPM**.

Una vez instaladas las dependencias de **Justo/JS**, lo siguiente es conocer los paquetes NPM básicos:

- `justo`, contiene el *kernel* de **Justo/JS** y se recomienda instalarlo localmente para cada proyecto.
  Debe añadirse al archivo `package.json`, ya sea como dependencia de producción, campo `dependencies`, o como dependencia de desarrollo, `devDependencies`.
- `justojs`, contiene la interfaz de línea de comandos de **Justo/JS**, recomendándose una instalación global.

Además, se recomienda instalar los generadores básicos de **Justo/JS**.

Una buena instalación inicial puede consistir en ejecutar:

```
$ npm install -g justojs
$ npm install -g justo.generator.justo justo.generator.catalog justo.generator.workflow justo.generator.generator justo.generator.plugin justo.generator.inventory
```

Una vez instalado, es buena práctica comprobar que se tiene acceso al comando `justojs`, mostrando su ayuda:

```
$ justojs help
```

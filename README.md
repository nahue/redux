# [Redux](http://rackt.github.io/redux)

Redux es un contenedor predecible del state de aplicaciones Javascript.  

Ayuda a escribir aplicaciones que se comportan de manera consistente, que se ejecutan en diferentes entornos (cliente, servidor, y nativo), y son faciles de testear. Ademas de eso, provee una gran experiencia al desarrollador, como [edicion de codigo en tiempo real combinada con un debugger que permite viajar en el tiempo](https://github.com/gaearon/redux-devtools).

Usted puede usar Redux en conjunto con [React](https://facebook.github.io/react/), o con cualquier otra libreria que maneje vistas.
Es muy pequeña (2kB) y no tiene dependencias.

[![build status](https://img.shields.io/travis/rackt/redux/master.svg?style=flat-square)](https://travis-ci.org/rackt/redux)
[![npm version](https://img.shields.io/npm/v/redux.svg?style=flat-square)](https://www.npmjs.com/package/redux)
[![npm downloads](https://img.shields.io/npm/dm/redux.svg?style=flat-square)](https://www.npmjs.com/package/redux)
[![redux channel on discord](https://img.shields.io/badge/discord-%23redux%20%40%20reactiflux-61dafb.svg?style=flat-square)](https://discord.gg/0ZcbPKXt5bZ6au5t)
[![#rackt on freenode](https://img.shields.io/badge/irc-%23rackt%20%40%20freenode-61DAFB.svg?style=flat-square)](https://webchat.freenode.net/)


### Testimonios

>[“Love what you’re doing with Redux”](https://twitter.com/jingc/status/616608251463909376)  
>Jing Chen, creator of Flux

>[“I asked for comments on Redux in FB's internal JS discussion group, and it was universally praised. Really awesome work.”](https://twitter.com/fisherwebdev/status/616286955693682688)  
>Bill Fisher, author of Flux documentation

>[“It's cool that you are inventing a better Flux by not doing Flux at all.”](https://twitter.com/andrestaltz/status/616271392930201604)  
>André Staltz, creator of Cycle

### Experiencia de Desarrollo

Escribi Redux mientras trabajaba en una charla de React Europe llamada [“Hot Reloading with Time Travel”](https://www.youtube.com/watch?v=xsSnOQynTHs). Mi meta fue crear una libraria de manejo de state con una API minimalista pero con un comportamiento totalmente predecible, de tal manera que sea posible implementar logging, hot reloading, time travel, apps universales, grabar y re-reproducir, sin ningun tipo de compromiso de parte del desarrollador.

### Influencias

Redux evoluciona ideas de [Flux](https://facebook.github.io/flux), pero evita sus complejidades tomando conceptos claves de [Elm](https://github.com/evancz/elm-architecture-tutorial/).  
Los haya usado o no, solo tomara unos minuto para empezar con Redux.

### Instalacion

Para instalar la version estable:

```
npm install --save redux
```

Lo mas probable es que tambien necesite [los conectores a React](http://github.com/gaearon/react-redux) y [las herramientas para desarrolladores](http://github.com/gaearon/redux-devtools).

```
npm install --save react-redux
npm install --save-dev redux-devtools
```

Esto asume que esta usando el administrador de paquetes [npm](http://npmjs.com/) con un compilador de modulos como [Webpack](http://webpack.github.io) o [Browserify](http://browserify.org/) para consumir [modulos CommonJS](http://webpack.github.io/docs/commonjs.html).

Si usted todavia no utilizar [npm](http://npmjs.com/) o un compilador de modulos moderno, y prefiere utilizar un build de un unico archivo [UMD](https://github.com/umdjs/umd) que provee `Redux` como un objeto global, puede descargar una version pre-compilada de [cdnjs](https://cdnjs.com/libraries/redux). Nosotros *no* recomdamos este procedimiento para una aplicacion seria, ya que la mayoria de las librerias complementarias a Redux solo estan disponibles en [npm](http://npmjs.com/).

### El Gist

Todo el state de su aplicacion esta guardado en un objeto dentro de un unico *store*.  
La unica manera de cambiar el state es emitiendo una *accion*, un objeto describiendo que acaba de suceder.  
Para especificar como las acciones transforman el state, usted escribe *reducers* puros.

Eso es todo!

```js
import { createStore } from 'redux'

/**
 * Este es un reducer, una funcion pura con la firma (state, action) => state.
 * Describe como la accion transforma el state en el proximo state.
 *
 * La forma del state depende de usted: puede ser un primitivo, un array, un objeto,
 * o hasta una estructura de datos Immutable.js. La unica parte importante es que usted
 * no debe mutar el objeto del state, sino devolver un nuevo objeto con los cambios en
 * el state.
 *
 * En este ejemplo utilizamos una declaracion `switch` y strings, pero tambien puede
 * usar un helper que siga una convencion distinta (como funciones que usen maps)
 * si es que tiene sentido para su proyecto.
 */
function counter(state = 0, action) {
  switch (action.type) {
  case 'INCREMENT':
    return state + 1
  case 'DECREMENT':
    return state - 1
  default:
    return state
  }
}

// Crear un store Redux que contiene el state de su app.
// Su API es { subscribe, dispatch, getState }.
let store = createStore(counter)

// Usted puede suscribir a las actualizaciones manualmente, o usar bindings desde las vistas.
store.subscribe(() =>
  console.log(store.getState())
)

// La unica manera de mutar el state interno es disparando una accion.
// Las acciones pueden ser serializadas, logueadas, o guardadas y ser reproducidas mas tarde.
store.dispatch({ type: 'INCREMENT' })
// 1
store.dispatch({ type: 'INCREMENT' })
// 2
store.dispatch({ type: 'DECREMENT' })
// 1
```

En vez de mutar el state directamente, usted especifica las mutaciones que quiere que sucedan con objetos planos llamados *acciones*. Entonces usted escriba una funcion especial llamada *reducer* para decidir como la accion transforma el state de toda la aplicacion.

Si usted viene de Flux, hay una importante diferencia para comprender. Redux no tiene un Dispatcher o soporta muchos stores. En cambio, hay solo un store con solo una funcion reductora. Mientras su aplicacion crece, en vez de agregar stores, usted divide el reductor raiz en reductores (reducers) mas pequeños que operan independientemente en las diferentes partes del state. Esto es exactamente igual a que si hubiera un componente principal en una aplicacion React, pero que esta compuesto por muchos componentes pequeños.

Esta arquitectura puede parecer demasiado grande para una aplicacion *contador*, pero la belleza de este patron es lo bien que escala hacia aplicaciones grandes y complejas. Tambien permite herramientas de desarrollador muy potentes, porque es posible rastrear y enlazar cada mutacion y la accion que la causo. Puede *grabar* sesiones de usuario y reproducirlas solo ejecutando cada accion de la sesion.

### Documentacion

* [Introduccion](http://rackt.github.io/redux/docs/introduction/index.html)
* [Basica](http://rackt.github.io/redux/docs/basics/index.html)
* [Avanzada](http://rackt.github.io/redux/docs/advanced/index.html)
* [Recetas](http://rackt.github.io/redux/docs/recipes/index.html)
* [Resolucion de Problemas](http://rackt.github.io/redux/docs/Troubleshooting.html)
* [Glosario](http://rackt.github.io/redux/docs/Glossary.html)
* [Referencia API](http://rackt.github.io/redux/docs/api/index.html)

Para leer offline en formatos PDF, ePub, and MOBI, y las instrucciones de como crearlos, por favor vea: [paulkogel/redux-offline-docs](https://github.com/paulkogel/redux-offline-docs).

### Ejemplos

* [Contador](http://rackt.github.io/redux/docs/introduction/Examples.html#counter) ([source](https://github.com/rackt/redux/tree/master/examples/counter))
* [TodoMVC](http://rackt.github.io/redux/docs/introduction/Examples.html#todomvc) ([source](https://github.com/rackt/redux/tree/master/examples/todomvc))
* [Todos with Undo](http://rackt.github.io/redux/docs/introduction/Examples.html#todos-with-undo) ([source](https://github.com/rackt/redux/tree/master/examples/todos-with-undo))
* [Async](http://rackt.github.io/redux/docs/introduction/Examples.html#async) ([source](https://github.com/rackt/redux/tree/master/examples/async))
* [Universal](http://rackt.github.io/redux/docs/introduction/Examples.html#universal) ([source](https://github.com/rackt/redux/tree/master/examples/universal))
* [Real World](http://rackt.github.io/redux/docs/introduction/Examples.html#real-world) ([source](https://github.com/rackt/redux/tree/master/examples/real-world))
* [Shopping Cart](http://rackt.github.io/redux/docs/introduction/Examples.html#shopping-cart) ([source](https://github.com/rackt/redux/tree/master/examples/shopping-cart))

If you’re new to the NPM ecosystem and have troubles getting a project up and running, or aren’t sure where to paste the gist above, check out [simplest-redux-example](https://github.com/jackielii/simplest-redux-example) that uses Redux together with React and Browserify.

### Discusion

Join the [#redux](https://discord.gg/0ZcbPKXt5bZ6au5t) channel of the [Reactiflux](http://reactiflux.com) Discord community.

### Agradecimientos

* [The Elm Architecture](https://github.com/evancz/elm-architecture-tutorial) for a great intro to modeling state updates with reducers;
* [Turning the database inside-out](http://blog.confluent.io/2015/03/04/turning-the-database-inside-out-with-apache-samza/) for blowing my mind;
* [Developing ClojureScript with Figwheel](http://www.youtube.com/watch?v=j-kj2qwJa_E) for convincing me that re-evaluation should “just work”;
* [Webpack](https://github.com/webpack/docs/wiki/hot-module-replacement-with-webpack) for Hot Module Replacement;
* [Flummox](https://github.com/acdlite/flummox) for teaching me to approach Flux without boilerplate or singletons;
* [disto](https://github.com/threepointone/disto) for a proof of concept of hot reloadable Stores;
* [NuclearJS](https://github.com/optimizely/nuclear-js) for proving this architecture can be performant;
* [Om](https://github.com/omcljs/om) for popularizing the idea of a single state atom;
* [Cycle](https://github.com/staltz/cycle) for showing how often a function is the best tool;
* [React](https://github.com/facebook/react) for the pragmatic innovation.

Special thanks to [Jamie Paton](http://jdpaton.github.io) for handing over the `redux` NPM package name.

### Cambios

This project adheres to [Semantic Versioning](http://semver.org/).  
Every release, along with the migration instructions, is documented on the Github [Releases](https://github.com/rackt/redux/releases) page.

### Patrones

The work on Redux was [funded by the community](https://www.patreon.com/reactdx).  
Meet some of the outstanding companies that made it possible:

* [Webflow](http://webflow.com/)
* [Chess iX](http://www.chess-ix.com/)

[See the full list of Redux patrons.](PATRONS.md)

### Licencia

MIT

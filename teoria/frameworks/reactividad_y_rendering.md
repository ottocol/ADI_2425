<!-- .slide: class="titulo" -->

# Tema 5: *Frameworks* JS en el cliente
## Parte II: Reactividad y _rendering_

---

Recordemos que prácticamente todos los *frameworks JS* actuales tienen características similares:

- Basados en la idea de **componentes** *anidables*
- Automatizan el *rendering*  de HTML (gracias a la **reactividad**))
- Implementan *routing* (*)
- Ofrecen mecanismos de *gestión del estado* (los datos que no son propios de los componentes) (*)

(*) Algunos *frameworks* tienen esto integrado, en otros son *plugins*


---

<!-- .slide: class="titulo" -->

# Reactividad


---

Aunque **reactividad** es un término bastante amplio que tiene distintos significados en distintos contextos, en el contexto de los *frameworks* JS se suele entender como:

1. **cambiar automáticamente el valor de una variable** cuando esta depende de otra
2. **repintar la vista automáticamente cuando cambia el estado** del componente 

En general todos los *frameworks* JS son reactivos al menos en el sentido (2), en el (1) solo algunos como Vue o Svelte

---

En realidad la reactividad no es nada nuevo

![](imag_3/hoja_calculo.png) <!-- .element class="stretch" -->


---

## Reactividad en *rendering* (Vue)

Con el *options API*


```javascript
var app = Vue.createApp({
      data: function()  {
        return {
          nombre: "",
          puesto: ""
        }
      },
      methods: {
        generarPuesto: function() {
          this.puesto = faker.name.jobTitle()
        }
      },
      template: `
        Tu nombre: <input type="text" v-model="nombre"><br>
        <button v-on:click="generarPuesto">Generar puesto</button>
        <div class="back">
          {{nombre}} <br>
          {{puesto}}
        </div>  `
 })
 app.mount('#app') 
```

[https://jsbin.com/nahikon/edit?html,js,output](https://jsbin.com/nahikon/edit?html,js,output) <!-- .element: class="caption" -->


---

En *frameworks* JS se pueden distinguir dos tipos de reactividad:

- **Tipo *pull***: dispararla requiere la "colaboración" del desarrollador. Típicamente hay que llamar a un método para actualizar el estado, que a su vez "repinta" la vista. Por ejemplo: React, Svelte 2, Angular (aunque este último hace el *pull* automáticamente, por ejemplo en manejadores de evento)
- **Tipo *push***: al cambiar los datos se recalculan las dependencias y/o repinta la vista automáticamente. Ejemplos: Vue, Svelte 3, SolidJS

---


## Ejemplos de reactividad tipo "pull"

- [Ejemplo de código "de juguete"](https://jsbin.com/cozigix/2/edit?html,js,console,output) 
  - Para probarlo escribir en la consola `setState({contador:0}`. A partir de ahí se puede usar el botón (que llama a `setState`) 
- [Ejemplo con el framework React (bueno, preact)](https://preactjs.com/repl?code=aW1wb3J0IHugcmVuZGVyIH0gZnJvbSAncHJlYWN0JzsKaW1wb3J0IHsgdXNlU3RhdGUgfSBmcm9tICdwcmVhY3QvaG9va3MnOwoKZnVuY3Rpb24gQ291bnRlcigpIHsKCWNvbnN0IFt2YWxvciwgc2V0VmFsb3JdID0gdXNlU3RhdGUoMCk7CiAgCiAgZnVuY3Rpb24gaW5jcmVtZW50YXIoKSB7CgkJc2V0VmFsb3IodmFsb3IrMSkKCX0KCiAgY29uc29sZS5sb2coIlJlbmRlciBjb250YWRvciIpCglyZXR1cm4gKAoJCTw%2BCgkJCTxoMT57dmFsb3J9PC9oMT4KCQkJPGJ1dHRvbiBvbkNsaWNrPXtpbmNyZW1lbnRhcn0%2BSW5jcmVtZW50YXI8L2J1dHRvbj4KCQk8Lz4KCSk7Cn0KCnJlbmRlcig8Q291bnRlciAvPiwgZG9jdW1lbnQuZ2V0RWxlbWVudEJ5SWQoJ2FwcCcpKTsK) (podéis ver que la idea es muy similar, aunque la función que hace *pull* es `useState`)


---

## Reactividad push

Como ya sabíais, Javascript no es reactivo:

```javascript
var a1 = 2
var a2 = 3
var total = a1 + a2
console.log(total)  //5
a1 = 10
console.log(total)  //sigue siendo 5 😕
```

---
## Reactividad push en Vue

Vue incorpora funcionalidades que hacen reactivo el código (con cierta colaboración del desarrollador): 

```javascript
import { reactive, watchEffect } from 'vue';

let estado = reactive({valor:1})

watchEffect(function(){
  console.log("valor", estado.valor)
})

//ejecutará otra vez el console.log
estado.valor = 2
```

[El ejemplo online](https://play.vuejs.org/#eNp9kUFPwzAMhf9KlMs6aSpCcBoDCdAOcAAEHHOpMrfrSJ0ocbpJVf87bspGkdBuid/nl2enk/fO5W0EuZSroH3tSASg6O4U1o2znkQn9gXp7bosQZPoReltI2bcMruZMB4KTXULfwCFBkhAoGJjxe2Jybq2MNYvL/u5QoUT+6yMyIjFbN4pFEJbDNZAbmyVKZm6lFz8OObpzhajzepizM/JFcqFpMDdZV3lu2CRx0uGSmrbuNqAf3XDO0HJpUjKoBXG2P1zqpGPsDjW9Rb01z/1XTgMNSXfPATwLXC4o0aFr4BGef3xAgc+n8TGbqJh+oz4Djx5HDKO2EPEDceecCntU/qAGqvPsD4QYDgONQQdyD7xvLwIj2dG/417lV+nPt6r7L8BdpG3xA==)

Notas: 
Para comprobar la reactividad más claramante, añadir una línea
`setInterval(()=>estado.valor++, 1000)`

---

## Cómo funciona la reactividad en Vue 3

Para cada propiedad de cada objeto reactivo guardamos la lista de los *effects* que usan su valor

![](imag_3/deps.png) <!-- .element class="stretch" -->

[Del video "Reactivity in Vue 3: How does it work?"](https://www.youtube.com/watch?v=NZfNS4sJ8CI) <!-- .element class="caption" -->


---

Cada vez que modificamos una propiedad de un objeto reactivo, debemos ser capaces de ejecutar automáticamente todos los *effects* asociados 

Vue usa internamente *proxies* ES6, una funcionalidad estándar de JS para "envolver" objetos interceptando las llamadas a sus métodos

```javascript
let estado = {valor:1}
//el handler del proxy, en el ejemplo interceptamos los setters
let handler = {
  set: function(obj, prop, val) {
    //en Vue estaría ejecutando los effects asociados a "prop"
    console.log('cambiando', prop, "a", val)
    //Aplico el cambio manualmente
    obj[prop] = val
  }
}
let estadoProxy = new Proxy(estado, handler)
estadoProxy.valor = 2
```
[El ejemplo online](https://playcode.io/2045583)

Como habréis adivinado, el método `reactive` en Vue devuelve un *proxy*

---

## Ejemplo en SolidJS

El *framework* SolidJS usa **signals** y **effects** para modelar la reactividad, son ideas similares a las de Vue

```javascript
import { createSignal, createEffect } from "solid-js";

const [valor, setValor] = createSignal(1);
createEffect(() => console.log("valor", valor()));
  
setValor(valor()+1)
```

[El ejemplo online](https://playground.solidjs.com/anonymous/fec3a75f-4fac-4ae8-8b80-849dafc7e94f)

Nótese que `valor()` es una función para poder interceptar la operación de "leer valor", SolidJS no usa *proxies* para esto (aunque sí para otras cosas).

Notas: 
Para comprobar la reactividad más claramante, añadir una línea
`setInterval(()=>setValor(valor()+1), 1000)`

---

Definir los *effects* manualmente es tedioso, los *frameworks* lo hacen automáticamente en ciertos casos, por ejemplo cuando mostramos una variable en un *template* de un componente

```javascript
mport { createSignal } from "solid-js";
import {render} from "solid-js/web"

function Contador() {
  // Creamos una signal para el contador
  const [count, setCount] = createSignal(1);

  return (
    <div>
      <h1>Contador reactivo con SolidJS</h1>
      <h1>Contador: {count()}</h1>
      <button onClick={() => setCount(count() + 1)}>Incrementar</button>
    </div>
  );
}

render(()=><Contador/>, document.getElementById("app"))
```

[El ejemplo online](https://playground.solidjs.com/anonymous/87241469-f8b9-4d2e-afa1-63643f46eefb)

---

## Otro ejemplo: Svelte 

- Internamente sigue un enfoque distinto a Vue, en lugar de hacer "la magia" en *runtime*, es un **compilador** que genera "código reactivo".  
+ Esto en teoría genera un código más *ligero* y eficiente.
- [Ejemplo online](https://svelte.dev/repl/https://play.vuejs.org/#eNp9kDFrwzAUhP+K0BIHjFvIZhJDWzy0Q1vajlqM/OwolSUhPTsG4/9eSSZuhpBN3H0n7t5En4zJha17ccc68af7d44948dee4b68256766dc?version=3.44.1)


---

Reactividad de "grano grueso" en React vs "de grano fino" en Vue

Ejemplo React:
 https://playcode.io/2046096

de momento no consigo demostrar la reactividad de grano fino de Vue


---

<!-- .slide: class="titulo" -->

# *Rendering*

---

Demo de que en React cuando se ejecuta un componente padre se ejecutan también los hijos


---

El *framework* debe permitirnos especificar cómo será la vista en función de los datos

```
vista = f(datos)
```

Aunque podríamos hacer muchas más diferencias hay *frameworks* que usan un lenguaje de **_templates_** (p.ej Angular, Svelte, Vue _en apariencia_) y otros usan directamente **Javascript** (p.ej React)

---

## Opción 1: lenguajes de templates

- Problema: nos vemos limitados a la expresividad del lenguaje
- Ventaja: Los *framework* suelen incluir un *compilador* de *templates* que puede optimizar la detección de cambios para el repintado

```html
<div id="content">
  <p class="text">Lorem impsum</p>
  <p class="text">Lorem impsum</p>
  <p class="text">{{mensaje}}</p>
  <p class="text">Lorem impsum</p>
  <p class="text">Lorem impsum</p>
</div>
```

En el ejemplo, solo puede cambiar el tercer `<p>`, así que **para repintar el componente *como mucho* solo hace falta repintar ese `<p>`**

[Ejemplo en Svelte](https://svelte.dev/repl/a17ccc68af7d44948dee4b68256766dc?version=3.44.1)


Notas:

- En Svelte la template es todo lo que está fuera de `<script></script>`
- Mirar la solapa "JS Output" para ver el resultado del compilador de Svelte
- La función `c()` es la que  crea el HTML de la template, `m()` la monta en el DOM
- `p(ctx,dirty)` actualiza solo las partes que pueden cambiar (simplificando, `ctx` son las variables y `dirty` las que han cambiado)


---

## Opción 2: JS para definir *templates*

- En React podemos generar una etiqueta con la función `React.createElement(<tag>, <atributos>, <hijos>)`
- Podemos generar HTML arbitrario combinando en JS varias llamadas a `createElement` 

```javascript
//esto es como la "f" de vista=f(datos) 
function HelloReact(props) {
  return React.createElement("div", null, 
            React.createElement("h1", null, props.saludo), 
            React.createElement("p", null, "Welcome to React"));
}
//maquinaria necesaria para pintar el componente React en la página
const element = <HelloReact saludo="Ehh!!"/>;
ReactDOM.render(
  element,
  //suponiendo que exista un tag con id="mountNode"
  document.getElementById('mountNode')
);
```
[Probar ejemplo](https://jscomplete.com/playground/s357745)

---

Usar JS para la función de *render* tiene la **ventaja** de que **podemos usar toda la expresividad de JS** (control de flujo, funciones de la librería estándar, propias,...)

```javascript
//Aclaración: nadie programa así en React, normalmente se usa un formato llamado JSX,
//que permite poner las etiquetas en el JS y es mucho más legible
function Ejemplo(props) {
    const c = React.createElement
    const children = []
    for(var i=0; i<5; i++) {
        children.push(c('p', {class:'text'},
                       i == 2 ? props.mensaje.toUpperCase() : 'Lorem ipsum'))
    }
    return c('div', {id:'content'}, children)
}

const element = <Ejemplo mensaje="Hola amigos" />;
ReactDOM.render(
  element,
  document.getElementById('mountNode')
);
```

[Ejemplo online](https://jscomplete.com/playground/s357763) | [Ejemplo con sintaxis JSX](https://jscomplete.com/playground/s768783)

---

**Problema**: el lenguaje es tan expresivo que el *framework* no puede analizar qué estamos haciendo y no puede optimizar el proceso de repintado.

Como hemos visto en los ejemplos, el desarrollador lo programa como si se repintara todo el árbol (*como los gráficos de un juego que se repintan enteros n veces por segundo*) ([ejemplo del reloj](https://codepen.io/ottocol/pen/QWWVWPa?editors=1010))

**¿Cómo reducir entonces el coste del repintado?**

---

## DOM virtual

- Idea introducida por React
- `React.createElement` no genera nodos del DOM real, sino en memoria (en un "árbol DOM virtual"), con un API más rápido
- En cada *render* se hace una especie de *diff* entre el DOM virtual actual y el anterior (["reconciliation"](https://reactjs.org/docs/reconciliation.html)). En React el coste es lineal con el número de nodos

<img src="imag_3/virtual_dom.png" class="stretch"/>

- **Solo se repintan en el DOM real los nodos que cambian**. 


---

[Ejemplo del reloj](https://codepen.io/ottocol/pen/QWWVWPa?editors=1010)

Para ver el efecto del DOM virtual:
1. Abrir la herramientas de desarrolladores del navegador, 
2. Ir al código fuente en "tiempo real" (pestaña "Elements" en Chrome, "Inspector" en Firefox) 
3. Buscar el div con id="root", es el componente *reloj*. 

Aunque en el código de la función de *render* se repinta el componente entero, en el navegador solo se está cambiando un nodo.

---

## Vue y el Virtual DOM

- Aunque Vue usa plantillas para describir el HTML de los componentes, estas internamente se comportan como funciones JS, de hecho podemos escribir la función `render()` si las plantillas "se nos quedan pequeñas"

```javascript
//Esta función crea nodos en el DOM virtual
import { h } from 'vue'

export default {
  data() {
    return {
      msg: 'hello'
    }
  },
  render() {
    return h('div', this.msg)
  }
}
```


- Por tanto, **Vue también usa un DOM virtual**


---


## Referencias

- 📺 [dotJS 2016 - Evan You - Reactivity in Frontend JavaScript Frameworks](https://www.youtube.com/watch?v=r4pNEdIt_l4)
- 📺 [Evan You on Vue.js: Seeking the Balance in Framework Design | JSConf.Asia 2019](https://www.youtube.com/watch?v=ANtSWq-zI0s)
- 📺 Reactivity in Vue 3: How does it work? [video1](https://www.youtube.com/watch?v=NZfNS4sJ8CI) [video2](https://www.vuemastery.com/courses/vue-3-reactivity/proxy-and-reflect/) (para ver el vídeo 2 tenéis que registraros en el sitio web del curso de Vue)
- [Cómo funciona el compilador de Svelte](https://lihautan.com/compile-svelte-in-your-head-part-1/)
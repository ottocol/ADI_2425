# Introducción a Javascript

> **IMPORTANTE:** en modo alguno debe verse esto como una introducción completa a Javascript. Simplemente se trata de los **conocimientos mínimos necesarios para poder empezar las prácticas de ADI**. Como tal, se pasan por alto muchos temas avanzados (¡y también algunos básicos!). Conforme avancemos en la asignatura se introducirán más conceptos de Javascript que por el momento no vamos a necesitar.

Javascript nació en el lado del cliente, dentro del navegador, pero en los últimos años se ha trasladado también al lado del servidor. Aquí vamos a ver el "núcleo básico" del lenguaje, la parte común e independiente de en qué lado estemos.

## Características básicas del lenguaje

- **Qué es (y no es) Javascript**
    + Es un lenguaje de propósito general
    + **No** es una versión reducida de Java, ni está directamente relacionado con este lenguaje. La semejanza en el nombre es [una cuestión de *marketing*](https://en.wikipedia.org/wiki/JavaScript#Beginnings_at_Netscape).
    + **No es un lenguaje trivial**, ni pensado para no programadores. Originalmente se usaba para pequeñas tareas que requerían pocas líneas de código y que eran fáciles de copiar/pegar, pero en la actualidad se usa para escribir aplicaciones de cientos de miles de líneas de código.

- **Es interpretado, no compilado**
    + Los navegadores incluyen un intérprete Javascript (aunque la mayoría de implementaciones usan [compilación Just In Time - JIT](http://creativejs.com/2013/06/the-race-for-speed-part-2-how-javascript-compilers-work/), por motivos de eficiencia).
    + Al ser interpretado, los fuentes son directamente accesibles, (aunque pueden ser poco legibles si están [*ofuscados* o *minificados*](http://librosweb.es/libro/ajax/capitulo_11/ofuscar_el_codigo_javascript.html).

<!--
    + Lo anterior puede cambiar si tiene éxito la idea de [WebAssembly](http://arstechnica.com/information-technology/2015/06/the-web-is-getting-its-bytecode-webassembly/): un código compilado y portable para una Máquina Virtual Javascript, al estilo del *bytecode* de Java.
-->

- **Hay que distinguir entre el "núcleo" del lenguaje y las librerías** 
    + Las del navegador nos permiten modificar dinámicamente el HTML/CSS, comunicarnos con el servidor, dibujar en pantalla en 2D/3D ... Por el momento aquí solo vamos a ver el núcleo del lenguaje.
    +  El núcleo del lenguaje está estandarizado en lo que se denomina ECMAScript. Se eligió un nombre "neutro", ya que Javascript es una marca comercial (ahora de Oracle - antes de Sun, y antes de Netscape). 
    + Desde 2015, cada año hay una nueva versión del estándar, que se precede de las letras ES (por ECMAScript). La versión anterior a ES2015 usaba una nomenclatura más tradicional, denominándose ES5. Por complicar un poco, en algunos sitios ES2015 aparece como ES6, usando la nomenclatura antigua. Las últimas versiones de la mayoría de navegadores implementan ES2015 [casi en su totalidad](https://kangax.github.io/compat-table/es6/).
    
- **Entornos de ejecución**: aunque JS nació en el navegador, desde hace unos años se usa también para escribir aplicaciones en el servidor (e incluso de escritorio). 
+ Cada navegador tiene su propio intérprete JS. Hace unos años había incompatibilidades importantes entre ellos, que se han ido eliminando conforme se estandarizaba el lenguaje y sus APIs asociados y se dejaba de lado la "guerra de los navegadores". Actualmente los problemas de portabilidad vienen  por el lado de si el navegador ya implementa o no determinada funcionalidad.
+ En el servidor, el entorno de ejecución más usado es Node.

> Javascript tiene fama de ser un lenguaje "rarito" y "especial". En apariencia es como un C "con tipos dinámicos" pero esconde muchas sutilezas por debajo de la superficie. Podéis ver algunas en tono humorístico  en la conocida charla titulada [WAT](https://www.destroyallsoftware.com/talks/wat). Os recomiendo verla solo "por las risas" (dura menos de 5 minutos). En esta charla no explica el por qué de los extraños resultados, si tenéis realmente curiosidad podéis verla por ejemplo en [este video](https://www.youtube.com/watch?v=oK2vXWfCnt4) o [en este artículo](https://medium.com/dailyjs/the-why-behind-the-wat-an-explanation-of-javascripts-weird-type-system-83b92879a8db).

## Sintaxis básica

- A grandes rasgos, la sintaxis básica es muy similar a la de C
- Los comentarios siguen el estilo C/C++ (`/* ... */`, `//`)
- El `;` al final de una sentencia es *opcional*, pero se suele recomendar su uso para evitar ambigüedades.
- Las cadenas se pueden delimitar por comillas dobles o simples (`"hola"`, `'hola'`). Esto será útil cuando lleguemos al navegador y mezclemos JS con HTML (en el que solo se pueden usar comillas dobles). 

### Variables y constantes

- No tienen tipo predefinido, o mejor dich{o, *el tipo puede cambiar dinámicamente*.
- No existen palabras clave en el lenguaje para definir tipos. Las variables se declaran simplemente con `var`

```javascript
var a;

a = 1;
a = "Hola"; //Cambiamos el tipo. Y JS sin rechistar
a = [1,2];  //Literal para definir un array. Sigue sin rechistar
b = "OK";   //Podemos usar variables no declaradas. Se declaran automáticamente
```

- Internamente se diferencia entre tipos *primitivos* (numérico, *booleano*, cadena) y objetos, que son referencias, como en Java (por ejemplo `Date`, `RegExp`, entre las "clases" predefinidas en JS, o los objetos que podemos definir nosotros)

```javascript
typeof 3          //"number"
typeof 3.5        //también "number"
typeof "hola"     //"string"
typeof new Date() //"object"
```

> Curiosamente, en Javascript tanto los enteros como los reales se representan en coma flotante (en C serían `double`), de ahí que 3 y 3.5 sean del mismo tipo.

- El valor de una variable declarada pero no inicializada es un valor especial llamado `undefined`    

```javascript
var no_definida;
console.log(no_definida)  //"undefined", variable declarada pero no inicializada
//Comprobar si una variable es undefined
//Luego veremos por qué se usa el operador `===` en lugar del típico `==`
if (no_definida===undefined)
    console.log("no_definida es undefined");
console.log(c)  //ERROR, intentamos leer una variable no declarada
```

- En Javascript existe también un valor vacío o `null` que es casi lo mismo que `undefined`, aunque [hay pequeñas diferencias](https://developer.mozilla.org/es/docs/Web/JavaScript/Reference/Global_Objects/null)

- **Modo estricto**: considera errores ciertos comportamientos "toleradas" en JS, por ejemplo asignar un valor a una variable no declarada

```javascript
//Activar modo estricto en todo el ámbito del script
'use strict'
b = 1 //¡Error!

function estricta(){
  // Activar modo estricto solo en una función
  'use strict';
  function anidada() { return "¡Y yo!"; }
  return "Soy una función en modo estricto  " + nested();
}
function noEstricta() { return "Yo no estoy en modo estricto"; }
```

- A diferencia de C y derivados, el ámbito de las variables definidas con `var` no es el bloque (`{...}`) en que se definen, sino la función entera, o el ámbito global si están fuera de una función (más adelante veremos las funciones)

```javascript
if (false) {
  var prueba = "hola"
}
else {
  //En esta rama del if también existe la variable prueba
  //aunque como no se le ha dado valor, será undefined  
  console.log(prueba)
}
```

Es como si el intérprete  de JS moviera las declaraciones de variables al principio del código (al estilo de lo que en la universidad os recomendábamos hacer en los tiempos de programación 1 :) ). Esto se conoce como *hoisting*.

- En ES2015 se introdujo `let` como alternativa a `var` para declarar variables. `let` define una variable con ámbito de bloque (`{...}`), al estilo C. Si en el ejemplo anterior cambiamos `var` por `let` obtendremos un error en tiempo de ejecución en el `console.log` al no estar definida la variable `prueba`.

- Para definir constantes se usa `const`

```javascript
const PI_AGAIN = 3.141592
```

### Operadores

- En su mayor parte son equivalentes a los de C. 
- No se pueden redefinir, al contrario que en C++, aunque algunos están sobrecargados por defecto, por ejemplo `+` también sirve para concatenar cadenas.
- **CUIDADO**: en Javascript el operador de comparación `==` intenta conversión de tipos. Si queremos comprobar la igualdad estricta, usaremos `===`

```javascript
1=="1"     //true!!, porque "1" se convierte a 1
false=="0" //true!!, porque "0" se convierte a 0, e igual que en C, 0 es false
1===true   //false!! son tipos distintos 
```

> Se recomienda usar siempre `===` para comprobar si un valor es `undefined` porque `null==undefined`, con lo que se confundirían ambos valores. Con `===` no hay ambigüedad ya que no es cierto que `null===undefined`

### Estructuras de control y manejo de errores

- Básicamente son las mismas que en C/Java: `for`, `while`, `do...while`,...
- Existe una variante del `for` que nos permite iterar por todas las propiedades de un objeto, la veremos cuando hablemos de objetos  
- Los errores se gestionan con *excepciones* al estilo Java, con `try...catch...finally`.

[DEMO en repl.it](https://repl.it/KxGO/3)
```javascript
try {
  var a = 4;
  //La siguiente línea daría error por variable no definida
  a = b + 2;
  //La siguiente línea en realidad no se ejecutará nunca
  console.log("Después del error");
} catch(err) {
  console.error("Error en try/catch " +  err.message);
} finally {
  console.log("Pase lo que pase, llegamos al finally");
}
```

- Podemos lanzar nuestras propias excepciones con `throw`. Podemos lanzar un objeto de cualquier clase o simplemente una expresión (numérica, de cadena, booleana,...). 
- En Javascript las excepciones lanzadas por el *runtime* son instancias de la ["clase" `Error`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Error)

```javascript
//Ejemplos de uso de throw
//Podemos lanzar valores cadena, numéricos,...
throw "La cosa está muy mal";
...
throw 33;
...
//Podemos lanzar una instancia de cualquier objeto
...
var miExcepcion = {codigo:1, mensaje:"¡Error!", tipo:"critico"};
throw miExcepcion;
...
//Podemos aprovechar el constructor estándar "Error"
throw new Error("La cosa está muy mal");
```

## Funciones

- Se definen con la palabra clave `function`. La mayor diferencia con lenguajes tipo C/Java es que los parámetros no tienen tipo declarado (como es lógico dado el tipado dinámico de JS) y tampoco se especifica el tipo de retorno.
- Para devolver un valor se usa `return` en el cuerpo de la función

```javascript
function saludo(nombre) {
   return 'Hola ' + nombre; 
}

console.log(saludo('Pepe'));  //Hola Pepe
console.log(saludo(42));      //Hola 42 (podemos pasar cualquier tipo)
```

> En el ejemplo anterior puede verse que cuando intentamos "sumar" una cadena y un valor numérico, Javascript convierte automáticamente el número a cadena y concatena ambas.

- Se recomienda **declarar las variables locales a las funciones** con `var` para evitar que por descuido estemos referenciando una global

```javascript
var mensaje = 'Soy tu padre';

function saludo(nombre) {
    mensaje = 'Cuidado' + nombre + ', la ira conduce al odio';
    return mensaje;
}

saludo('Pepe');
console.log(mensaje) //"Cuidado, Pepe, la ira conduce al odio"
```

- Las funciones son "[ciudadanos de primera clase](https://codepicnic.com/codebites/funciones-en-javascript-i-9a1158154dfa42caddbd0694a4e9bdc8)": pueden pasarse como parámetros, ser asignadas a variables y pueden ser devueltas por otra función.

```javascript
function suma (arg1, arg2)  {
    var res = arg1 + arg2;
    return res;
}

function operar(arg1,arg2,op) {
  return op(arg1,arg2)
}
console.log(operar(2,2,suma));  //4
```

- **Funciones anónimas**: no se les da un nombre, se definen para ser asignadas a una variable. 

```javascript
//Este código se supone ejecutándose en un navegador
//Asociamos una función a un evento sobre un botón
//El botón estaría definido en HTML:
//  <input type="button" value="Soy un botón" id="boton"/>
document.getElementById('boton').onclick = function() {console.log("Hola")};
```

> Uno de los usos más típicos de las funciones anónimas en JS es en la definición de *callbacks*. En el ejemplo anterior definimos un tipo especial de *callback*: un *manejador de evento*, que el navegador ejecutará cuando se produzca un determinado evento sobre un objeto.

- Funciones de "flecha" (*fat arrow*). Introducidas en ES2015, reciben este nombre por definirse con los caracteres `=>` en lugar de la palabra clave `function`. Esta forma de definir funciones tiene unas cuantas variantes:

En la sintaxis más habitual, primero se ponen los argumentos entre paréntesis y separados por comas, luego `=>` y finalmente una expresión que será el valor de retorno (no hace falta `return`)

```javascript
let suma = (val1,val2) => val1+val2
console.log(suma(2,3))  //5

//La función anterior es equivalente a
let suma = function(val1, val2) {
    return val1+val2
}
```

> Nótese que las funciones de flecha siempre son anónimas, por eso en el ejemplo hemos asignado la función a una variable

Si necesitamos definir algo más que una expresión, ponemos el cuerpo de la función entre llaves, tal y como lo definiríamos usando `function`

```javascript
//Esta sintaxis no es necesaria para este caso tan sencillo, es solo un ejemplo
let suma = (val1,val2) => {
  return val1+val2
}
```

La sintaxis de flecha puede verse como una forma de abreviar la definición de funciones, pero tiene otras utilidades, que veremos más adelante en la asignatura. La más importante es la [vinculación léxica del `this`](https://www.eventbrite.com/engineering/learning-es6-arrow-functions/#lexical-this) (que ya veremos en qué consiste).

### Paso por valor vs. por referencia

Como ocurre en Java, **los tipos primitivos se pasan por valor y los objetos por referencia**. Esto se aplica al paso de parámetros en funciones y a la asignación. Por ejemplo:

```javascript
var a,b;
a = [5];              // Inicializamos un array de una sola posición con un literal
a[0] = 1;
b = a;               // b referencia al array a, no es una copia 
a[0] = 100;
alert(b[0]);         // muestra el valor 100 
```

## Programación orientada a objetos

- Tanto los objectos como sus propiedades se pueden crear y modificar dinámicamente

```javascript
var persona;
persona = new Object();  //Objeto "vacío", sin propiedades. También valdría hacer persona = {}
persona.nombre = "Homer Simpson";
persona.edad = 34;
persona.casado = true; 
delete persona.edad      //A partir de ahora, persona.edad==undefined
//Las propiedades pueden ser funciones. Estaríamos definiendo un método.
persona.saludo = function() {console.log("Hola, soy " + this.nombre)};
persona.saludo();  //Llamamos al método
```

> Cuando se elimina una propiedad de un objeto con `delete`, esta pasa a ser `undefined`. Esto ocurre en realidad con cualquier propiedad que no tenga un objeto, sea porque la hemos eliminado, sea porque nunca ha existido. Por ejemplo, `persona.altura` también sería `undefined`

- Para iterar por todas las propiedades de un objeto usamos `for...in`

```javascript
for (propiedad in objeto) {
  //nótese que objeto.propiedad sería la propiedad llamada "propiedad"
  console.log(objeto[propiedad]);
}
```

- Podemos inicializar un objeto usando *notación literal*: se ponen los campos del objeto como pares `propiedad:valor`, separados por comas. Todo ello delimitado por un par de llaves

```javascript
var persona = {
  nombre: "Homer Simpson",
  edad:34,
  casado:true,
  saludo: function() {
    console.log("Hola, soy " + this.nombre);
  },
  //Se pueden especificar arrays usando corchetes
  hijos: ["Bart", "Lisa", "Maggie"],
  //Un objeto puede contener a su vez otros objetos
  profesion : {
    puesto: "técnico nuclear",
    lugar: "central de Springfield"
  },
  //Si el nombre de una prop. contiene espacios o caracteres especiales, se pone entre comillas
  "nombre esposa": "Marge"
};
```

- El formato **JSON** (*JavaScript Object Notation*), muy usado en la actualidad tanto dentro como fuera de Javascript es lo mismo que el formato literal, pero no se admiten caracteres especiales en los nombres de las propiedades

> En JSON existe una forma estándar de representar cadenas, enteros, booleanos, arrays y objetos genéricos, pero no fechas u otros objetos de la librería estándar como expresiones regulares. Tampoco se define cómo representar el valor `undefined`.

### Prototipos

- Javascript es prácticamente el único lenguaje *mainstream* orientado a objetos que **originalmente no incluía la idea de clase ni de herencia basada en clases**, sino basada en **prototipos**
- Cuando creamos un objeto podemos especificar cuál queremos que sea su *prototipo*. Si el objeto no tiene una propiedad, se buscará en el prototipo
- Si la propiedad sigue sin encontrarse en el prototipo, se irá al prototipo del prototipo, y así sucesivamente hasta llegar a `Object.prototype`.
- Podemos ver esto como **una forma de herencia en la que un objeto concreto hereda de otro**, en lugar de una clase de otra.


[DEMO en repl.it](https://repl.it/@ottocol/SuddenCorruptObjectcode)
```javascript
var original = {
  nombre: "original",
  saludar: function() {
    return "hola, qué tal";
  }
}

//El prototipo de "descendiente" es "original"
var descendiente = Object.create(original);
console.log(descendiente.nombre) //"original"
console.log(descendiente.hasOwnProperty("nombre")) //nos dice que la propiedad no está directamente en descendiente
console.log(descendiente.saludar()) //"hola, qué tal"
original.nombre = "original_2"
console.log(descendiente.nombre) //el mismo, el valor se comparte!!
descendiente.nombre = "descendiente"  //propiedad nueva en descendiente
console.log(descendiente.hasOwnProperty("nombre")) //Ahora será true
```

### Clases

- La herencia orientada a prototipos es ajena a la experiencia del 99% de los desarrolladores, acostumbrados a la herencia basada en clases de lenguajes como Java o C++. Tanto es así que en Javascript han surgido multitud de patrones de código e incluso librerías para poder definir y usar clases. 
- Sin acudir a librerías externas, **la forma más habitual de definir una clase en versiones anteriores a ES2015** consiste en definir una función con el nombre de la clase, usarla como constructor, y asignarle propiedades al prototipo de esta función. Podéis ver un ejemplo [aquí](https://leanpub.com/understandinges6/read#leanpub-auto-class-like-structures-in-ecmascript-5) de este patrón, usado en multitud de sitios.
- Finalmente en ES2015 se añadieron clases al lenguaje, con una sintaxis similar a la de otros lenguajes más clásicos. 

[DEMO en repl.it](https://repl.it/KxLq/0)
```javascript
class Persona {
    constructor(nombre) {
        this.nombre = nombre
    }

    saludar() {
        console.log("Hola, soy " + this.nombre)
    }

    get miNombre() {
        return this.nombre
    }

    set miNombre(nuevoNombre) {
        this.nombre = nuevoNombre
    }
}

let p = new Persona("Pepe")
p.saludar()   //Hola, soy Pepe
p.miNombre = "DJ Pep"
console.log(p.miNombre) //DJ Pep
```

A destacar del código anterior:

* Los constructores se definen con la palabra clave `constructor`
* A diferencia de la notación literal no es necesario separar los nombres de los métodos por comas
* Podemos definir métodos que actúen de *getters* y *setters* con las palabras clave `get`y `set`. En el ejemplo, al acceder a/cambiar el valor de la propiedad `miNombre` en realidad estamos llamando al *getter* y al *setter*, respectivamente.
* A diferencia de lenguajes como C++/Java, no hay modificadores de acceso (`public`, `private,...)

> NOTA: A pesar de las clases, Javascript sigue usando internamente herencia basada en prototipos. Es decir, las clases son "azúcar sintáctico".

Para crear una clase que herede de otra la definimos con `extends`

[DEMO en repl.it](https://repl.it/KxLq/1)
```javascript
class StarWarsFan extends Persona {
    constructor(nombre) {
        super("Darth " + nombre)
    }

    saludar() {
        super.saludar()
        console.log("Yo soy tu padre")  
    }
}

let juan = new StarWarsFan("Juan")
juan.saludar()  //Hola, soy Darth Juan\n Yo soy tu padre
```

Como vemos, con `super` podemos invocar el constructor o los métodos de la clase base. 

Si no definimos constructor en la clase heredada, el intérprete Javascript define automáticamente uno que llama al de la clase base.

## Arrays

- Son *colecciones dinámicas* de objetos accesibles por posición, más que *arrays* al estilo C/Java
    * Cada posición de un array puede ser de un tipo distinto
    * Al crear un array no es necesario especificar el tamaño. Se pueden añadir/eliminar elementos y el tamaño cambiará dinámicamente. 

```javascript
//
var a = new Array();    //O usando notación literal, var a = []
a[0] = 33;              //Podemos añadir elementos especificando su posición
a[1] = "hola";
a.push("el último");    //Otra forma de añadir "por la cola"
//Al igual que en Java, la propiedad length indica el tamaño del array
console.log(a.length);  //3
delete a[0];            
console.log(a[0]); //Al borrar una posición, esta pasa a ser undefined...
//... y el tamaño no cambia. 
console.log(a.length);  //¡¡3!! 
//Así, podemos ver "length" como el índice del último elemento+1,
//más que como el tamaño. Pero si no hay "undefined" coincidirán ambos
a[100] = "¡último ahora!"; //los elementos entre la pos. 3 y la 99 son "undefined"
```

- La [clase Array](https://developer.mozilla.org/es/docs/Web/JavaScript/Reference/Global_Objects/Array) implementa muchos métodos interesantes para trabajar con arrays, por ejemplo para añadir/eliminar elementos, iterar, buscar elementos, ...



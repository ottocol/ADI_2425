# Introducción a Firebase

Firebase es una plataforma de *Backend As A Service* que nos permite implementar de forma rápida y sencilla los servicios más comunes de *backend* que puede necesitar nuestra aplicación cliente/servidor. Por ejemplo se puede ocupar de la autentificación de los usuarios, de almacenar datos en el servidor en una base de datos escalable, de avisar a los usuarios con notificaciones *push* o de servicios complementarios como gestionar analíticas de uso o el *hosting* de las páginas web.

Aunque en la asignatura usaremos Firebase para desarrollar aplicaciones web, no está limitada a esta plataforma, también podemos desarrollar apps móviles en Android o iOS, apps en Unity, en Flutter y otras ...



## Crear proyectos y aplicaciones en Firebase

Para usar Firebase solo necesitamos una cuenta de Google. La consola de administración está en [https://console.firebase.google.com](https://console.firebase.google.com). Aquí aparecerán nuestros proyectos, si tenemos alguno,  la opción de crear uno nuevo y la de usar uno ya creado de demostración.

![](intro_firebase_imag/consola_firebase.png "Consola de Firebase sin proyectos creados")


Firebase distingue entre "proyectos" y "aplicaciones":

- Un "proyecto" de Firebase es la parte del servidor de una aplicación, la infraestructura manejada por Firebase que nos permite almacenar los datos de los usuarios, los datos de la aplicación, las notificaciones *push*, etc.
- Una "aplicación" de Firebase es la parte del cliente, aquí tendremos tanto el código que gestiona la interfaz que ve el usuario como las llamadas al API de Firebase que interactúan con la infraestructura del proyecto en el servidor, es decir que por ejemplo intentan registrar un nuevo usuario, averiguar si un login y un password son correctos, guardar o recuperar datos de la base de datos, etc...

Un solo "proyecto" puede tener asociadas varias "aplicaciones" ya que por ejemplo podríamos tener una red social que tuviera acceso mediante web o mediante aplicación móvil en iOS o Android. Para todos los casos la infraestructura en el servidor sería la misma.

Vamos a crear un proyecto de prueba y luego añadirle una aplicación web:

1. En la consola de Firebase hay que pulsar sobre **"Comenzar con un proyecto de Firebase"**
2. **Elegimos un nombre** para el proyecto, por ejemplo `demo-ADI` (no será público, solo visible para nosotros). Debajo del nombre elegido aparecerá un identificador único, basado aproximadamente en el nombre elegido más un sufijo numérico. Este nombre es el que se usará en ciertos ficheros de configuración de nuestro código.
3. En el siguiente paso podemos elegir si queremos usar **Google Analytics** para controlar las visitas a nuestras aplicaciones, podemos deshabilitarlo, en este ejemplo no nos va a hacer falta
4. En la siguiente pantalla aparecerá una barra de progreso circular indicando que se está creando el proyecto.
5. Una vez creado, en la siguiente pantalla podemos añadir aplicaciones al mismo. Para añadir una aplicación web lo hacemos pulsando sobre el icono `</>`.


![](intro_firebase_imag/add_app.png "Añadir aplicación al proyecto")


6. En la pantalla "Agregar Firebase a tu aplicación web" tenemos dos pasos:
     1. Elegir nombre para la aplicación y marcar la casilla si queremos usar Firebase como servicio de *hosting* para alojar las páginas web. Como en este caso es una demo de prueba y ni siquiera va a haber páginas web no es necesario marcarla. Una vez seleccionado lo que deseemos, pulsar sobre el botón de "Registrar app"
   	 2. El paso de "Agregar el SDK de Firebase" nos muestra las instrucciones a seguir para comenzar a crear el código fuente para nuestra aplicación. En el caso de las aplicaciones web hay dos opciones. Elegiremos la de "usar npm". Debajo aparecerán instrucciones de instalación de las bibliotecas de Firebase y cómo inicializar Firebase en nuestro código. A continuación lo veremos con más detalle. 

> La opción más "moderna" y aplicable a proyectos grandes, es la que se referencia en la documentación como "modular" y que aquí pone como "usar npm" y la más antigua y aconsejada solo para pruebas o proyectos pequeños que es "usar una etiqueta `<script>`". Aunque en nuestro caso nos bastaría con la segunda elegimos la primera para ver cómo se usa. 

## Crear el código fuente de una aplicación web

Con "usar npm" indicamos que vamos a usar este gestor de paquetes para gestionar la aplicación. Como ya hemos visto lo primero que necesitamos es inicializar un proyecto "npm"

```bash
mkdir demo_ADI
cd demo_ADI
npm init -y #contestar "si" por defecto a todas las preguntas
```

Como dicen las instrucciones de la consola de Firebase necesitamos instalar su dependencia:

```bash
npm i firebase
```

Esto debería crear la carpeta `node_modules` con las librerías de Firebase, y actualizar el `package.json` con la dependencia.


Vamos a añadir un usuario a nuestra aplicación para poder probar que podemos hacer login correctamente. En la consola tenemos que ir a "Authentication". Firebase admite distintos métodos de autentificación o como se llaman aquí, "proveedores" o "métodos de acceso", como por ejemplo, autentificar a los usuarios con email/contraseña, con su cuenta de Google/Facebook/Twitter etc..., usando otros proveedores como OpenID, ... Nosotros vamos a usar el de email/contraseña y dar de alta un usuario de ejemplo con email ` adi@ua.es` y contraseña `adiadi`.

![](intro_firebase_imag/add_user.gif)

Ahora vamos crear un archivo `prueba.js` para intentar autentificarnos por código y comprobar que funciona:


```javascript
// Import the functions you need from the SDKs you need
import { initializeApp } from "firebase/app";
import { getAuth } from "firebase/auth";

// TODO: Add SDKs for Firebase products that you want to use
// https://firebase.google.com/docs/web/setup#available-libraries

// Your web apps Firebase configuration
const firebaseConfig = {
  //IMPORTANTE: sustituir todo esto POR LOS DATOS 
  //QUE OS APAREZCAN A VOSOTROS EN LA CONSOLA
  apiKey: "xxxxxx",
  authDomain: "xxxxxxxxx",
  projectId: "xxxxxxx",
  storageBucket: "xxxxxxxxx",
  messagingSenderId: "xxxxxxxxx",
  appId: "xxxxxxxxx"
};

// Initialize Firebase
const app = initializeApp(firebaseConfig);

//Inicializar autentificación
const auth = getAuth(app)

try {
   //"await" indica que es una operación asíncrona
   const usuario = await signInWithEmailAndPassword(auth, "adi@ua.es", "adiadi")
   console.log(usuario.user.email) 
}
catch (error) {
    console.error(error.message)
}
```

> Si no os habéis guardado los datos del objeto `firebaseConfig` los podéis ver en las propiedades de la app en la consola de Firebase. Por ejemplo en la barra de navegación de la izquierda, en la parte superior hacéis click sobre el icono de engranaje > Configuración de proyecto y aparecerá una página con todas las apps asociadas (de momento solo tendréis la web). En las propiedades de la app deben aparecer los datos del `firebaseConfig`.

IMPORTANTE: estamos usando otra sintaxis alternativa a la que vimos en la introducción a node y npm para importar módulos Javascript. En concreto estamos usando el sistema ESM (ECMA Script Modules) que podemos ver como la versión "moderna" de los módulos, y no CommonJS. Dicha sintaxis nació en el lado del cliente (navegador) aunque se puede usar también en node. No obstante como no es la sintaxis nativa de node necesitamos añadir una línea indicándolo al `package.json`, la línea en cuestión es `"type": "module"` y la podemos añadir en cualquier parte del `package.json` como una propiedad más:

```json
{
	"name" : "demo_ADI",
	//otras propiedades
	...	
	...
	"type": "module",
	"dependencies": {
	   "firebase": "^10.13.1"
	}
}
``` 

Si ahora probamos el código de `prueba.js` debería autentificar al usuario en Firebase e imprimir su email. Si probáis a cambiar el email o la contraseña por uno incorrecto, se generará una excepción `FirebaseError` y saltará al `catch` imprimiendo un mensaje de error en la terminal.

> El uso de `await` antes de la llamada a `signInWithEmailAndPassword` indica que esta es una operación asíncrona que puede tardar un tiempo indefinido y que la ejecución debe "pausarse" y esperar a que termine la operación, por tanto nos aseguramos que la siguiente línea de código con el `console.log` no se ejecuta hasta que ha terminado el `signInWithEmailAndPassword`. Puedes quitar el `await` y verás que no funciona correctamente.


## Breve introducción a la programación asíncrona en Javascript


Javascript es un lenguaje *monohilo* por diseño. Eso quiere decir que cuando queremos ejecutar una operación costosa en tiempo de ejecución no podemos hacerlo en un hilo adicional para que se ejecute en paralelo con el código principal, lo que haremos en Javascript es usar código asíncrono, lanzando la operación y "diciéndole al intérprete" qué debe continuar ejecutando cuando ésta acabe.

En Javascript se pueden usar tres métodos para escribir código asíncrono:

- *callbacks*: es el más antiguo y que genera un código más embarullado y tiende a no usarse demasiado salvo en APIs antiguos. En nuestro caso, los APIs modernos de Firebase ya no usan *callbacks* por lo que no los veremos aquí de momento. Como explicación breve diremos que un callback es un bloque de código en forma de función que se le pasa a un método asíncrono para indicarle que lo llame cuando la operación haya terminado.
- *promesas*: una promesa es un objeto que representa una operación asíncrona. Puede estar en varios estados: pendiente/*pending*, cumplida/*fulfilled* (ha acabado con éxito) o rechazada/*rejected* (ha acabado con error). Veremos su uso a continuación.
- *async/await*: aunque internamente usa promesas es una forma de escribir código asíncrono lo más parecido a como si fuera síncrono, por lo que la estructura queda mucho más "limpia".

La mayoría de APIs modernos en Javascript usan las promesas para manejar el código asíncrono. Veremos primero la forma clásica, que genera código un poco "embarullado" (aunque mucho más legible que con los *callbacks*) y luego la que usa "async/await".

### Uso de promesas con `.then`

`then` es un método de la clase `Promise` al que le pasamos una función a ejecutar cuando la promesa termine con éxito y que devuelve otra promesa con el resultado de la anterior (lo cierto es que la descripción es un poco liosa pero espero que se vea mejor con un ejemplo):

Por ejemplo si leemos la documentación de Firebase [sobre la función `signInWithEmailAndPassword`](https://firebase.google.com/docs/reference/js/v8/firebase.auth.Auth#signinwithemailandpassword) veremos que devuelve una promesa. Para indicar que queremos ejecutar un código cuando acabe la promesa con éxito se lo pasamos al `.then`:

```javascript
signInWithEmailAndPassword(auth, "adi@ua.es", "adiadi").then(function(result){
   console.log(result.user.email)
})
```
La función que pasamos al `then` va a recibir automáticamente uno o más parámetros con el resultado de la operación asíncrona, para ver qué son estos parámetros hay que mirar la documentación del API que estamos usando. [En nuestro caso](https://firebase.google.com/docs/reference/js/v8/firebase.auth.Auth#signinwithemailandpassword) se trata de un objeto `UserCredential` en el que el campo `user` contiene la información básica del usuario.

Si la promesa acabara con error, podríamos detectarlo con un método especial `.catch` que se encadena con el resultado del `.then`

```javascript
signInWithEmailAndPassword(auth, "adi@ua.es", "adiadi")
 .then(function(result){
   console.log(result.user.email)})
 .catch(function(error){
   console.log("Se ha producido un error: " + error.message)
 })
```

Lo interesante de las promesas es que se pueden encadenar varias operaciones asíncronas, asegurando que cada una de ellas solo se ejecuta cuando ha acabado la anterior.

Aunque es un ejemplo un poco forzado, supongamos que tras autenticar al usuario quisiéramos obtener un *token* de autentificación. Los *tokens* de autentificación son valores alfanuméricos que se usan para autentificar a un usuario ante el servidor, en lugar de tener que usar login y password. La idea es que primero verificamos que el login y password son correctos, si es así obtenemos el *token* y a partir de ahí usamos el *token* para cualquier operación que requiera autentificación. En Firebase podemos obtener un *token* de autentificación (de tipo JWT, como veremos en sucesivas clases de teoría) con `getIdToken` del objeto `user`:


```javascript
signInWithEmailAndPassword(auth, "adi@ua.es", "adiadi")
  .then(function(result){
     console.log(result.user.email)
     return user.getIdToken()})
  .then(function(token){
     console.log("token obtenido: " + token)	
  })
```

Hay varias cosas a destacar del código anterior:

- La mecánica para encadenar varias operaciones asíncronas es siempre llamar a la siguiente operación asíncrona y devolver su valor.
- Como una llamada a una operación asíncrona devuelve una promesa, podemos encadenar este valor de retorno con un `.then`, lo que nos permite crear "una cadena de `.then`" con todas las operaciones.

> Las promesas son bastante potentes. Además de encadenar varias operaciones asíncronas de forma secuencial podemos ejecutar operaciones en paralelo usando un método llamado `.all`, aunque no lo veremos aquí.

### Uso de promesas con `async/await`

Aunque la sintaxis del `.then` es una mejora considerable con respecto al uso de *callbacks* (¡creedme!), sigue siendo tediosa y algo confusa. La funcionalidad de `await`, que se introdujo en Javascript algunos años después que el `.then`, nos permite escribir código asíncrono de forma mucho más limpia:

```javascript
try {
    const result = await signInWithEmailAndPassword(auth, "adi@ua.es", "adiadi")
    console.log(result.user.email)
}
catch(error) {
    console.log("Se ha producido un error: " + error.message)
}
``` 

`await` se coloca delante de cada método que devuelva una promesa y desde el punto de vista del programador asegura que el código no va a continuar hasta que la promesa termine, una vez hecho esto nos devolverá el resultado. En caso de terminar con error para detectarlo ahora podemos usar un `catch` estándar de Javascript en lugar de tener que usar el `.catch` especial para las promesas.

Es muy importante destacar que poner await delante de una llamada asíncrona no la convierte en síncrona, sigue siendo código asíncrono y sigue teniendo sus peculiaridades. Por ejemplo si envolviéramos el código anterior en una función para poder llamarlo desde donde queramos habría que marcar la función como `async` para indicar que contiene código asíncrono.

```javascript
async function doLogin(auth, login, password) {
	try {
	    const result = await signInWithEmailAndPassword(auth, "adi@ua.es", "adiadi")
	    console.log(result.user.email)
	}
	catch(error) {
	    console.log("Se ha producido un error: " + error.message)
	}
}
```  

Las funciones marcadas con `async` no se pueden llamar como las síncronas, siempre tienen que usarse con `await`, así si quisiéramos llamar desde otra función a `doLogin()` tendríamos que hacer:

```javascript
//como este código llama usa await, también pasa a ser asíncrono
async function miOtraFuncion(auth) {
	const result = await doLogin(auth, "adi@ua.es", "adiadi")
	//Nótese que esto sería INCORRECTO 
	//const result = doLogin(auth, "adi@ua.es", "adiadi")
	console.log(result.user.email)
}
```

Nótese que cuando llamamos a código asíncrono con `await` toda la cadena de funciones que acaba llamando a este código asíncrono pasa a ser asíncrona y por tanto todas las funciones se deben marcar con `async`. 






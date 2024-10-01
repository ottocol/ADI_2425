# Introducción básica a Firestore

En Firebase hay dos tipos de bases de datos para nuestras aplicaciones:

- **Firestore**: es una base de datos NoSQL similar a MongoDB, en lugar de tablas usa el concepto de colecciones de documentos en formato JSON.
- **Realtime Database**: cada base de datos de este tipo es un único árbol JSON que engloba toda la información almacenada. Se extrae o inserta la información especificando la trayectoria donde la queremos, como en un árbol de directorios.

Veremos aquí Firestore ya que os puede resultar más accesible si habéis usado MongoDB en otras asignaturas. No es una base de datos trivial de usar, más si se viene a ella sin haber usado ninguna otra base de datos NoSQL por lo que aquí solo podemos hacer una introducción muy sencilla. Si queréis saber más sobre Firestore os recomiendo que veáis la serie de videos ["Get to know Cloud Firestore"](https://www.youtube.com/playlist?list=PLl-K7zZEsYLluG5MCVEzXAQ7ACZBCuZgZ) en el [canal de Youtube de Firebase](https://www.youtube.com/@Firebase).


## Comenzar con Firestore

En la consola de Firebase, seleccionamos el proyecto en el que queramos usar Firestore y en la pantalla "Elige un producto para agregarlo a tu app" pulsamos sobre su icono. En el siguiente paso pulsamos sobre el botón "Crear base de datos". 

En el asistente podremos elegir 1) La ubicación geográfica del servidor para la base de datos y 2) Las reglas de seguridad. Para estas últimas se recomienda de momento "comenzar en modo de prueba" para permitir lecturas y escrituras a cualquier cliente en los próximos 30 días.

> Las reglas de seguridad de Firestore nos permiten especificar los permisos para las diferentes operaciones a realizar con las distintas partes de la base de datos. Por ejemplo podemos especificar que cierta colección tenga acceso público de lectura y escritura y que otra solo se pueda modificar si el usuario está autentificado en Firebase. O bien cosas como que solo se pueda modificar un documento si el campo `id_creador` coincide con el id del usuario autentificado.

Aparecerá la consola de Firestore donde podemos agregar/editar datos, reglas de seguridad, índices para facilitar las búsquedas y otras operaciones.

Para poder editar los datos primero necesitamos conocer algo más sobre el modelo de datos de Firestore.

## Modelos de datos en Firestore

Firestore es una base de datos NoSQL, lo que quiere decir que se aleja del paradigma de las tablas, los esquemas, las relaciones entre tablas, las consultas con JOIN, las formas normales, etc.

Una base de datos de Firestore se compone de una o más **colecciones** de **documentos**. A nivel superficial, las colecciones son como *tablas* en una base de datos SQL y los documentos como *registros* pero con ciertas diferencias:

- **Las colecciones no tienen un *esquema* fijo**, cada documento dentro de una colección podría tener una estructura completamente distinta (aunque en realidad lo habitual es que todos los documentos de una colección tengan prácticamente los mismos campos)
- Los **documentos se definen en notación literal de Javascript** (similar a JSON). Por tanto un documento puede contener no solo valores "simples" como cadenas, números o fechas sino también valores "compuestos" como arrays u objetos enteros.
- **No hay forma de relacionar automáticamente las colecciones entre sí**. Si bien en una colección de `Pedidos` podemos tener un campo `id_usuario` que vincule cada pedido con el usuario que lo ha realizado, es una relación que mantenemos nosotros y no el sistema, que no validará automáticamente que el usuario exista en la colección de `Usuarios`.
- Las colecciones pueden tener **subcolecciones**. Eso quiere decir que podríamos modelar la relación entre usuarios y pedidos haciendo que `Pedidos` fuera una subcolección de `Usuarios` (salvando las distancias, es como si en una BD SQL un registro pudiera tener dentro una tabla). Así cada usuario tendría además de sus datos (`id`, `nombre`, ...) una colección de `Pedidos` que ha hecho.
- La **información puede estar duplicada**: como decíamos no se usan las formas normales. Podemos tener una subcolección de direcciones de envío dentro de cada usuario y tener también la información de la dirección de envío repetida dentro del pedido, para si recuperamos los datos del pedido tener ya la dirección sin tener que hacer otra consulta a la base de datos.

Un problema típico viniendo del mundo de las bases de datos SQL es cómo relacionar los datos entre sí. Volviendo al ejemplo de los usuarios y los pedidos, diremos que podemos hacerlo de tres maneras:

- **Usando colecciones de "nivel superior"**: es decir colecciones que no son subcolecciones de otras. Con un campo `id_usuario` en la colección de `Pedidos` podríamos buscar fácilmente los pedidos de un usuario dado. Tener la colección de pedidos de forma independiente de los usuarios también nos facilita hacer búsquedas en ella.
- **Usando subcolecciones**: como decíamos antes podríamos hacer que `Pedidos` fuera una subcolección de `Usuarios`. Esto nos haría muy sencillo obtener los pedidos de un usuario dado o buscar dentro de los pedidos de un usuario, pero dificulta las búsquedas genéricas dentro de la colección de `Pedidos` ya que ahora no están todos juntos, sino separados por usuario (se puede hacer pero requiere la creación de lo que en Firebase se llaman *índices compuestos*)
- "Embebiendo" los datos de los pedidos dentro del documento que representa al usuario (**usando lo que en Firebase llaman *maps***, que serían como objetos dentro del objeto usuario). Esto imposibilita las búsquedas pero es apropiado en los casos en que siempre que recuperemos la información del objeto "padre" nos interese también recuperar la de los "hijos". Para el caso que nos ocupa no parece lo más apropiado usarlo para usuarios y pedidos pero sí podríamos guardar los productos dentro de un carrito o la dirección de envío de un pedido.

como vemos, la decisión de cómo estructurar los datos en una base de datos Firestore está íntimamente ligada a los casos de uso que preveemos que van a ser más comunes con esta base de datos.

## Probar Firestore en nuestro código. CRUD de datos

Para usar Firestore en el código de nuestro proyecto, además de activarlo en la consola de Firebase, necesitamos el objeto `FirebaseApp` que a su vez depende del `FirebaseConfig`.

En este primer ejemplo vamos a ver cómo añadir elementos a la BD y cómo leer datos.

```javascript
import { initializeApp } from "firebase/app";
import { getDoc, getFirestore } from "firebase/firestore";
import { collection, addDoc, doc} from "firebase/firestore"; 

//Esto debería estar relleno con los valores que aparezcan en la consola de Firebase para este proyecto
const firebaseConfig = {
  apiKey: "...",
  authDomain: "...",
  projectId: "...",
  storageBucket: "...",
  messagingSenderId: "...",
  appId: "..."
};


const app = initializeApp(firebaseConfig);
const db = getFirestore(app);
let docRef = undefined
try {
  //Añadimos un documento dentro de la colección de nivel superior "usuarios"
  //si la colección no existiera todavía, se crearía
  //addDoc devuelve la referencia al elemento creado
  docRef = await addDoc(collection(db, "usuarios"), {
    nombre: "Ada Lovelace",
    registro: new Date()
  });
  console.log("Document written with ID: ", docRef.id);
} catch (e) {
  console.error("Error adding document: ", e);
}

//La referencia a un documento siempre es la referencia a la colección que lo contiene + su id

const miDoc = await getDoc(docRef)
console.log(miDoc.data())
//también podríamos obtener la referencia al usuario "Ada Lovelace"
//concatenando la colección "usuarios" con el id del usuario
//const adaRef = doc(db, "usuarios", docRef.id)
```

Para referenciar un elemento dentro de la base de datos tenemos que conocer su trayetoria desde la raíz. Recordemos que en una base de datos de Firestore solo hay documentos y colecciones.  Por la propia estructura de Firestore en la trayectoria se irán alternando colecciones y documentos (ya que una colección contiene documentos y un documento puede contener una colección).

Podemos **obtener la referencia** a un documento con el método `doc()` y a una colección con `collection()`. En ambos casos el primer parámetro es la base de datos y el siguiente o siguientes la trayectoria, que podemos ir dando por partes `doc(db, "usuarios", docRef.id)` o bien separada por barras `doc(db, "usuarios/" + docRef.id)`.

Una vez obtenida la referencia, si es de un documento lo **leemos** con `getDoc()` y si es de una colección con `getCollection()`.

Para añadir un documento a una colección usamos `addDoc()`. Si queremos crear una colección lo haríamos añadiendo el primer documento a la colección vacía.

Aunque en el ejemplo no aparece, para actualizar los datos de un documento usaríamos `updateDoc()` pasando la referencia y un objeto con los nuevos campos a sobreescribir.

```javascript
await updateDoc(docRef, {
    nombre: "Grace Hopper"
})
```

Finalmente podemos borrar un documento con `deleteDoc(referencia)`. Si el documento contiene colecciones estas no se borrarían automáticamente, y sigue pudiendo accederse a ellas aunque en la trayectoria aparezca el id del documento borrado. Por ejemplo si los usuarios contuvieran una subcolección `direcciones` podríamos  seguir acccediendo a `usuarios/122212ftg/direcciones` aunque borráramos el usuario `usuarios/122212ftg`. Para [borrar una colección](https://firebase.google.com/docs/firestore/manage-data/delete-data#collections) tenemos que acceder a ella e ir borrando los documentos que la componen uno a uno.
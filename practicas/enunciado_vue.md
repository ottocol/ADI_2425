# Práctica evaluable 3: Frameworks Javascript

El objetivo de esta práctica es desarrollar una web que sirva de cliente al backend de la práctica 1. En la implementación debéis usar un *framework JS*, en concreto **Vue**

## Organización del código

Para crear el proyecto cliente usad el comando `npm create vue@latest`, que crea un proyecto usando la herramienta [`create-vue`](https://github.com/vuejs/create-vue). Al ejecutar el comando `npm create vue@latest` aparecerá un asistente en modo texto que os irá preguntando funcionalidades y características del proyecto. Podéis decirle a todo que no salvo al "Vue Router" y "Pinia" y os recomiendo que también instaléis las "Vue DevTools" ("Vue Router" es para una parte optativa pero tampoco os molestará tenerlo instalado aunque no lo uséis).

Una vez creado el proyecto, debéis copiar el código fuente de vuestra práctica 1 a la carpeta `src` del proyecto de Vue.


## Requisitos mínimos

La implementación correcta de estos requisitos es necesaria para poder aprobar la práctica. Con estos requisitos podéis optar hasta un 6 si además de la implementación comentáis adecuadamente el código fuente, indicando para cada componente: qué hace en líneas generales, qué eventos genera o procesa, qué representa cada variable del estado del componente (cada variable reactiva definida con `ref` o `reactive`) , cada prop, y cada método (descripción de lo que hace, parámetros, efecto en el estado si lo tiene).

Debéis desarrollar una web en la que como mínimo se pueda hacer:

- Login/logout
- Listado de items: pueden ser todos o puede haber un cuadro de búsqueda asociado. Si permitís búsqueda e implementáis paginación en el cliente tendréis **0.5 puntos más**
- Eliminación de items (similar al botón "x" de la lista de la compra)
- Creación de items
- Ver detalles: al clicar en un item o en un botón asociado a cada item, se muestra más información sobre el mismo (sin cambiar de página)
- Edición de items: cada elemento del listado debería tener un botón o enlace "editar" para cambiar su contenido. Al pulsarlo aparecerá un formulario para escribir los nuevos datos. Dónde y cómo aparezca (y desaparezca al acabar la edición) queda a vuestra elección. Los formularios de edición también deben validarse.

> Pongo "items" como término genérico porque cada uno estáis haciendo una aplicación distinta, en vuestro caso concreto serán "productos" o "usuarios" o "películas" o lo que proceda.

**Los formularios de login/creación de items deberían estar validados** (por ejemplo login y password no pueden estar vacíos, si tenéis algún campo numérico hay que chequear que lo sea, etc), sin perjuicio de que los datos también se chequeen en el servidor. Para implementar la validación podéis usar el método que queráis. Por ejemplo se pueden usar [atributos de HTML y JS estándar](https://developer.mozilla.org/en-US/docs/Learn/Forms/Form_validation) o emplear una librería de terceros, como por ejemplo [VeeValidate](https://vee-validate.logaretm.com/v4/) para Vue.

> Ojo, la validación con HTML/JS estándar se dispara en algunos casos con el evento "submit" de un formulario. Cuando este evento llega al navegador, el comportamiento por defecto es cargar una página nueva, o recargar la actual, esto hará que el estado de los componentes se pierda, y no es una buena idea :). Si usáis este método tendréis que aseguraros de que el evento submit se anula en vuestro *listener* con un `preventDefault`.

Además, **se debe usar [Pinia](https://pinia.vuejs.org/) para gestionar el estado de vuestra aplicación de manera centralizada**. En el *store* debería estar como mínimo:

- La información sobre el usuario autentificado y los métodos de negocio de login/logout/registrar usuario (si tenéis este último)
- Los datos de los listados que mostréis en la web
- El código de otros métodos de lógica de negocio si los tenéis


## Requisitos adicionales

Además de los 6 puntos de los requisitos mínimos, podéis implementar los siguientes requisitos adicionales:

- hasta *0.5 puntos* búsqueda y paginación de datos, evidentemente para esto tendría que estar implementado también en el servidor. Si no lo hicisteis en su día y queréis hacerlo ahora, podéis añadirlo (aunque no os puedo dar puntos extra por la parte del servidor).
- Hasta *0.5 puntos* introducir transiciones/animaciones cuando aparezcan/desaparezcan elementos de la página. Añadir además el típico efecto de que el botón/formulario "tiembla" cuando introducimos un login/password incorrectos. Vue incorpora ya integrada la [funcionalidad](https://v3.vuejs.org/guide/transitions-overview.html#class-based-animations-transitions) para hacer estas transiciones
- hasta *1 punto* si usáis algún *framework* de componentes visuales. Para Vue tenéis por ejemplo [vuetify](https://vuetifyjs.com/en/) o [bootstrap-vue](https://bootstrap-vue.org/)
- hasta *0.5 puntos*: usar un *router* para gestionar la navegación por la *app*. En el caso de Vue se usaría [Vue Router](https://router.vuejs.org)
- hasta *0.5 puntos*: implementar el listado de otro recurso incluyendo también "ver detalles", "eliminar" y "editar".
- Hasta *1 punto*: implementar el listado y eliminación de items con otro framework cualquiera (Svelte, Angular, React,...). No es necesario implementar editar y ver detalles en este caso. Tendréis que hacer un proyecto nuevo ya que la estructura del proyecto de Vue no os va a servir para otro *framework*.

Cualquier otra funcionalidad que queráis añadir consultadla antes conmigo para ver cuánto se podría valorar en el baremo.

## Entrega

El plazo de entrega concluye el **~~lunes 2 de diciembre de 2024 a las 23:59 horas~~** **domingo 14 de diciembre de 2024 a las 23:59 horas**. La entrega se realizará como es habitual a través de Moodle.

Entregad también un LEEME.txt donde expliquéis brevemente las partes optativas que habéis hecho y cualquier detalle que consideréis necesario.
# Práctica 1: Desarrollo del *backend* de la aplicación web


En esta práctica desarrollaremos el *backend* de una aplicación web usando Firebase. Podéis usar el dominio que queráis para la aplicación: una tienda, una red social, un sitio educativo para cursos *online*, ... lo que elijáis.

> Por el momento vamos a **implementar solamente el backend, no el sitio web**. Este se desarrollará a partir de la práctica siguiente. Eso quiere decir que por el momento **no tendremos interfaz de usuario, no habrá nada de HTML**. Probaremos el código bien manualmente en línea de comandos o bien con pruebas unitarias. 

El desarrollo de la práctica 1 tendrá las partes que se detallan a continuación. Por supuesto debéis entender esto como un proceso iterativo, por ejemplo al diseñar el sitio web (parte 3) podéis daros cuenta de que alguna "pantalla" requiere de funcionalidades que no habéis considerado en los requerimientos iniciales (parte 1) o bien requiere modificar el modelo de datos (parte 2). Al entregar la documentación de la práctica debéis entregar el estado final y también brevemente podéis explicar las iteraciones y cambios sucesivos que habéis ido haciendo.

Con los requisitos que se piden en las 4 partes de la práctica podéis obtener hasta un 6 en la nota. Si desarrolláis alguno o todos los **requisitos adicionales** podéis obtener más nota hasta el 10, pero no se os contará ningún requisito adicional si no habéis realizado (al menos parcialmente) todos y cada uno de los requisitos "obligatorios".

> **AVISO MUY IMPORTANTE**: Podéis usar LLMs (ChatGPT, Claude, LLama, ...) para ayudaros con el planteamiento, desarrollo o implementación de la práctica. De hecho, os recomiendo que los uséis ya que son muy prácticos en estas tareas. No obstante **vosotr@s sois l@s responsables de cada decisión de diseño e implementación**. Esto quiere decir que si se revisa vuestra práctica con vosotr@s delante tenéis que ser capaces de explicar el por qué de cada decisión en el diseño de la web, el modelo de datos o cómo funciona el código entregado. Si la práctica la hacéis entre dos personas ambas tenéis que conocer cómo funciona todo aunque esa parte en concreto la haya realizado inicialmente vuestr@ compañer@.

## Parte 1: Requerimientos iniciales (0.5 puntos)

Podéis empezar creando una lista de requerimientos iniciales o casos de uso. 

> NOTA: en este apartado llamamos "recursos" a las entidades que forman parte del dominio de nuestra aplicación. Por ejemplo en la *app* de *crodfunding* los recursos serían los usuarios, los proyectos, las noticias sobre cada proyecto...

Aunque no es posible dar requerimientos más concretos, ya que cada uno vais a desarrollar algo completamente distinto, vuestro sitio debería incluir las siguientes funcionalidades genéricas:

- **Autentificación de usuarios**: ciertas operaciones deberían estar restringidas solo a usuarios autentificados mediante email y password. Se debe poder hacer login y logout.
- **"Olvidé mi contraseña"**: usando las facilidades que proporciona Firebase implementar un mecanismo para crear nueva contraseña si se le olvidó al usuario.
- **Gestión de usuarios**: se deben poder dar de alta usuarios, editar el perfil, y dar de baja. Todo esto lo harán los propios usuarios, no es necesario que implementéis funciones de administrador salvo que sea una parte vital de vuestra aplicación. 
- **Listado de recursos**: debe haber alguna pantalla con un listado de los datos básicos de un recurso, por ejemplo en la inicial del sitio de *crowdfunding* aparecen los últimos proyectos dados de alta.
- **Detalles de recurso**: en el listado de recursos, al pulsar sobre uno de ellos se deben poder ver todos sus datos. Este recurso también debe tener subrecursos asociados. Por ejemplo en la *app* de *crowdfunding* al pulsar sobre el título de cada proyecto en el listado se ven todos sus detalles y las noticias asociadas al proyecto.
- **Edición de recurso**: se debe poder editar al menos uno de los recursos que se manejan en la aplicación (por ejemplo en el caso del *crowdfunding* los creadores de un proyecto pueden editar sus detalles).
- **Eliminación de recurso**: se debe poder eliminar al menos uno de los recursos que se manejan en la aplicación

Las operaciones de Listado, detalles, edición y eliminación se entiende que hay que implementarlas en al menos un recurso distinto de los propios Usuarios de la aplicación.

## Parte 2: Modelo de datos (0.5 puntos)

Por suerte o por desgracia al usar Firebase nos vamos a ver más o menos "obligados" a usar las bases de datos de esta plataforma, Firestore o la Realtime Database. Os recomiendo usar Firestore porque es similar a MongoDB, que ya habéis visto en otras asignaturas, pero podéis usar también la otra. En los ejemplos de clase usaremos Firestore. 

> También podríais enlazar Firebase con cualquier base de datos externa, pero todas las soluciones que he visto hacen uso de las *cloud functions* , que si bien son gratuitas hasta cierto nivel de uso requieren darse de alta en la plataforma con tarjeta de crédito.

## Parte 3: Prototipado simple del sitio web (1 punto)

Esto puede parecer un contrasentido, ya que hemos dicho que de momento no va a haber nada de HTML. No obstante lo que pedimos aquí no es una implementación sino un prototipado "de baja fidelidad". 

No necesitamos **mockups**, es decir, representaciones fieles de cómo quedará la interfaz sino **wireframes**, bocetos que simplemente nos permitan saber la disposición de elementos (*layout*) en cada página del sitio. Hay muchas herramientas que permiten hacer *wireframes*, podéis usar la que queráis. En el ejemplo de *crowdfunding* se ha usado [Moqups](https://moqups.com), porque es sencilla de usar y gratuita (hasta 2 proyectos), pero como decimos podéis usar cualquier otra.

La misión del *wireframe* es doble: por un lado planificar cómo será el sitio web de la práctica siguiente y por otro, al hacerlo os podéis dar cuenta de que quizá falta alguna funcionalidad importante para dar soporte a la interfaz de usuario. Por tanto en cada pantalla del sitio deberíais anotar qué casos de uso de la parte 1 se están empleando.

## Parte 4: Desarrollo del *backend* (4 puntos)

El código cliente que en el futuro haga uso de los servicios del *backend* no debería necesitar conocer que este está implementado en Firebase, es decir debéis crear funciones o métodos que actúen como una "capa de servicios" aislando del API de Firebase.

Por ejemplo podéis crear una función `login(email, password)` que acabe llamando al `signInWithEmailAndPassword` de Firebase, o en el caso del *crowdfunding* una función o método `listarProyectosMasPopulares(num)` que devuelva los datos de los `num` proyectos más populares llamando internamente al API de Firestore. Y así con todos los "servicios" proporcionados por el *backend*.

## Requerimientos adicionales

> Tened en cuenta que no os puedo poner más de un 10 en la práctica. No hagáis partes adicionales "de más".

- **Pruebas unitarias (hasta 1 punto):** si implementáis una suite de pruebas unitarias para probar las funcionalidades implementadas. Hay muchas herramientas para pruebas unitarias en Javascript, por ejemplo Mocha o Jest, tendréis que buscar cómo configurarlas y usarlas.
- **CRUD sobre más de un recurso (hasta 1 punto):** en los requisitos mínimos se pide  implementar CRUD sobre al menos un recurso (y listar también los detalles de un subrecurso asociado). Para esta parte adicional se pide implementar CRUD sobre otro recurso adicional.
- **Acceso a un API REST externo (de 0.5-1 puntos):** que algún caso de uso implique acceder a un API REST externo. Por ejemplo si vuestra app fuera de reserva de hoteles os podría interesar mostrar el tiempo que va a hacer en el destino en la fecha elegida. Si el API usa OAuth para autenticación se os puede contar hasta 1 punto, si no hasta 0.5.
- **paginación de resultados (hasta 0.5 puntos):** implementar una búsqueda con paginación, tendréis que mirar en la documentación de Firestore.

Podéis realizar cualquier otra ampliación o mejora que se os ocurra, pero consultadlo antes con el profesor para asegurarse de que sería válida para la evaluación y tener una idea de hasta cuánto se podría valorar en la nota.

## Evaluación de la práctica y normas de entrega

La fecha límite para la entrega será **el lunes 14 de octubre a las 23:59**. La entrega se realizará comprimiendo todos los archivos de vuestro proyecto en un .zip y subiéndolos a moodle (¡¡acordáos de no comprimir el `node_modules`!!).



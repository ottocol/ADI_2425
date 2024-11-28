# Práctica evaluable 4: Aplicaciones web en dispositivos móviles

El objetivo de esta práctica es convertir el *frontend* desarrollado en la práctica anterior a un formato apropiado para ejecutar en un dispositivo móvil. El "producto final" de la práctica debería tener apariencia de aplicación para móvil aunque en realidad estará desarrollado con HTML+CSS+JS.

Para que la app web tenga aspecto de aplicación móvil usaremos [Ionic](https://ionicframework.com/), que proporciona componentes de interfaz de usuario con aspecto similar al nativo en iOS o Android. Ionic se puede usar con apps Vue, Angular, React o JS "vanilla".


## Requisitos mínimos (hasta 7 puntos)

> Aunque gran parte del código de la práctica 3 os va a servir para esta, el proyecto de esta práctica debería ser uno nuevo y distinto de la práctica anterior, para evitar problemas. Lo que cambiará menos será el Javascript y lo que cambiará más será el HTML de los componentes, que ahora usará etiquetas de Ionic.

Cread un nuevo proyecto de Ionic para Vue [como se describe](https://ionicframework.com/docs/vue/quickstart) en la documentación. Hay tres plantillas a elegir, para los requisitos mínimos la mejor es la más sencilla, la `blank`, pero podéis también probar las otras dos para ver el aspecto que tienen. 

**Como requisitos mínimos se pide que en la aplicación se pueda hacer login, logout y se pueda hacer CRUD de un recurso cualquiera** (el que proceda en vuestra aplicación, películas, coches, productos, ...). Eso sí, las "pantallas" de la app deberían estar organizadas adecuadamente para un dispositivo móvil. En Ionic una "pantalla" es una `<ion-page>`.

- Una pantalla inicial solo con el formulario de login
- Una pantalla con una lista (en Ionic, [`<ion-list>`](https://ionicframework.com/docs/api/list) ) en la que en cada fila aparezca un elemento, con botones para editar/borrar, y haya alguna forma de añadir un elemento. Además desde esta pantalla se debería poder hacer logout, lo que redirigiría a la pantalla de login.

Cómo implementéis visualmente las funcionalidades de editar, borrar y añadir queda a vuestra elección. Echad un vistazo a la [documentación sobre los componentes visuales](https://ionicframework.com/docs/components) de Ionic para averiguar cuáles os podrían ser útiles.




## Requisitos adicionales (hasta 3 puntos)
 
- **CRUD de más de un recurso (1 punto)** otra pantalla adicional donde se pueda hacer CRUD de otro recurso, para navegar entre ambas necesitaréis *tabs* o un *sidebar menu*.
- **Desplegar la app como una pwa en Firebase: (1 punto)** En la documentación de Ionic para vue se describe cómo convertir la app web en una PWA (añadiendo automáticamente un manifest y un service worker) y cómo desplegarla en los servidores de Firebase hosting. 
- **Implementar notificaciones push (1 punto)**: Con Firebase Cloud Messaging podéis enviar notificaciones *push* a los dispositivos registrados. Puedes consultar la documentación de Firebase para ver cómo obtener el *token* del dispositivo y enviar un mensaje, es suficiente con que envíes el mensaje desde la consola de Firebase. Para documentar que has realizado esta parte deberías grabar un video de cómo se obtiene el token del dispositivo, se usa en la consola de cloud messaging de Firebase y cómo se muestra la notificación al usuario.
- Cualquier otra funcionalidad relacionada con el funcionamiento de la aplicación en dispositivos móviles, consultad antes con el profesor para ver cómo se podría valorar.


## Entrega

El plazo de entrega concluye el **viernes 10 de enero de 2025 a las 23:59**. La entrega se realizará como es habitual a través de Moodle.

Entregad también un LEEME.txt donde expliquéis brevemente las partes optativas que habéis hecho y cualquier detalle que consideréis necesario.



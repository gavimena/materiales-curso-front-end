# AJAX

## Contenidos

- Usando XMLHttpRequest para pedir datos al servidor
- Usar callbacks con addEventListener sobre readyStateChange.
- Explicar un poco de HTTP, status, get, post y cabeceras?
- El formato JSON. Parsear datos de cadenas a JSON y viceversa.


## Introducción

Las peticiones AJAX son la vía a través de la cuál, tras haber cargado una web, podemos obtener información adicional de un servidor sin necesidad de recargar nuestra página o cargar una nueva. Si pensamos en una web sin AJAX, esta sería una en la que tendríamos que recargar las páginas cada vez que quisiesemos ver nuevos cambios. De esta forma se perdería gran parte de la calidad de experiencia del usuario en muchas páginas con actualizaciones de datos en tiempo real (chats, notificaciones, mapas que se cargan del servidor al movernos entre distintas regiones, etc.)

Durante esta sesión veremos cómo funciona la comunicación entre el navegador y el servidor a la hora de realizar una petición AJAX dentro de nuestra página. Además veremos cómo hacer esa petición y las fases y estados que tiene. El objetivo es que al final de la sesión sepamos como implementar una petición AJAX en nuestro código y la idea de obtener nueva información con JavaScript parezca menos mágica.


## ¿Para qué sirve lo que vamos a ver en esta sesión?

En esta sesión queremos dar a conocer, paso a paso, como se desarrolla la comunicación entre un servidor y el navegador web cuando realizamos una petición AJAX. Esto nos permitirá saber qué está pasando en cada momento y entender mejor el funcionamiento de las webs.

Por otro lado, gracias a las peticiones AJAX seremos capaces de coger información de un servidor y mostrarla en nuestra web o al revés, enviar información de nuestra página a un servidor. Esto nos abrirá la puerta a saber cómo hacer que lo que el usuario haga en la página le permita mandar información al servidor (como datos de un formulario) y que los datos de una página se muestren según los datos que introduzca el usuario. En definitiva todo esto nos permitirá que lo que haga un usuario no se quede en nuestra página sino que vaya mas allá y permita gestionar y obtener información de fuera de la página (un servidor).

La única tarea que desempeña una petición AJAX será esa. El resto será código que combinemos con esa petición. Pongamos por ejemplo un caso de un formulario que rellenamos, enviamos al servidor y en función de la respuesta del servidor mostramos un mensaje en la página (sin recargarla). En ese caso estos serían los pasos:

- Añadir un evento en JavaScript para que al pulsar el botón se obtenga información de los inputs y se guarden en una variable (aquí no hacemos nada con AJAX)
- Tras guardarse se mandarán al servidor usando AJAX y recibiremos una respuesta (que nos dirá si ha podido guardar correctamente la información)
- Mostraremos un mensaje en la pantalla con los datos de la respuesta (usando innerHTML o añadiendo un estilo a un elemento, por ejemplo)

Veremos más adelante que las peticiones AJAX generan una serie de eventos que podemos escuchar, pero con esto ilustramos bien que su uso es solo enviar y recibir información y ejecutar un código en función de la información que se recibe.


## ¿En qué casos se utiliza?

Como hemos comentado, sirven para enviar y obtener información del servidor, por lo que si pensamos en casos de uso, todos ellos estarán relacionados con esa tarea. Por nombrar algunos de los casos más comunes, podríamos mencionar los siguientes:

- Cuando rellenamos un formulario y pulsamos en el botón de enviar y al momento, en la misma página, aparece un mensaje de que la información se ha enviado correctamente
- Cuando subimos archivos a una página como imágenes. Muchas veces se realiza con AJAX. Un ejemplo podría ser cuando arrastramos un archivo a Slack para compartirlo y este se sube automáticamente
- Cuando hacemos una búsqueda en un formulario y automáticamente al pulsar el botón de buscar aparecen en la misma página los resultados, como sucede en la página del buscador de Google
- Cuando navegamos por el mapa de una web y al movernos a una zona esta está en gris y al momento se cargan las imágenes, como sucede en Google Maps
- Cuando tenemos un listado, como podría ser la lista de fotos de Instagram o Pinterest y llegamos al final de la página y automáticamente se cargan las siguientes 20 fotos o el número de fotos que sea

Y podríamos poner mil ejemplos más. En todos ellos se ejecutará la petición AJAX para obtener/enviar información del servidor y en función de esa información realizar una tarea u otra (como mostrar la información).


### Cómo funciona una petición AJAX

Vamos a ver cómo funciona una petición y después veremos cómo realizamos ese proceso en JavaScript, explicándolo paso a paso. Es decir, veremos primero el proceso global y luego asociaremos cada uno de los pasos de ese proceso a lineas de código.

Cuando hacemos una petición AJAX estamos haciendo lo siguiente:


#### 1. Crear la petición y conectar con el servidor

Al principio creamos la petición AJAX y definimos una conexión con el servidor, le decimos cómo nos queremos conectar a este para establecer una conexión. Con esto definimos la petición y cómo se va a hacer. En este paso decimos cómo nos queremos conectar y a qué dirección, por lo tanto, tenemos que especificar el método que vamos a utilizar para enviar la información y la dirección web (URL) a donde queremos enviar esa información.

Hay varias métodos de envio de información a un servidor, pero los más comunes son los métodos GET y POST.

Utilizaremos **GET** (_obtener_ en español) cuando lo que queremos es obtener información del servidor (cargar una lista de artículos para un blog, obtener los resultados de una búsqueda, etc.).

Utilizaremos **POST** cuando queremos enviar información al servidor (enviar información de un formulario, guardar un cambio en un artículo, subir una foto, subir un nuevo artículo, etc.)

Bien con saber esto quedan explicados los métodos que vamos a ver por el momento, existen más pero de momento nos centraremos en estos.

Ahora es el momento de explicar cómo funciona eso de la URL y por qué ponemos una dirección web para enviar datos a un servidor. Bien antes de explicar el por qué de ese caso, es importante explicar un concepto llamado API.

Una API (Aplication Programming Interface) es el conjunto de reglas que definen cómo podemos (y debemos) interactuar con una aplicación. Este concepto es muy vago y muy general, por lo que vamos a explicarlo mejor con ejemplos, pero antes de poner un ejemplo, es fundamental que sepamos qué es una interfaz.

Bien, pensemos en una web con sus campos, botones, enlaces, texto e imágenes, YouTube por ejemplo. La interfaz de una web es gráfica, es decir, podemos verla y la forma en que interactuamos con ella es mediante nuestro ratón (o pantalla táctil) y nuestro teclado, con los que le damos instrucciones, como iniciar o pausar el video, ir a otro video o escribir para buscar un video.

Todo esto, tanto la forma en la que se muestra el contenido, como la forma en que interactuamos con él forman parte de la interfaz. Es decir, la interfaz nos proporciona una conexión que permite el intercambio de información, YouTube me dará información como el título del video, los comentarios o el propio video y yo le daré información, como lo que estoy buscando o a donde quiero ir, y se establecerá una comunicación en la que podremos decirle a la web qué queremos hacer y ella nos responderá. Esto es a grandes rasgos la descripción de una interfaz, pero es una interfaz gráfica y por tanto tiene la ventaja de que podemos tener botones, campos de texto, etc. Pero ¿cómo nos comunicamos con sistemas que no disponen de la capacidad de mostrar gráficos (como un servidor web)? Aquí es donde entra en juego la API. Una API, como decíamos al principio, es el conjunto de reglas que definen cómo podemos interactuar con una aplicación y si pensamos en un servidor, la única forma posible será mediante el envio de datos a una dirección concreta del servidor.

Para tener una idea más clara de cómo funciona una API web, vamos a ver un ejemplo. En este caso vamos a ver la API de GitHub. GitHub nos ofrece un servicio para obtener información de la plataforma, como por ejemplo, info de un usuario (cuantos seguidores tiene, cuantos repos, etc.) o de un repositorio (cuantos commits se han hecho, cuantos contribuidores tiene, etc.). Todo esto lo hace a través de una API web, ofreciendo una serie de URLs para que, en función de la información que queramos visitemos una u otra y nos devuelva los datos que estamos buscando. Para saber cuales son las rutas disponibles podéis visitar [la web de su API](https://api.github.com/).

Como podéis ver tiene un texto con distintas URLs metidas en un JSON, que sería algo parecido a un objeto de JavaScript y que explicaremos más adelante en esta sesión. Si copiamos alguna de estas URLs y la pegamos en la barra de direcciones de nuestro navegador, por ejemplo la de emojis, veremos que nos devuelve una nueva URL con nueva información también en formato JSON. Además podemos ver que algunas URLs tienen texto entre llaves, esto es porque GitHub te dice que tienes que sustituir eso por un texto válido. Una de las URLs raras es la que va asociada a `repository_url`, esta tiene `{owner}` y `{repo}` y tendremos que sustituirlo por un texto donde owner será el dueño del repositorio y repo será el nombre del repositorio.

![Foto de la api de repos]()

Cuando vamos a un repositorio en GitHub vemos que en la URL nos aparece el nombre del dueño y luego el nombre del repo.

![Foto del repo]()

Por lo tanto vamos a probar a poner Adalab en el owner y soluciones-ejercicios-front en repo a ver qué sucede.

Como se puede ver, nos devuelve datos sobre ese repositorio. ¡Genial! nos hemos comunicado con la API y con la información que le hemos pasado nos ha devuelto una serie de datos.

Pensemos que todo este proceso no lo vamos a hacer nosotros a mano metiendo URLs en nuestro navegador sino que lo haremos a través de JavaScript. De ahí que los datos sean sin formato, texto puro sin estilos y de ahí también que la comunicación sea a través de URLs en vez de botones ya que JavaScript no puede visualizar botones y saber cuál pulsar en cada momento.

Al igual que sucedía en el ejemplo de YouTube, en una API web tenemos una interfaz, pero en este caso no es gráfica (no posee botones ni campos). Aun así, podemos darle instrucciones para obtener nuevos datos (como URLs de imágenes, texto, etc.), podemos interactuar con ella. Al fin y al cabo obtenemos podemos hacer cosas similares a las que hacemos con una web solo que cambia la forma en la que interactuamos.

Por lo tanto, para resumir, una API será el conjunto de reglas y procedimientos que describen cómo podemos interactuar con una aplicación. En el caso de una API web, estos definirán las rutas a las que debemos acceder (llamadas endpoints) y los datos que podemos pasarles (como hemos visto con `{owner}` y `repo`).

Existen otro tipo de APIs, como por ejemplo de código, pero vienen a ser lo mismo interfaces con las que actuaremos para obtener los resultados deseados, la única diferencia entre cada una de ellas es cómo interactuamos con ellas. En el caso de las APIs web, interactuamos a través de URLs, en el caso de las de programación interactuaremos cargando un script con el código creado por los creadores de la API y usando las funciones, métodos y objetos que sean necesarios para obtener los resultados.

Normalmente las APIs web suelen estar documentadas. Aquí podéis ver algunos ejemplos de documentación de APIs.

Al final cada empresa hace la documentación de su API de una forma distinta y es dificil explorar cómo funciona cada una, pero en las APIs web todas funcionan a través de APIs y según los datos que le pases en la petición

Bien, ahora que hemos explicado qué es una API y los métodos. Podemos continuar con qué sucede despues de conectar con un servidor.

#### 2. Enviar la petición

- A veces esa petición puede tener contenido y otras veces no.

#### 2.A No podemos recibir una respuesta del servidor

Hemos enviado nuestra información al servidor pero no logramos recibir una respuesta, ya sea porque no tenemos conexión a internet, porque no se encuentra el servidor o por lo que ha habido cualquier otro fallo de red.

Cuando no podemos recibir una respuesta del servidor, nuestra petición AJAX emite un evento `'error'` al igual que se emite un evento `'click'` cuando pulsamos sobre un botón. Como veremos más adelante podemos ejecutar una función cuando suceda eso, lo normal es que esa función muestre al usuario de la página que ha surgido un error, por ejemplo, mostrando un pequeño mensaje en la web.

![Mensaje de error]()

Es importante que no ignoremos este caso porque se puede llegar a producir y si no hacemos nada cuando suceda el usuario no sabrá que está pasando o no podremos encontrar una vía alternativa a través del código para conseguir que el usuario obtenga los resultados que desea.

#### 2.B Recibimos una respuesta del servidor

Cuando nuestro código llega al servidor, este lo procesa y devuelve una respuesta. Si recibimos esa respuesta correctamente en nuestro navegador, podremos trabajar con ella y realizar un código u otro en función de su contenido.

En este caso, al igual que en el caso de que suceda un error, mediante JavaScript podemos ejecutar una función cuando se haya terminado de recibir la respuesta. Como norma general, esa respuesta suele venir con otra información adicional que nos sirve para saber más acerca de ella. Uno de los datos fundamentales que acompañan a la respuesta es el código de estado de esta o _status code_. Este código será muy importante para nosotros porque nos indicará, sin ver el contenido de la respuesta, si hemos recibido una respuesta válida o hay algún error en ella.

¿Pero y cómo enviamos los datos y los recibimos?

- Cuando hacemos una petición o recibimos una, no hacemos otra cosa que enviar un recurso (archivo) al servidor o recibirlo. Una petición sería algo como lo siguiente. Un archivo de texto Que contiene lo siguiente. Es como cuando pedimos información de un archivo HTML. Al final es un archivo con texto plano.

El código de estado es un código numérico comprendido entre el 100 y el 599 y sirve para representar mediante un número cual es el estado de la respuesta. Todos hemos visto alguna vez una página en la que aparece el típico mensaje "Error 404", ese 404 es un ejemplo de un código de estado devuelto por un servidor y lo que indica es que la ruta solicitada (URL) no contiene ningún recurso (página web, imagen, archivo CSS o JavaScript o lo que sea). Obviamente no vamos a ver todos los códigos pero si que podemos saber que tipo de error es dependiendo del rango en el que se encuentre:

- **1XX** (Ej: 100, 101, 102...) indican una respuesta informativa
- **2XX** (Ej: 200, 204, 226...) indican éxito en la respuesta. Por ejemplo que la respuesta se ha recibido correctamente (200) o que se ha creado correctamente una nueva entrada en nuestro servidor (201)
- **3XX** (Ej: 300, 301, 304...) indican una redirección para indicar que el recurso se ha movido a otra ruta y se está redirigiendo a ella o que el recurso no se ha modificado desde la última vez que se obtuvo y está cacheado.
- **4XX** (Ej: 403, 404, 400) indican que ha habido un error por parte del cliente (por nuestra parte). Estos errores suelen ser debidos a que hemos escrito mal la URL, que la dirección a la que queremos acceder no está relacionada con ninguna página web o recurso, que no tenemos acceso a la web que estamos intentando solicitar y errores del mismo estilo
- **5XX** (Ej: 503, 500, 501) indican que ha habido un error por parte del servidor. Estos errores suelen ser debidos a un fallo producido en el servidor, como por ejemplo cuando se cae un servidor. Muchas veces habréis visto mensajes de error 503, estos se deben a que el servidor se ha caido, por ejemplo.

Por lo tanto, el proceso sería el siguiente. En primer lugar, recibimos la respuesta del servidor en nuestro navegador.

Una vez recibida, con JavaScript, comprobamos cuál es el código del estado de la respuesta para ver que hacer:

- Si la respuesta tiene un _status code_ entre 200 y 399, es que la respuesta está bien y ejecutaremos un código con el contenido de esta
- Si recibimos un _status code_ entre el 400 y el 499, es que la respuesta está mal y el fallo está en nuestro código de JavaScript (la URL está mal, no tenemos los permisos necesarios para solicitar información, etc.)
- Si recibimos un _status code_ entre el 500 y el 599 es que se ha producido un error en el servidor o está caido.

Al igual que ejecutabamos un código cuando la petición emitia un evento `'error'`, podemos ejecutar un código cuando la petición emita un evento `'load'` que indicará que ha recibido nueva información del servidor.

Por tanto en este evento ejecutaremos un código y dentro de ese código, en función del _status code_ que recibamos, haremos una cosa u otra.

Al igual que podemos enviar cualquier tipo de recurso al servidor, como vimos en el paso del envio de la petición, podemos recibir cualquier tipo de recurso. Podríamos recibir una imagen, un código HTML, un código CSS. Este código siempre lo recibiremos en formato string, es decir, como texto.

Aun así se suele utilizar dos formatos normalmente. XML y JSON.

XML es un lenguaje parecido a HTML

JSON sería similar a un valor de JavaScript (JavaScript object notation) (objeto, array, booleano) con algunas diferencias. Solo se pueden usar comillas

Estos valores nos vendrán como si fuese texto normal. Veremos cómo convertirlo a texto un poco más adelante.


## Trabajando con AJAX

GitHub, conseguir las estrellas

- Cuando pulsas en un botón se envia si queremos estrellas con un dropdown

Vamos a crear nuestro HTML.

Tendremos que saber qué api queremos y como funciona.

- Bien lo primero es establecer una conexión, para eso creamos una nueva petición. En JavaScript para crear una nueva petición, utilizaremos el siguiente código.

```js
new XMLHttpRequest();
```

Esto crea un nuevo objeto para la petición con sus métodos y propiedades. Veremos en el sprint 3 para que sirve eso del `new` por el momento digamos que sirve para crear algo nuevo, en este caso esa petición. Como con cualquier objeto, lo asignaremos a una variable para poder modificarlo despues o ejecutar sus métodos

```js
var request = new XMLHttpRequest();
```

Bien, tenemos la petición, pero antes de enviar, lo normal suele ser definir que queremos hacer cuando recibamos una respuesta del servidor. Para definir qué queremos hacer con la respuesta del servidor, crearemos una función que se ejecute cuando se produzca el evento `load` de la petición:

```js
var request = new XMLHttpRequest();

function manageResponse() {
  // Aquí el código
}

request.addEventListener('load', )
```

Como hemos comentado, cuando recibimos una respuesta del servidor, esta puede ser de tres tipos. Para comprobar de qué tipo y ejecutar un código u otro en función de este haremos lo siguiente.

En nuestro caso, añadiremos un estilo distinto para poner el código en verde, otro en amarillo y otro en rojo.

Para comprobar cual es el estado de la información obtenida haremos lo siguiente.

```js
var request = new XMLHttpRequest();

function manageResponse() { // <---
  // Aquí el código
}

request.addEventListener('load', ) // <---
```

Tras eso, abrimos una nueva conexión entre la página y el servidor del cual queremos obtener o enviar información. En este paso solo estableceremos la conexión, no enviaremos nada de información sin que se establecerá el canal para posteriormente, en otro paso, enviar el mensaje (los datos).

```js
var request = new XMLHttpRequest();

function manageResponse() {
  // Aquí el código
}

request.addEventListener('load', )


request.open('GET', 'https://thecatapi.com/api/images/get?format=html'); // <---
```

Por último, enviaremos la petición. Esto lo haremos con el el método `send()`.

```js
var request = new XMLHttpRequest();

function manageResponse() {
  // Aquí el código
}
request.addEventListener('load', )
request.open('GET', 'https://thecatapi.com/api/images/get?format=html');
request.send();  // <---
```

Y de esta forma es como utilizamos una petición AJAX. Pero imaginemos que queremos cambiar las estrellas por seguidores para saber cual

Vamos a probar qué pasaría si ponemos una dirección que no existe.

Crea un código para que si la dirección no existe cambie el color a rojo

Convertir un JSON a un objeto de JavaScript

JSON es texto sin más mientras que en JavaScript usamos un objeto. Habrá que convertirlo

## Conceptos clave a la hora de trabajar con AJAX

Las peticiones son asíncronas



Ejercicio del buscador de GitHub

## Recursos externos

- [Curso JavaScript desde 0. Ajax I. Vídeo 68](https://www.youtube.com/watch?v=7w5PpdHKndc)


- Ya hemos cargado la página y la mostramos pero queremos obtener información adicional del servidor para añadirla a nuestra página, normalmente porque el usuario ha introducido un dato o ha realizado una interacción. Sin recargar la página añadimos información en ella


- intentar hacer una app que coja las últimas imágenes de instagram de utopic us

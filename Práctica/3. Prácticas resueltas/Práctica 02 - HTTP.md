<h1 align="center">Práctica 2 - HTTP</h1>

### 1. ¿Cuál es la función de la capa de aplicación?

La función de la capa de aplicación es proveer servicios de comunicación a usuarios o aplicaciones de forma amigable, via interfaces gráficas.

### 2. Si dos procesos deben comunicarse:

##### a. ¿Cómo podrían hacerlo si están en diferentes máquinas?

Si están en diferentes máquinas pueden comunicarse vía alguna red, siempre y cuando ambas máquinas usen los mismos protocolos.

##### b. Y si están en la misma máquina, ¿qué alternativas existen?

Si están en la misma máquina pueden comunicarse via el IPC del sistema operativo.

### 3. Explique brevemente cómo es el modelo Cliente/Servidor. Dé un ejemplo de un sistema Cliente/Servidor en la “vida cotidiana” y un ejemplo de un sistema informático que siga el modelo Cliente/Servidor. ¿Conoce algún otro modelo de comunicación?

En el modelo Cliente/Servidor se comparte el procesamiento. El cliente procesa la interfaz de usuario y realiza pedidos y el servidor procesa éstos y devuelve respuestas al cliente.

Un ejemplo cotidiano de este modelo sería el delivery de comida, donde los clientes realizan pedidos a restaurantes y el restaurante los envía.

Un ejemplo informático sería la navegación web en general. El cliente sería nuestro navegador (Chrome, Firefox, etc) y el servidor es el servidor web que queremos acceder (google.com por ejemplo)

Otros modelos incluyen el Mainframe, P2P y el P2P híbrido.

### 4. Describa la funcionalidad de la entidad genérica “Agente de usuario” o “User agent”.

El agente de usuario es una interfaz entre el usuario y la aplicación de red. Por ejemplo, el agente para acceder a google.com sería Firefox o Google Chrome, es decir nuestro browser.

### 5. ¿Qué son y en qué se diferencian HTML y HTTP?

HTML es un **lenguaje** de markup usado para estructurar páginas web, mientras que HTTP es un **protocolo** para comunicar al cliente con el servidor y viceversa.

### 6. HTTP tiene definido un formato de mensaje para los requerimientos y las respuestas. (Ayuda: apartado “Formato de mensaje HTTP”, Kurose).

##### a. ¿Qué información de la capa de aplicación nos indica si un mensaje es de requerimiento o de respuesta para HTTP? ¿Cómo está compuesta dicha información?¿Para qué sirven las cabeceras?

La forma más fácil de determinar si un mensaje HTTP es un requerimiento es que la primer linea del mensaje HTTP incluye el método que se utilizó en el request. Si es una respuesta, la primer linea incluye el código de respuesta (1XX, 2XX, 3XX, 4XX, 5XX).

La información de capa de aplicación que nos indica si un mensaje HTTP es un requerimiento o una respuesta es la primer línea de ese mismo mensaje.
Si es un **requerimiento**, la primer linea posee el formato:

```
MétodoHTTP Recurso HTTP/Versión

por ejemplo

GET /hola/test.html HTTP/1.1
```

Si es una **respuesta**, la primer linea posee el formato:

```
HTTP/Versión CódigoDeRespuesta

por ejemplo

HTTP/1.1 200 OK
```

Las cabeceras sirven para que el servidor pueda enviarle datos de forma semántica al cliente, y viceversa.

##### b. ¿Cuál es su formato? (Ayuda: https://developer.mozilla.org/es/docs/Web/HTTP/Headers)

El formato de los headers es clave:valor.

##### c. Suponga que desea enviar un requerimiento con la versión de HTTP 1.1 desde curl/7.74.0 a un sitio de ejemplo como www.misitio.com para obtener el recurso /index.html. En base a lo indicado, ¿qué información debería enviarse mediante encabezados? Indique cómo quedaría el requerimiento.

Debería enviarse el Host y la versión de curl utilizada vía dos headers. El requerimiento completo sería:

```
GET /index.html HTTP/1.1
Host: www.misitio.com
User-agent: curl/7.74.0
```

### 7. Utilizando la VM, abra una terminal e investigue sobre el comando curl. Analice para qué sirven los siguientes parámetros (-I, -H, -X, -s).

El comando curl transfiere datos desde o hacia un servidor usando algún protocolo (HTTP, SMTP, TELNET, etc).

-   El parámetro -I realiza un HEAD request, es decir que solo muestra los encabezados que se recibieron, pero no la página en sí.
-   El parámetro -H permite agregar headers personalizados en el request.
-   El parámetro -X permite especificar el método HTTP que queremos en nuestro request. Por defecto se utiliza GET.
-   El parámetro -s silencia el progreso y no muestra mensajes de error si los hubiera.

### 8. Ejecute el comando curl sin ningún parámetro adicional y acceda a www.redes.unlp.edu.ar. Luego responda:

##### a. ¿Cuántos requerimientos realizó y qué recibió? Pruebe redirigiendo la salida (>) del comando curl a un archivo con extensión html y abrirlo con un navegador.

Realicé un requerimiento GET y recibí un código HTML. Al abrir ese código html con el navegador vemos la página pero sin estilos.

##### b. ¿Cómo funcionan los atributos href de los tags link e img en html?

Los atributos href contienen URLs y al clickearlos realizan un requerimiento HTTP a esa URL.

##### c. Para visualizar la página completa con imágenes como en un navegador, ¿alcanza con realizar un único requerimiento?

No, no alcanza, ya que se hace un requerimiento HTTP por cada tag **link, src y img**. En este ejemplo deberían realizarse 8 requerimientos en total para visualizar la página en su totalidad, con imágenes, estilos, etc.

##### d. ¿Cuántos requerimientos serían necesarios para obtener una página que tiene dos CSS, dos Javascript y tres imágenes? Diferencie cómo funcionaría un navegador respecto al comando curl ejecutado previamente.

Se necesitarían 1 (la página en sí) + 2 (CSS) + 2 (JS) + 3 (imágenes) = 8 requerimientos.

La diferencia entre el navegador y curl es que curl solo obtiene el html en sí, no realiza todos los requerimientos anterior mencionados, si no que uno solo, a no ser que se le especifique realizar más requerimientos. El navegador realiza todos los requerimientos.

### 9. Ejecute a continuación los siguientes comandos:

```
curl -v -s www.redes.unlp.edu.ar > /dev/null
curl -I -v -s www.redes.unlp.edu.ar
```

##### a. ¿Qué diferencias nota entre cada uno?

El primer comando realiza un GET request ya que no se especifica otro método. Este GET request obtiene el HTML, pero no lo muestra en la salida del comando ya que lo redirecciona al archivo /dev/null.

El segundo comando realiza un HEAD request ya que se especifica el parámetro -I. Este HEAD request no obtiene el html, solo los headers de la respuesta.

##### b. ¿Qué ocurre si en el primer comando se quita la redirección a /dev/null? ¿Por qué no es necesaria en el segundo comando?

Si en el primer comando se quita la redirección entonces se muestra el código HTML en la salida del comando, en la terminal.

La redirección no es necesaria en el segundo comando porque no hay nada que redireccionar, el código HTML no se obtuvo por haber hecho un HEAD request.

##### c. ¿Cuántas cabeceras viajaron en el requerimiento? ¿Y en la respuesta?

En el requerimiento viajaron 3 headers, y en la respuesta viajaron 7.

### 10. ¿Qué indica la cabecera Date?

La cabecera Date indica la fecha y hora exacta en la que el servidor envió la respuesta.

### 11. En HTTP/1.0, ¿cómo sabe el cliente que ya recibió todo el objeto solicitado de manera completa? ¿Y en HTTP/1.1?

En HTTP/1.0 el cliente sabe que ya recibió todo el objeto solicitado vía el header Content-Length el cual especifica la longitud de la respuesta que el servidor envió. El problema es que para esto el cliente debe saber de antemano qué tan grande debe ser la respuesta, para poder comparar. **En caso de no saberlo, el cliente sabrá que ya recibió todo el objeto cuando se percate de que el servidor cerró la conexión.**

En HTTP/1.1 el cliente sabe que ya recibió todo el objeto gracias a una nueva técnica llamada Chunked Transfer Encoding que le permite al servidor enviar pedazos de la respuesta uno a uno. **La transmisión de la respuesta termina con un chunk de longitud cero.**

### 12. Investigue los distintos tipos de códigos de retorno de un servidor web y su significado. Considere que los mismos se clasifican en categorías (2XX, 3XX, 4XX, 5XX).

| Código | Significado                                                                                                  |
| ------ | ------------------------------------------------------------------------------------------------------------ |
| 1XX    | Informativos, indican que el servidor recibió el request y lo está procesando.                               |
| 2XX    | Éxito, indican que el request se recibió, se entendió y se procesó.                                          |
| 3XX    | Redirección, indican que el recurso solicitado se ha movido a otro lugar.                                    |
| 4XX    | Error del CLIENTE, indican que el request no es válido.                                                      |
| 5XX    | Error del SERVIDOR, indican que algo malo ocurrió en el server (se encuentra fuera de servicio por ejemplo). |

### 13. Utilizando curl, realice un requerimiento con el método HEAD al sitio www.redes.unlp.edu.ar e indique:

##### a. ¿Qué información brinda la primera línea de la respuesta?

La primera línea indica la versión de HTTP del servidor y el código de retorno:

```
HTTP/1.1 200 OK
```

##### b. ¿Cuántos encabezados muestra la respuesta?

Se muestran 7 encabezados.

##### c. ¿Qué servidor web está sirviendo la página?

El servidor web que está sirviendo la página se indica en el header Server: "". En este caso se está usando el servidor Apache/2.4.56 (Unix).

##### d. ¿El acceso a la página solicitada fue exitoso o no?

Si, fue exitoso, ya que se recibió un código de éxito 200.

##### e. ¿Cuándo fue la última vez que se modificó la página?

La última vez que se modificó la página se indica en el header Last-Modified: "". En este caso se modificó por última vez el domingo 19/3/2023 a las 19:04:46 GMT.

##### f. Solicite la página nuevamente con curl usando GET, pero esta vez indique que quiere obtenerla sólo si la misma fue modificada en una fecha posterior a la que efectivamente fue modificada. ¿Cómo lo hace? ¿Qué resultado obtuvo? ¿Puede explicar para qué sirve?

Podemos lograr esto usando el parámetro -H y agregando un header If-Modified-Since:

```
curl -H "If-Modified-Since: Sun, 19 Mar 2023 20:04:46 GMT" www.redes.unlp.edu.ar
```

Como la fecha y hora que indicamos es 1 hora después de la fecha y hora exacta donde se modificó la pagina por última vez, la condición es falsa y por ende curl no muestra respuesta alguna. Es importante indicar la fecha y hora en el formato **exacto** que acepta el header mencionado.

### 14. Utilizando curl, acceda al sitio www.redes.unlp.edu.ar/restringido/index.php y siga las instrucciones y las pistas que vaya recibiendo hasta obtener la respuesta final. Será de utilidad para resolver este ejercicio poder analizar tanto el contenido de cada página como los encabezados.

1. El comando inicial nos retorna un código html que indica que necesitamos autenticarnos. Para esto nos indica dirigirnos a www.redes.unlp.edu.ar/obtener-usuario.php.

2. La URL indicada requiere que enviemos un header "Usuario-Redes: obtener" para poder obtener nuestro usuario y contraseña.

3. Entonces hacemos curl -H "Usuario-Redes: obtener" www.redes.unlp.edu.ar/obtener-usuario.php

4. Obtenemos los datos usuario: redes | contraseña: RYC.

5. Ahora hacemos curl -u "redes:RYC" www.redes.unlp.edu.ar/restringido/index.php que nos permite autenticarnos.

6. Recibimos un 302 de redirección indicando que nuestra autenticación fue exitosa, pero que tenemos que dirigirnos a otra URL ahora: www.redes.unlp.edu.ar/restringido/the-end.php

7. Allí recibimos un 200 de éxito y un código largo (base64?) además de felicitaciones y la fecha y hora actual.

### 15. Utilizando la VM, realice las siguientes pruebas:

##### a. Ejecute el comando ’curl www.redes.unlp.edu.ar/extras/prueba-http-1-0.txt’ y copie la salida completa (incluyendo los dos saltos de línea del final).

Se recibe la siguiente respuesta:

```
GET /http/HTTP-1.1/ HTTP/1.0
User-Agent: curl/7.38.0
Host: www.redes.unlp.edu.ar
Accept: */*


```

##### b. Desde la consola ejecute el comando telnet www.redes.unlp.edu.ar 80 y luego pegue el contenido que tiene almacenado en el portapapeles. ¿Qué ocurre luego de hacerlo?

Luego de hacerlo se realiza el request y se recibe la respuesta (código 400).

##### c. Repita el proceso anterior, pero copiando la salida del recurso /extras/prueba-http-1-1.txt. Verifique que debería poder pegar varias veces el mismo contenido sin tener que ejecutar el comando telnet nuevamente.

### 16. En base a lo obtenido en el ejercicio anterior, responda:

##### a. ¿Qué está haciendo al ejecutar el comando telnet?

El comando telnet inicia una conexión TCP con la URL indicada en el puerto 80. Una vez establecida se puede realizar un HTTP request.

##### b. ¿Qué método HTTP utilizó? ¿Qué recurso solicitó?

Usé GET y solicité un código HTML.

##### c. ¿Qué diferencias notó entre los dos casos? ¿Puede explicar por qué?

Con HTTP 1.0 una vez que se obtuvo la respuesta la conexión finalizó. En cambio, en 1.1 podemos seguir haciendo requests en esa misma conexión hasta que se cierre. Esto se debe a que HTTP 1.1 posee conexiones persistentes.

##### d. ¿Cuál de los dos casos le parece más eficiente? Piense en el ejercicio donde analizó la cantidad de requerimientos necesarios para obtener una página con estilos, javascripts e imágenes. El caso elegido, ¿puede traer asociado algún problema?

1.1 es más eficiente. En el ejercicio 8)d) se hubieran realizado la misma cantidad de requests pero en una sola conexión TCP en vez de 7 distintas, ahorrando recursos.

### 17. En el siguiente ejercicio veremos la diferencia entre los métodos POST y GET. Para ello, será necesario utilizar la VM y la herramienta Wireshark. Antes de iniciar considere:

##### Capture los paquetes utilizando la interfaz con IP 172.28.0.1. (Menú “Capture → Options”. Luego seleccione la interfaz correspondiente y presione Start).

##### Para que el analizador de red sólo nos muestre los mensajes del protocolo http introduciremos la cadena ‘http’ (sin las comillas) en la ventana de especificación de filtros de visualización (display-filter). Si no hiciéramos esto veríamos todo el tráfico que es capaz de capturar nuestra placa de red. De los paquetes que son capturados, aquel que esté seleccionado será mostrado en forma detallada en la sección que está justo debajo. Como sólo estamos interesados en http ocultaremos toda la información que no es relevante para esta práctica (Información de trama, Ethernet, IP y TCP). Desplegar la información correspondiente al protocolo HTTP bajo la leyenda “Hypertext Transfer Protocol”

##### Para borrar la cache del navegador, deberá ir al menú “Herramientas → Borrar historial reciente”. Alternativamente puede utilizar Ctrl+F5 en el navegador para forzar la petición HTTP evitando el uso de caché del navegador.

##### En caso de querer ver de forma simplificada el contenido de una comunicación http, utilice el botón derecho sobre un paquete HTTP perteneciente al flujo capturado y seleccione la opción Follow TCP Stream.

##### a. Abra un navegador e ingrese a la URL: www.redes.unlp.edu.ar e ingrese al link en la sección “Capa de Aplicación” llamado “Métodos HTTP”. En la página mostrada se visualizan dos nuevos links llamados: Método GET y Método POST. Ambos muestran un formulario como el siguiente:

![Formulario](https://i.imgur.com/IHJHuvj.png)

##### b. Analice el código HTML.

##### c. Utilizando el analizador de paquetes Wireshark capture los paquetes enviados y recibidos al presionar el botón Enviar.

##### d. ¿Qué diferencias detectó en los mensajes enviados por el cliente?

La diferencia principal es que el GET envía los datos ingresados en el formulario a través de la URL vía querys (?clave=valor) mientras que POST envía los datos ingresados en el Body del request, por ende es más seguro.

Otras diferencias:

-   Los POST requests no se cachean, los GET pueden cachearse.
-   Los POST request no permanecen en el historial del browser, los GET si.
-   Los POST requests no tienen límites de longitud en cuanto a los datos a enviar, los GET si, ya que las URLs (donde los query parameters se colocan) tienen límites de longitud.

##### e. ¿Observó alguna diferencia en el browser si se utiliza un mensaje u otro?

No, ninguna.

### 18. Investigue cuál es el principal uso que se le da a las cabeceras Set-Cookie y Cookie en HTTP y qué relación tienen con el funcionamiento del protocolo HTTP.

Las cookies contienen datos que identifican y rastrean la sesión del usuario lo que permite mantener coherencia y estado entre las distintas solicitudes HTTP, permitiendo personalizar la experiencia del usuario.

Set-Cookie se usa cuando el servidor le envía una cookie al cliente, y Cookie al revés.

Se utilizan para simular tener estado, ya que HTTP es un protocolo sin estado.

### 19. ¿Cuál es la diferencia entre un protocolo binario y uno basado en texto? ¿De qué tipo de protocolo se trata HTTP/1.0, HTTP/1.1 y HTTP/2?

Un protocolo binario es uno que usa representaciones binarias para enviar datos lo cual dificulta la lectura y debugging, pero es muy eficiente.

Uno de texto utiliza ASCII, por ende es legible a costo de performance.

**1.0: Texto**
**1.1: Texto**
**2.0: Binario**

### 20. Responder las siguientes preguntas:

##### a. ¿Qué función cumple la cabecera Host en HTTP 1.1? ¿Existía en HTTP 1.0?¿Qué sucede en HTTP/2? (Ayuda: https://undertow.io/blog/2015/04/27/An-in-depth-overview-of-HTTP2.html para HTTP/2)

En HTTP 1.1 la cabecera Host se utiliza para indicar al servidor web el nombre de dominio al que se le quiere realizar un request. Esto es porque a partir de esta versión, una dirección IP de un servidor puede alojar más de un dominio, y si no utilizamos la cabecera Host se producirían ambiguedades.

Este header no existía en HTTP 1.0 ya que en ese entonces cada dominio necesitaba tener su propia IP única.

En HTTP 2.0 también se utiliza la cabecera Host, pero además se puede utilizar la cabecera Authority que es similar pero más potente.

##### b. ¿Cómo quedaría en HTTP/2 el siguiente pedido realizado en HTTP/1.1 si se está usando https?

```
GET /index.php HTTP/1.1
Host: www.info.unlp.edu.ar
```

Quedaría de la siguiente manera:

```
:method: GET
:path: /index.php
:scheme: https
:authority: www.info.unlp.edu.ar
```

### 21. Ejercicio de parcial

```
curl -X ?? www.redes.unlp.edu.ar/??
> HEAD /metodos/ HTTP/??
> Host: www.redes.unlp.edu.ar
> User-Agent: curl/7.54.0

< HTTP/?? 200 OK
< Server: nginx/1.4.6 (Ubuntu)
< Date: Wed, 31 Jan 2018 22:22:22 GMT
< Last-Modified: Sat, 20 Jan 2018 13:02:41 GMT
< Content-Type: text/html; charset=UTF-8
< Connection: close
```

##### a. ¿Qué versión de HTTP podría estar utilizando el servidor?

No es HTTP 1.0, ya que la cabecera Host no existía en esta versión.

No es HTTP 2.0, ya que la sintaxis es distinta.

Por lo tanto, es HTTP 1.1.

##### b. ¿Qué método está utilizando? Dicho método, ¿retorna el recurso completo solicitado?

Se está utilizando el método HEAD. Este método no retorna el recurso completo solicitado, solo los headers de la respuesta.

##### c. ¿Cuál es el recurso solicitado?

El recurso solicitado es /metodos/HTTP/1.1

##### d. ¿El método funcionó correctamente?

Si, funcionó correctamente ya que se recibió un HTTP code de 200, es decir, código de éxito.

##### e. Si la solicitud hubiera llevado un encabezado que diga:

```
If-Modified-Since: Sat, 20 Jan 2018 13:02:41 GMT
```

##### ¿Cuál habría sido la respuesta del servidor web? ¿Qué habría hecho el navegador en este caso?

En ese caso, como la fecha es exactamente igual al If, la condición es falsa, es decir que no se modificó el recurso en una fecha y hora posterior a la indicada en el If.

La respuesta hubiera sido un código HTTP 304 Not Modified.

<center>

# Clase 1 - 12 de marzo, 2024

</center>

## Red

-   Una red es un grupo de computadoras o dispositivos interconectados con el objetivo de compartir recursos o información.
-   Emisor -> Medio/Dispositivos intermedios -> Receptor
-   Algunos de los componentes de una red son:

1. Computadoras o hosts
2. Routers, Switches, Gateways, Access Points
3. NICs (placas de red), Modems
4. Enlaces (cables, fibras ópticas, antenas, etc)
5. Programas (Servidores Web, Clientes de Mail, etc)

## Clasificación de redes

#### Por cobertura

Se refiere a la distancia o alcance de la red.

-   LAN: Red de área local. Ejemplos: Ethernet, WiFi.
-   MAN: Red de una ciudad. Ejemplos: MetroEthernet, MPLS.
-   WAN: Red de amplio alcance. Ejemplos: PPP, Frame-Relay, HDLC.
-   SAN: Red de almacenamiento. Ejemplos: ESCON.
-   PAN: Red personal. Ejemplos: Bluetooth, USB.

#### Por acceso

-   Privadas: Red exclusiva a una organización o individuo.
-   Públicas: Red abierta a cualquier persona. Suelen no ser seguras.

#### Por tipo de conmutación

-   De conmutación de circuitos.
-   De conmutación de paquetes.

## Protocolo

-   Serie de normas o reglas que se tienen que respetar y cumplir para que los distintos componentes se comuniquen correctamente.
-   Tienen como objetivo comunicar billones y billones de dispositivos distintos entre sí que podrían estar usando sistemas operativos distintos, etc, sin problema.

## Arquitectura en capas

-   Como las redes tienen demasiados componentes se requiere algún tipo de organización de estos.
-   El modelo de organización más usado es el **modelo por capas**.
-   Una capa ofrece servicios a la capa de arriba y usa servicios de la capa de abajo mediante interfaces.
-   Existen 2 modelos por capas principales, el OSI y TCP/IP.
-   El modelo OSI (Open System Interconnection) es un modelo teórico y define 7 capas:
    1. Aplicación
    2. Presentación
    3. Sesión
    4. Transporte
    5. Red
    6. Enlace
    7. Física
-   El modelo TCP/IP es el estándar global y define 5 capas:
    1. Aplicación
    2. Transporte
    3. Red
    4. Enlace
    5. Física
       Por simplicidad las capas 4 y 5 suelen agruparse en una sola capa llamada "Enlace" o "De Acceso a la Red"

| Comparación    | TCP                                                 | OSI                     |
| -------------- | --------------------------------------------------- | ----------------------- |
| Capas          | Se divide en 5 capas (4 en la versión simplificada) | Se divide en 7 capas    |
| Tecnología     | Conmutación de paquetes                             | Conmutación de paquetes |
| Complejidad    | Más simple                                          | Más complejo            |
| Tipo de modelo | Práctico                                            | Teórico                 |

-   Cada capa define su Protocol Data unit o PDU, en el caso del modelo TCP/IP las capas son (respectivamente): el Dato, el Segmento, el Paquete, y la Trama. Es decir, el dato se "mete" adentro del segmento, el segmento adentro del paquete, etc, hasta llegar a la última capa donde la Trama se traduce a binario.
-   Cada capa se comunica de forma **indirecta** con la misma capa del otro lado de la comunicación (receptor), ya que cada capa solo "entiende" el contenido de esa misma capa.

## Internet

Es una red de redes de computadoras, descentralizada y pública que utiliza el modelo (o stack de protocolos) TCP/IP.

Cuando decimos computadoras, incluimos a las PCs, Mainframes, autos, heladeras, laptops, celulares, etc.

También utiliza routers y switches, y puede usar fibra óptica, wireless, cobre, satélites, etc.

#### Estructura

-   Se organiza de forma jerárquica
-   Las tecnologías de fibra óptica, cobre o satélites se interconectan mediante POPs (Point Of Presence) con proveedores.
-   Entre proveedores se interconectan mediante NAPs (Network Access Point) o conexiones directas.
-   Actualmente los NAPs se los llama IXPs (Internet Exchange Point)

## RFC

-   Documentación oficial que explica como funcionan los protocolos de TCP/IP.
-   Hay muchas categorías de RFC:

1. Standard Track: los RFC principales que se discuten seriamente.
2. Off Track: ideas experimentales o menos serias.

---

<center>

# Clase 2 - 19 de marzo, 2024

</center>

## Capa de Aplicación

#### Funciones

1. Provee servicios de comunicación a los usuarios y aplicaciones.
2. Existe modelo de comunicación machine to machine (sin usuarios).
3. Interfaz con el usuario (UI).
4. Las apps que usan la red y los protocolos que implementan las apps pertenecen a esta capa.

#### Componentes

1. Programas que corren en distintas plataformas y se comunican entre si.
2. Las aplicaciones suelen correr en los end systems y no en el núcleo de la red.
3. Integra, segun la OSI:
    1. Capa de Aplicación propiamente dicha:
        - Define el formato y semántica de los mensajes.
        - Existen protocolos que trabajan de forma binaria y otros de forma textual.
        - Define el diálogo (intercambio de mensajes).
        - Por ejemplo: HTTP.
    2. Capa de Presentación:
       ...
    3. Capa de Sesión:
       ...

#### Modelos de comunicación

###### Mainframe

...

###### Cliente/Servidor

...

###### Peer To Peer (P2P)

...

###### Híbrido

...

#### Requerimientos de las aplicaciones

Cada aplicación puede tener distintos requerimientos en términos de seguridad, tiempo de respuesta, confiabilidad, ancho de banda, etc. Según esto, se utilizará uno u otro protocolo de **transporte**: TCP, UDP, SCTP, RUDP. Veremos TCP y UDP.

#### Direccionamiento de procesos

-   Las aplicaciones las implementan los SO como **procesos**.
-   Para que un proceso pueda recibir mensajes, debe tener un identificador único, un puerto.

## HTTP

-   [Video HTTP y curl vimeo](https://vimeo.com/209457973)

#### Elementos

-   Recurso u Objeto HTTP, por ej. una página, una imágen, un gif, archivos multimedia, etc.
-   El recurso está referenciado por una URI (Uniform Resource Identifier) que puede ser una URL (Uniform Resource Location) o URN (Uniform Resource Name).
-   Formato URL: protocol://[user:pass@]host:[port]/[path].
-   Formato URN: no indica ubicación, solo identifican categorías poco implementadas.
-   Una página web es un archivo HTML que incluye vínculos o directamente otros objetos.

#### Funcionamiento

-   Modelo cliente/servidor, request/response.
-   No tiene estado.
-   Corre sobre TCP, en el puerto 80 por default.
-   El cliente elige cualquier puerto no privilegiado (> 1024)

#### Versión 0.9

-   Utiliza TCP.
-   Solo una forma de request (GET).
-   Solo una forma de response.
-   Si el objeto no existe o hay algun error se cierra la conexión abruptamente.

#### Versión 1.0

-   Se especifica la versión HTTP en el request.
-   Se definen distintos **métodos** HTTP (GET, POST, HEAD, PUT, DELETE, etc).
-   Define response codes.
-   Admite caracteres especiales, más allá del ASCII.
-   Admite MIME.
-   Por defecto NO usa conexiones persistentes.

###### Métodos

-   GET: ...
-   HEAD:
-   POST:
-   PUT:
-   DELETE:
-   LINK/UNLINK:

###### Host virtuales

-   Mediante el parámetro Host se pueden multiplexar varios servidores sobre un mismo host.
-   Por ejemplo, 192.168.0.2 podría tener dos sitios, www.uno.com y www.dos.com.

###### Autenticación

-   El servidor, ante un requerimiento de un documento que requiere auth, enviará 401 indicando la necesidad de que el cliente se autentique.

#### Versión 1.1

-   Introduce nuevos mensajes: OPTIONS, TRACE, CONNECT.
-   Conexiones persistentes por omisión.

###### Pipelining

-   No necesita esperar la respuesta del primer requerimiento para realizar el segundo requerimiento, y así.
-   Solo se utiliza con conexiones persistentes.
-   Mejora la performance.
-   **Insertar imagen de pipelining (slide 30)**

###### TRACE y CONNECT

-   TRACE se usa para debugging.
-   CONNECT se usa para generar conexiones a otros servicios montados en HTTP.

###### Redirects

...

#### CGIs Scripts y JavaScript

###### Server-side scripts

-   Un CGI (Common Gateway Interface) es una aplicación que interactúa con un servidor web.
    ...

###### Client-side scripts

...

###### Server-side to Server-side scripts

#### Cookies

-   Mecanismo para manejar estado, ya que HTTP no tiene estado de por sí.
-   Cuando el cliente hace un request, el servidor retorna un recurso indicando al cliente que almacene determinados valores por un determinado tiempo. Esto lo hace mediante el header Set-Cookie:
-   El cliente en cada requerimiento enviará esa cookie con el header Cookie:
-   El servidor puede utilizar esta cookie o no.
-   El servidor puede borrar las cookies del cliente.
-   Las cookies pueden ser usadas para personalizar la experiencia del usuario, y también por client-side scripts.

#### HTTPS

-   Versión segura de HTTP, ya que implementa una capa adicional de cifrado y autenticación.
-   Usa el puerto 443 por defecto.
-   Se basa en una negociación de certificados antes de empezar a realizar requests y responses.

#### Web-Caché

-   Otra optimización de performance de HTTP.
-   Consiste en proxiar y cachear recursos HTTP.
-   Se solicita un objeto HTTP, si está en caché y está "fresco" (HIT) se retorna desde allí sin que el servidor tenga que hacer nada. Si el objeto no está en caché o está "viejo" (MISS) se solicita al servidor y una vez retornado se cachea.
-   Mejora el tiempo de respuesta, reduce la carga en la red, y balancea los requests, intentando atender a todos los clientes.

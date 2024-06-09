<h1 align="center">Clase 1 - 12 de marzo, 2024</h1>

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

<h1 align="center">Clase 2 - 19 de marzo, 2024</h1>

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
        - Convierte y codifica datos a ASCII, UTF-8, etc.
        - Comprime y descomprime datos.
        - Cifra y de-cifra datos.
        - Define formatos y algoritmos para JPEG, MPEG, LZW, etc.
        - Está integrada en las aplicaciones de red mismas.
    3. Capa de Sesión:
        - Administra conversaciones entre aplicaciones.
        - Maneja actividad.
        - Informa excepciones.
        - Integrada en las aplicaciones de red mismas.
        - En general no se usa.

#### Modelos de comunicación

###### Mainframe

-   Una computadora central como único núcleo (el mainframe).
-   Los clientes interactúan con el mainframe vía terminales de E/S.
-   El mainframe procesa todo y muestra el resultado en la terminal.

###### Cliente/Servidor

-   Se comparte la carga.
-   El cliente pone procesamiento de interfaz de usuario.
-   El servidor hace el resto.
-   El servidor corre servicio esperando de forma pasiva la conexión.
-   El cliente se conecta al servidor y se comunican a través de este.

###### Peer To Peer (P2P)

-   La carga es completamente compartida y distribuida.
-   Los peers pueden ser clientes, servidores, o ambos a la vez.
-   Muy escalable en rendimiento.
-   Poco escalable en términos de administración.
-   Por ejemplo: BitTorrent.

###### Peer To Peer Híbrido

-   La carga es completamente compartida y distribuida.
-   Los peers pueden ser clientes, servidores, o ambos a la vez.
-   Muy escalable en rendimiento.
-   Más escalable que P2P en términos de administración.
-   Nodos comunes.
-   Nodos centrales.

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
-   El cliente elige cualquier puerto no privilegiado (> 1024).

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

-   GET: Obtiene el documento requerido. Puede enviar información via la URL (query args).
-   HEAD: Similar a GET pero solo requiere los metadatos del documento y no el documento en sí.
-   POST: Como GET, pero además envía información en el Body. Se suele usar en los formularios.
-   PUT: Reemplaza un documento en el servidor. En general no se usa.
-   DELETE: Borra un documento en el servidor. En general no se usa.
-   LINK/UNLINK: Establece o desestablece relaciones entre documentos.

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
    ![Pipelining](https://i.imgur.com/oSRFwQp.png)

###### TRACE y CONNECT

-   TRACE se usa para debugging.
-   CONNECT se usa para generar conexiones a otros servicios montados en HTTP.

###### Redirects

-   Los códigos 3XX indican redirecciones, es decir que un documento no se encuentra en la URL solicitada, pero sí se encuentra en una nueva URL que el servidor indicará.

#### CGIs Scripts y JavaScript

###### Server-side scripts

-   Un CGI (Common Gateway Interface) es una aplicación que interactúa con un servidor web.
-   Los CGIs leen de STDIN (método POST) o de variables de entorno (método GET).
-   Escriben la respuesta en el STDOUT.
-   Deben anteponer content-Type en el header.
-   POST permite enviar más datos.
-   Lenguajes de scripting seguros y flexibles: PHP, ASP, JSP.

###### Client-side scripts

-   JavaScript estándar W3C.
-   Usan el DOM.
-   Permiten extensiones como AJAX, el cual hace requerimientos particulares y no necesita recargar toda la página.
-   Parsean XML para comunicarse.
-   Existen muchos frameworks que encapsulan esta funcionalidad brindando una interfaz programación y API fácil de usar.

###### Server-side to Server-side scripts

-   Permiten comunicación entre servidores.
-   Modelo de objetos y servicios distribuidos.
-   SOAP.
-   REST.
-   Web Services.

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
-   Actuan como servidores para los clientes y como clientes para los servidores web.
-   En general suelen correr sobre UDP.
-   Ejemplo: CDN.

## HTTP 2.0

-   Reemplaza cómo HTTP se transporta.
-   No reemplaza al protocolo por completo.
-   Los métodos y semántica permanecen igual que antes.

#### Problemas de HTTP 1.0 y 1.1

-   Un request por conexión, muy lento.
-   Por ende se busca implementar el pipelining, conexiones persistentes y conexiones paralelas.
-   El pipelining sigue teniendo algunos problemas por ende se necesita algo más.

#### Diferencias principales con 1.1

-   Usa binario en vez de ASCII (binary framing, más eficiente).
-   Multiplexa varios request en una petición en vez de que sea de forma secuencial y bloqueante. Es decir, se encolan los requests.
-   Usa una conexión para pedir/traer datos en paralelo, agrega datos fuera de orden, prioridad, flow control por frame.
-   Comprime los encabezados para optimizar.
-   Permite a los servidores pushear datos a los clientes.
-   Casi siempre requiere SSL/TLS.
-   Los mensajes HTTP2 se dividen en HEADERS y DATA, a diferencia de 1.1 que no había división.

#### Mux stream, framing

-   Puede generar 1 o más conexiones TCP.
-   Un stream es como una sub-conexión: una conexión HTTP2 dentro de una conexión TCP.
-   Cada stream tiene su ID y su prioridad (opcional).
-   Los streams son bidireccionales.
-   Sobre cada conexión TCP se multiplexan 1 o más streams (de ahí la palabra **mux**).
-   Cada stream transporta mensajes (request, response) y éstos son dividos en frames.
-   Cada frame tiene un header fijo + payload variable.
-   El mismo stream puede llevar diferentes mensajes.

#### Headers

-   Se mantienen la mayoría de headers de 1.1
-   Surgen nuevos pseudo-headers:
-   Por ejemplo, método HEAD /algo HTTP/1.1 se transforma en:
    -   :method: head
    -   :path: /algo
    -   :scheme: http // puede ser https
    -   :authority: www.site.com // reemplaza al header Host:
-   Para las respuestas surge el header status: código, por ejemplo **status: 404**.

#### Priorización

-   Los streams pueden tener prioridades asignadas.
-   El cliente puede especificar qué resources son más importantes que otros, permitiendole al servidor alocar recursos eficientemente.
-   Esto es particularmente útil para optimizar la carga de páginas de forma fluida.

#### Flow control

-   El control de flujo busca prevenir sobrecargar al cliente o al servidor.
-   Se introduce el concepto de ventanas de control de flujo para datos salientes y datos entrantes, para cada stream.
-   El tamaño de estas dos ventanas se va ajustando dinámicamente según la capacidad de los buffers de ambos lados de la comunicación (el del cliente y del servidor).

#### Inline vs push

-   HTTP 2.0 introduce una feature llamada "Server Push" que le permite al servidor enviar datos al cliente antes de que el cliente los pida explicitamente.
-   En el modo Inline, el cliente pide explicitamente los requerimientos usando tags como < link > o < script >.
-   En el modo Push, el servidor anticipa las cosas que el cliente necesitará según el request inicial y se las pushea proactivamente.

#### Compresión y soporte

-   Comprime encabezados.
-   Como el compresor que se usaba, GZIP, tiene vulnerabilidades, se usa HPACK.

#### Otras características

-   HTTP2 permite negociar el protocolo de aplicación.
-   También permite negociar el protocolo alternativo.

## HTTP 3.0

-   Transporta sobre un nuevo protocolo QUIC, que corre sobre **UDP**.
-   Se vuelve a conservar métodos y semántica de HTTP 1.1.
-   UDP presenta algunas vulnerabilidades.

---

<h1 align="center">Clase 3 - 26 de marzo, 2024</h1>

## DNS

-   [Cómo funciona DNS (nic.ar).](https://nic.ar/es/novedades/noticias/como-funciona-el-dns)
-   DNS surge debido a la necesidad de usar nombres mnemónicos y amigables en vez de direcciones IPs para acceder a sitios.
-   Ergo, es un mecanismo para mappear nombres de dominio a direcciones IPs.

#### Elementos

-   Espacio de nombres.
-   Procedimiento de delegación y arquitectura.
-   Base de datos distribuida y servidores.
-   Define las componentes y el protocolo para su comunicación.
-   Procedimiento de búsqueda/resolución (protocolos).

#### FQDN

-   Significa Fully Qualified Domain Name.
-   Es una lista de etiquetas (labels) separadas por puntos.
-   Se leen desde el label de la izquierda hasta la raíz del arbol (punto implícito al final del FQDN).
-   Construyen una estructura jerárquica con sub-nombres (niveles).
-   La sintaxis jerárquica implica la delegación de autoridad.
-   Las etiquetas no son case-sensitive y pueden ser de 63 caracteres como máximo.
-   Pueden tener 127 etiquetas como máximo, y la longitud total del FQDN puede ser como máximo 255 caracteres.
-   El punto implícito simboliza la raíz de la estructura jerárquica DNS.

#### TLDs

-   Los Top Level Domains son los labels siguientes al root ( . ).
-   Se clasifican en 3 tipos:
    1. Generic: .com, .net, .edu, etc.
    2. Country Code: .ar .uk .fr, etc.
    3. ARPA: Dominio especial.

#### Organización

-   El sistema global de DNS es distribuido pero regido por organizaciones.
-   Se utilizan dominios, sub-dominios, y host o servicios (jerarquía).
-   La IANA (Internet Assigned Numbers Authority) controla el funcionamiento.
-   Cada continente tiene su RIR (Regional Internet Register):
    -   ARIN (Norteamérica)
    -   LACNIC (Sudamérica)
    -   RIPE (Europa)
    -   AFRINIC (África)
    -   APNIC (Asia)
-   DNS debe mantener una gran cantidad de información en una base de datos distribuida. Por ende debe ser tolerante a fallos, escalable, y altamente cacheable. Esto último ya que se realizan muchas lecturas pero pocas escrituras.

#### Delegación de autoridad

Por ejemplo:

-   ada.info.unlp.edu.ar fue registrada por la administración de la red de la facultad de informática.
-   El administrador de la facultad tuvo que obtener previamente la autoridad del dominio info.unlp.edu.ar a partir de la administración de la UNLP.
-   La UNLP tuvo que obtener previamente la autoridad del dominio unlp.edu.ar a partir de la administración de "edu.ar", la RIU (Red Inter-universitaria).
-   La RIU tuvo que obtener previamente la autoridad del dominio edu.ar a partir de la administración de ".ar" que depende de alguna organización del gobierno.
-   La administración de nombres gubernamental de la Argentina tuvo que obtener autoridad delegada a partir del IANA o ICANN (organismos internacionales).

#### Raíz

-   Es el punto de inicio ( . ) de cualquier consulta DNS.
-   También llamada "zona raíz" de la jerarquía DNS.
-   Se manejan por 13 ROOT servers distribuidos en el mundo.

#### Funcionamiento de DNS

-   Sigue el modelo Cliente/Servidor.
-   DNS corre sobre UDP y TCP en el puerto 53.
-   No utiliza texto ASCII.
-   Los clientes utilizan un Resolver + cualquier aplicación que requiera la resolución de nombres.

#### Tipos de servidores DNS

-   **Raiz**: Servidor que delega a todos los TLD. Generalmente no permite recursión.
-   **Autoritativo**: Servidor con una zona o sub-dominio de nombres a cargo. Puede sub-delegar.
-   **Servidor Local/Resolver Recursivo**: Servidor que es consultado dentro de una red. Mantiene caché. Puede ser autoritativo. Permite recursivas "internas".
-   **Open Name Server**: Servidores que funcionan como locales para cualquier cliente en el mundo. Por ejemplo 8.8.8.8 (google).
-   **Forwarder Name Server**: Servidores que interactúan directamente con el sistema de DNS exterior. Son DNS proxies de otros DNS internos.
-   **Servidor Primario vs Secundario**: Cuestión de implementación. Dónde se modifican los datos realmente.

#### Registros

-   Son los datos que almacenan los servidores DNS.
-   Poseen el formato:

```
clave | TTL | IN | tipoRegistro | valor
```

#### Otras características

-   **Transferencia de zona**:
    -   Se trata de actualizar los servidores DNS secundarios con el primario.
    -   Utiliza AXFR.
    -   Se realiza sobre **TCP** de forma periódica.
-   **Dynamic DNS**:
    -   Actualización dinámica de registros, usada con IP dinámicas.
-   **Split DNS**:
    -   Responde de acuerdo a de dónde proviene la consulta.

---

<h1 align="center">Clase 4 - 9 de abril, 2024</h1>

## Email

#### Introducción

-   Es uno de los primeros servicios de Internet (cuando las conexiones eran vía modems) y fue creado por Ray Tomlinson en 1971.
-   El formato era MACH2!MACH1!USER.
-   En 1972 se escogió el "@" para denotar "at" (en).
-   En 1982 surgió SMTP.

#### Arquitectura

![Arquitectura email](https://i.imgur.com/c3SJvXU.png)

##### Componentes principales

1. MUA (Mail User Agent)
2. MTA (Mail Transport Agent)
3. MDA (Mail Delivery Agent) o LDA (Local Delivery Agent)
4. MAA (Mail Access Agent)
5. MRA (Mail Retrieval Agent)
6. MSA (Mail Submission Agent)

##### Componentes secundarios

1. Servidor de autenticación externo.
2. Web Mail (Frontend WWW).
3. Servidor de antivirus, antispam, servidores de listas, etc.

#### MUA

-   Es el cliente, es decir la interfaz con el usuario.
-   Puede ser mobile, desktop, o web based.
-   Es responsable de leer, editar y emitir correos locales.
-   Posee intregado un MTA local para comunicarse con el servidor de mail saliente (MTA relay). El MTA relay pre-procesa el email recibido desde el MUA con el agente MSA.
-   Posee intregado un MRA para comunicarse con el servidor de mail entrante (MAA).
-   Agrega la mayoría de los campos del header (id, to, from, subject, etc).
-   Utiliza protocolos SMTP, IMAP, POP y habla con MTA y MAA propios.
-   Ejemplos: Outlook, Thunderbird.
-   Ejemplos de WebMail: Squirrel, Gmail, Yahoo, Hotmail.

#### MSA

-   Es el servidor.
-   Habitualmente integrado en el MTA.
-   Recibe el mensaje del MUA y lo pre-procesa antes de pasarlo al MTA para que realice el transporte.
-   Agrega headers que podrían faltar.
-   Utiliza SMTP o ESMTP.
-   Usa el puerto 25 o 587.
-   Ejemplos: Componentes de Postfix (postdrop, pickup).

#### MTA

-   Es tanto cliente como servidor.
-   Toma el email desde el MSA o directamente desde el MUA (MSA integrado).
-   Se encarga de enviar el email al servidor de la casilla de destino, es decir que comunica de MTA a MTA.
-   Almacena temporalmente el correo saliente.
-   Se encarga de recibir y almacenar temporalmente los mensajes para las casillas que sirve desde el MTA remoto.
-   Utiliza SMTP o ESMTP.
-   Ejemplos: Postfix, Sendmail, MS Exchange, Exim, Qmail.

#### MDA/LDA

-   Servicio interno o servidor.
-   Toma los emails recibidos por el MTA y los lleva al mailbox del usuario local.
-   Se lo llama también LDA.
-   Suele definir el formato del mailbox.
-   Servicio de MDA puede hacer delivery remoto.
-   Se integra con los protocolos POP y/o IMAP dejando los recursos disponibles al MAA o al usuario directamente.
-   Ejemplos: procmail, postfix local, Sendmail, courrier, cyrus-IMAP, etc.

#### MAA

-   Servidor.
-   Puede estar integrado en el MDA o separado.
-   Autentica al MUA y lee los emails del mailbox local que dejó el MDA/LDA.
-   Transporta los emails hacia el MUA.
-   Implementa los protocolos POP y/o IMAP dejando acceder a los recursos y dialogando con el MUA.
-   Ejemplos: courrier, cyrus-IMAP, dovecot, sieve, etc.

#### SMTP

-   Significa Simple Mail Transfer Protocol.
-   Usa el modelo Cliente/Servidor.
-   Utiliza el formato ASCII.
-   Por defecto usa el puerto 25.
-   Trabaja con TCP.
-   Los servidores de email se comunican entre sí vía SMTP.
-   Los MTA locales de los MUA se comunican con su servidor saliente vía SMTP.
-   Usa conexiones persistentes.
-   Puede trabajar de forma interactiva (request/response) o pipeline.
-   Puede o no requerir auth.
-   Puede o no ser seguro (SSL/TLS).

#### Formato de los mails

1. From:
2. To:
3. Subject:
4. Date:
5. MIME-Version:
6. Content-Transfer-Encoding:
7. Content-Type:

#### Datos binarios

Como los emails trabajan con ASCII, todo lo que sea binario (imagenes, archivos, etc) debe traducirse a un formato compatible, por ejemplo base64.

#### Protocolos de acceso a emails

-   POP vs IMAP.
-   Ambos requieren autenticación.
-   Ambos utilizan ASCII.
-   POP usa el puerto 110, IMAP el 143.
-   Ambos admiten SSL/TLS.
-   IMAP permite el uso de carpetas y manipulación de mensajes en el servidor.

---

<h1 align="center">Clase 5 - 16 de abril, 2024</h1>

## FTP

-   FTP (File Transfer Protocol) es un protocolo antiguo cuyo objetivo es transferir archivos completos.
-   Utiliza el modelo Cliente/Servidor.
-   Corre sobre TCP.
-   Los clientes FTP no requieren UI.
-   Es soportado por los browsers mediante la URI: ftp://...

#### Funcionamiento

Usa 2 conexiones TCP:

1. **Conexión de Control**: Donde viajan los comandos y el estado (éxito por ej) de cada operación. Utiliza por defecto el puerto 21.
2. **Conexión de Datos**: Donde viajan los datos. Se crea y se cierra bajo demanda. Utiliza cualquier puerto no privilegiado (> 1024).

#### Comandos

-   **RETR**: Obtiene un archivo desde el servidor.
-   **STOR**: Envía un archivo al servidor.
-   **LIST**: Lista los archivos del directorio actual en el servidor.
-   **DELE**: Borra un archivo en el servidor.
-   **SIZE, STAT**: Obtiene información de un archivo en el servidor.
-   **CD, PWD, RMD, MDK**: Cambia de directorio, obtiene el directorio actual, borra el directorio, crea un directorio.

#### FTP Activo vs Pasivo

FTP posee 2 modos, se diferencia cómo maneja la conexión de **datos**:

En el modo Activo, el cliente abre uno de sus puertos para la conexión de datos, y **el servidor se conecta al cliente**.

En el modo Pasivo, el servidor abre uno de sus puertos para la conexión de datos, y **el cliente se conecta al servidor**.

| Aspecto                          | FTP Activo                                                                            | FTP Pasivo                             |
| -------------------------------- | ------------------------------------------------------------------------------------- | -------------------------------------- |
| Puerto de control de ambos lados | 21                                                                                    | 21                                     |
| Puerto de datos del cliente      | > 1024                                                                                | > 1024                                 |
| Puerto de datos del servidor     | 20                                                                                    | > 1024                                 |
| Comando PORT                     | Lo utiliza el cliente para indicarle al servidor cuál de sus puertos está escuchando. | No se usa, se utiliza el comando PASV. |
| NAT/firewall                     | Puede generar problemas.                                                              | No suele generar problemas.            |
| Seguridad                        | Puede ser menos seguro.                                                               | Puede ser más seguro.                  |

#### Formato de datos

-   Se puede utilizar formato ASCII o binario según el tipo de archivo.
-   Para imágenes, videos, programas ejecutables, etc se utiliza binario y los datos se transfieren byte a byte sin realizar ninguna conversión.
-   Para archivos de texto, los caracteres se transmiten de acuerdo a la codificación ASCII estándar.

#### Alternativas a FTP

-   FTP seguro: FTPS (FTP con SSL/TLS).
-   Versiones integradas con la suite de Open-SSH: SCP y SFTP.
-   Aplicaciones para compartir recursos: NFS, SMB/CIFS, iSCI, etc.
-   TFTP.

#### FTP vs HTTP

| Aspecto              | FTP                                            | HTTP                                   |
| -------------------- | ---------------------------------------------- | -------------------------------------- |
| Subida vs bajada     | Diseñado para subida pero soporta descarga.    | Se pueden ambos con PUT y POST.        |
| Formato              | ASCII/Binario.                                 | meta-data, Content-Type, más flexible. |
| Headers              | No los usa.                                    | Los usa.                               |
| Performance          | Command/Response, archivos pequeños más lento. | Pipelining.                            |
| NAT/firewalls        | Menos amigable.                                | Más amigable.                          |
| Número de conexiones | 2.                                             | 1.                                     |
| Ranges/resume        | Soportado.                                     | Soportado y más avanzado.              |
| Autenticación        | Soportado.                                     | Soportado.                             |
| Cifrado              | Soportado.                                     | Soportado.                             |
| Compresión           | RLE.                                           | Deflate: LZ77 + Huffman.               |

---

<h1 align="center">Clase 6 - 16 de abril, 2024</h1>

## Capa de Transporte

#### Relación entre Capa de Aplicación, Capa de Transporte y Capa de Red

-   La Capa de Aplicación comunica usuarios o agentes.
-   La Capa de Transporte comunica procesos.
-   La Capa de Red comunica hosts.

#### Función de la Capa de Transporte

1. Encapsulación: define su PDU, el segmento/datagrama.
2. Multiplexación: puertos.
3. Soporte de datos de tamaños arbitrarios.
4. Control y detección de errores (pérdida, duplicación, corrupción).
5. Control de flujo.
6. Control de congestión.

#### TCP vs UDP

La Capa de Transporte posee 2 modelos básicos: TCP, el cual es confiable; y UDP, el cual es no confiable.

#### UDP

-   Significa User Datagram Protocol.
-   Es un protocolo simple, con poco overhead.
-   Comparte características del protocolo IP, como el ser Best-Effort.
-   Su PDU es el Datagrama.
-   Tiene poca detección de errores, aunque no nula.
-   No establece conexión.
-   Se suele utilizar en aplicaciones donde la pérdida de algunos datos no es crítica, por ejemplo: Streaming de voz o video, TFTP, DNS, Broadcast, Multicast, etc.

##### Datagrama UDP

![Datagrama UDP](https://i.imgur.com/ki8TkNr.png)

-   Los puertos Source y Dest. permiten la multiplexación y de-multiplexación.
-   Longitud datagrama = (Longitud header) + (Longitud payload).
-   El checksum se utiliza para detección de errores y es opcional.

#### TCP

-   Significa Transmission Control Protocol.
-   Es un protocolo complejo, confiable, ordenado y con buffering con mucho mayor overhead que UDP.
-   Posee control de errores, de flujo y de congestión.
-   Es orientado a streams.
-   Su PDU es el Segmento.
-   Provee multip. y de-multip. vía puertos.
-   Requiere establecer conexión.
-   Se suele utilizar en aplicaciones donde la confiabilidad de los datos es crítica, por ejemplo: Transferencia de archivos, FTP/HTTP/SMTP, acceso remoto (SSH, telnet), Unicast, etc.

##### Segmento TCP

![Segmento TCP](https://i.imgur.com/pRHP0hC.png)

-   Los puertos proveen MUX y DEMUX.
-   No posee longitud total constante, es variable.
-   El tamaño del segmento se calcula dentro del datagrama IP.
-   El checksum es obligatorio y sirve para detección de errores.
-   Los flags SYN FIN y RST sirven para la sesión TCP.
-   Los campos ACK, Num, Seq sirven para el control de errores.
-   El campo Window sirve para control de flujo.
-   Algunos otros flags sirven para control de congestión.

##### Funcionamiento de TCP

-   TCP entrega y envía los datos agrupados o separados de forma desasociada de la aplicación:
    -   La aplicación puede enviar 3000 bytes en un write y TCP podría enviar 3 segmentos de 1000 bytes cada uno.
    -   La aplicación puede enviar 100 bytes y luego 200 y TCP puede esperar para enviarlos todos juntos en un solo segmento de 300 bytes.
    -   La aplicación puede intentar leer 200 bytes del buffer y TCP solo entregar 150 bytes y luego los restantes 50.
-   TCP requiere mantener recursos en ambos extremos de la conexión:
    -   Buffers de envio y de recepción.
    -   Timers Retransmission Timeout (RTO).
    -   Variables: datos enviados, no confirmados, retransmisiones, umbrales, RTT, etc.
    -   Estado de la conexión.

##### Establecimiento de la conexión TCP

-   Se logra vía el 3WH (3 Way Handshake): el cliente envía el flag SYN, el servidor envía SYN + ACK, y por último el cliente envía ACK.
-   En el tercer segmento se pueden enviar datos.
-   El 3WH utiliza el ISN (Initial Sequence Number) el cual es aleatorio.
-   Si un proceso no está en estado LISTEN y recibe una petición de establecimiento de conexión, devolverá un segmento vacío con flag RST.

##### Cierre de la conexión TCP

-   Se logra vía el 4WC o 3WC (4 Way Close o 3 Way Close).
-   El flag RST se utiliza para comenzar el cierre de la conexión.

##### Control de errores TCP

-   En IP pueden existir varios errores: pérdida de paquetes, desorden, retardos, datos duplicados, datos corruptos.
-   TCP intenta solucionar estos problemas vía su implementación del control de errores.

##### Estados de conexión TCP

![Estados de conexión TCP](https://i.imgur.com/awEzJF2.png)

---

<h1 align="center">Clase 7 - 30 de abril, 2024</h1>

## Control de errores en TCP

#### Posibles errores en Capa de Red o inferior

-   Los errores pueden generarse en los extremos (host local y host remoto) o en la red misma debido a la congestión o a fallos en la red.
-   Los posibles errores son los siguientes:
    1. Pérdida total del paquete.
    2. Duplicación del paquete.
    3. Corrupción del paquete.
    4. Desorden del arrivo de los paquetes.

#### Mecanismos de control de errores

-   El control de errores de TCP se realiza mediante estas 5 técnicas:
    1. Acknowledgement: Se utilizan números de ACK para confirmar que los datos que se enviaron fueron efectivamente recibidos. **Busca solucionar el error de pérdida total del paquete.**
    2. Números de Secuencia: Se asigna un número de secuencia a cada segmento TCP enviado para que el receptor sepa cuál es el orden correcto de los segmentos. **Busca solucionar el error de desorden de arrivo de los paquetes.**
    3. Retransmisión: Si el emisor no recibe un ACK pasado un cierto tiempo (RTO o Retransmission TimeOut) procede a retransmitir ese segmento. **Busca solucionar los problemas de pérdida total del paquete y también de corrupción del paquete.**
    4. Ventana: Se utiliza un mecanismo de ventana deslizante que permite enviar varios segmentos juntos en vez de enviar solo uno y esperar el ACK. **Esto ayuda de forma indirecta a reducir la pérdida de paquetes ya que reduce la congestión de la red, causa principal de ese error.**
    5. Checksum: Cada segmento posee un checksum para que el receptor chequee que el segmento que se envió es exactamente el mismo que el que se recibió. **Busca solucionar el problema de corrupción del paquete.**

#### ARQs

-   Automatic Repeat reQuest son una serie de técnicas usadas en TCP para asegurar la confiabilidad de la transmisión de datos sobre canales no confiables como IP.
-   Existen 3 técnicas ARQ principales: Stop & Wait, Go-Back-N y Selective Repeat.
-   La idea general de los ARQ es la siguiente:
    1. El emisor envía el segmento.
    2. El receptor recibe el segmento y lo chequea. Si posee errores devuelve un ACK negativo.
    3. Si el emisor recibe un ACK negativo o se acaba el RTO, retransmite el segmento.

#### ARQ Stop & Wait

-   Es el más simple pero el más ineficiente.
-   Se transmiten datos de a un segmento por vez.
-   El emisor envía un segmento y hasta no recibir un ACK no vuelve a enviar otro segmento.
-   Si el emisor no recibe un ACK luego de que se acabe el RTO o recibe un ACK negativo, asume que el segmento se perdió o tuvo errores, así que lo retransmite.

#### ARQ Go-Back-N

-   Más complejo para más eficiente ya que implementa pipelining.
-   Se transmiten datos de a varios segmentos a la vez.
-   El emisor envía N segmentos y luego espera un ACK cumulativo: El receptor envía un solo ACK para que el Sequence Number más alto que no tuvo errores.
-   Si el emisor no recibe un ACK luego de que se acabe el RTO o recibe un ACK negativo, asume que uno o más de los segmentos se perdieron o tuvieron errores, así que retransmite todos ellos empezando por el primero de todos ellos que no recibió ACK.
-   Utiliza una ventana deslizante (Sliding Window).

#### ARQ Selective Repeat

-   Similar a Go-Back-N pero más eficiente.
-   Se transmiten datos de a varios segmentos a la vez.
-   El emisor envía N segmentos y debe recibir un ACK por cada uno, a diferencia de Go-Back-N.
-   Si el emisor no recibe un ACK para un segmento particular luego de que se acabe el RTO de ese segmento o recibe un ACK negativo, asume que ese segmento se perdió o tuvo errores, así que retransmite solo ese segmento.
-   También utiliza una ventana deslizante (Sliding Window) aunque funciona de manera diferente.

---

<h1 align="center">Clase 8 - 7 de mayo, 2024</h1>

## Control de flujo en TCP

#### Conceptos básicos

-   Se busca un algoritmo que permita al receptor controlar la **tasa** a la que le envía datos el transmisor.
-   Se usa para controlar cuánto puede enviar una aplicación sabiendo que la app receptora tiene capacidad N de recibirlo y procesarlo.
-   Se busca que el emisor no **sobrecargue** de datos al receptor, además de evitar **desperdiciar** recursos de la red general.
-   Para lograrlo se usan técnicas de ARQ + RNR o ARQ + Dynamic Window: la capacidad de envió será el **mínimo** entre Congestión, Flujo, Errores.

#### Ventana deslizante

-   El control de flujo utiliza un mecanismo de ventana deslizante que permite enviar multiples segmentos sin esperar por ACKs.
-   Este mecanismo posee 2 ventanas: la del emisor y la del receptor.
-   La ventana del emisor representa la máxima cantidad de segmentos que el emisor puede enviar sin esperar ACKs.
-   La ventana del receptor indica la cantidad de espacio disponible actual en el buffer del receptor.
-   El emisor ajusta la velocidad a la que envia los datos basado en el window size del receptor.

## Control de congestión en TCP

#### Conceptos básicos

-   Se busca controlar la tasa de envío en base al estado actual de la red.
-   Se empieza lento: cuando la conexión inicia la ventana de congestión es 1.
-   Luego esa ventana empieza a aumentar exponencialmente hasta que se llega a un umbral predefinido o tener 3 ACKs duplicados.
-   ?

#### Slow Start

-   Algoritmo que ajusta la velocidad de envío basado en las condiciones de la red en su totalidad.
-   Cuando la conexión está recién establecida, el emisor empieza con una velocidad de envío bastante lenta.
-   Luego de esto, se empieza a incrementar la velocidad exponencialmente, duplicando la cantidad de segmentos que envía por cada ACK que recibe.
-   Continúa creciendo exponencialmente hasta que llega a un límite de congestión preestablecido o detecta packet loss.
-   Una vez que se detecta congestión, se pasa a la fase de Congestion Avoidance.

#### Congestion Avoidance

-   Utiliza crecimiento lineal y no exponencial, a diferencia de Slow Start.

---

<h1 align="center">Clase 9 - 14 de mayo, 2024</h1>

## Protocolo IP

#### La Capa de Red

-   La Capa de Red es la que le da la estructura al Internet.
-   Utiliza el protocolo IP, tanto en versión 4 como versión 6.

#### Conceptos básicos

-   Este protocolo trabaja de extremo a extremo (end to end).
-   El ruteo se produce salto a salto, es decir que las rutas no están predefinidas, en cada nodo se decide a dónde ir según las tablas de ruteo.
-   Cada nodo del camino debe implementar IP.
-   **Brinda servicios** a la capa de Transporte y **usa servicios** de la capa de Enlace.
-   Posee 2 versiones, IPv4 e IPv6, las cuales **no son compatibles** entre sí.
-   Es un protocolo similar a UDP en 2 sentidos:
    -   No es orientado a conexión.
    -   Es Best Effort, no confiable, no garantiza que los mensajes lleguen.
-   Su PDU es el datagrama o más comunmente paquete IP.

#### Funcionalidades

-   Posee varias funcionalidades:
    -   Direccionamiento: cómo identificar a cada uno de los hosts.
    -   Ruteo/forwarding/switching: cómo llegar de un host a otro.
    -   Multiplexing/demultiplexing.
    -   Fragmentación.
    -   Detección de bucles.
    -   Detección de errores básico aunque no toma ninguna acción para solucionarlos.

#### Estructura

![Esquema de IP en TCP/IP](https://i.imgur.com/udD8TEh.png)

-   IP es el núcleo de internet.
-   Como es Best Effort, requiere protocolos "Helpers".

#### Direccionamiento

-   Una **dirección IP** identifica unívocamente un punto de acceso (interfaz) en la red.
-   Un router o un host multi-homed tienen más de una IP. Cada interfaz tiene una IP única. Una interfaz puede tener varias IPs.
-   Tienen un significado global en internet (IPs públicas) o privado (IPs locales).
-   Las direcciones IPs globales son asignadas por una autoridad central: InterNIC, IANA, ICANN, LACNIC, etc.

#### Direcciones IP

-   Son números de 32 bits divididos en 4 partes de 1 byte cada uno separados por puntos.
-   Son 4G de direcciones (2^32) puras que organizadas de forma jerárquica se reducen.
-   Son necesarias para el ruteo.
-   Son direcciones **lógicas**.
-   Poseen 2 partes: la parte de **red** y la parte de **host**.

#### Clases de direcciones IP

-   Originalmente, las direcciones IP se dividían en **clases**:
    -   Clase A: pocas redes pero cada una alberga muchos hosts.
    -   Clase B: más redes pero más chicas cada una.
    -   Clase C: muchas redes muy chicas.
-   Luego este concepto evolucionó al concepto de **subredes y máscaras**.

![Clases de direcciones IP](https://miro.medium.com/v2/resize:fit:720/format:webp/1*wbYRk65-lnwsWYSFJ656xw.png)

#### Tipos de direcciones IP

-   Unicast: destino a **un** host/interfaz particular.
-   Broadcast: destino a **todos** los hosts en una red.
-   Multicast: destino a **varios** hosts en una red o varias redes. Clase D.
-   Anycast: destinada al **primero** que resuelva.

#### Direcciones IP especiales

-   Dirección de loopback, unicast, clase A: 127.0.0.1.
-   Dirección de red, la primera de su grupo: por ejemplo 192.168.1.0
-   Dirección de broadcast, la última de su grupo: por ejemplo 192.168.1.255
-   Dirección de broadcast limitado: 255.255.255.255
-   Dirección de "este host" cuando aún no tiene una dirección asignada: 0.0.0.0

#### Direcciones IP privadas

-   No tienen significado global.
-   No son únicas.
-   Se utilizan en intranets, es decir redes autónomas sin conexión a Internet.
-   Son las siguientes:
    -   Clase A: 10.0.0.0 - 10.255.255.255
    -   Clase B: 172.16.0.0 - 172.31.255.255
    -   Clase C: 192.168.0.0 - 192.168.255.255

#### Problemas con el direccionamiento fijo (clases)

-   Si por ejemplo necesitamos 4 redes de 25 hosts cada una, podemos usar 4 redes clase C, ya que las clase C soportan como máximo 255. Por ejemplo las siguientes:
    -   Red A: 193.168.1.0
    -   Red B: 193.168.2.0
    -   Red C: 193.168.3.0
    -   Red D: 193.168.4.0
-   El problema con esto es que 25 es mucho menor que 255, es decir tenemos 255 - 2 (dirección de red y de broadcast no se pueden usar)= 253 - 25 = 228 direcciones sin usar, desperdiciadas.
-   Esto se puede solucionar con el subnetting.

#### Subnetting fijo

-   Se toma una parte de la IP para generar subredes dentro de la red.
-   Se agrega una máscara de bits.
-   Para saber la subred se aplica un AND entre la dirección y la máscara de esa dirección.
-   Las máscaras se escriben en notación decimal o como longitud de prefijo, por ejemplo:
    -   255.255.255.0 o /24.
    -   255.255.0.0 o /16.
    -   255.255.255.252 o /30.
-   Las máscaras default para las clases son:
    -   Clase A: 255.0.0.0
    -   Clase B: 255.255.0.0
    -   Clase C: 255.255.255.0
-   Si por ejemplo necesitamos 4 redes de 25 hosts cada una, podemos usar 1 red clase C dividida en 4:
    -   Red A: 193.168.4.0 con máscara 255.255.255.192 o /26
    -   Red B: 193.168.4.64 con máscara 255.255.255.192 o /26
    -   Red C: 193.168.4.128 con máscara 255.255.255.192 o /26
    -   Red D: 193.168.4.192 con máscara 255.255.255.192 o /26

#### VLSM Subnetting

-   Surge como una mejora al subnetting fijo.
-   La longitud de la máscara ya no tiene necesidad de ser igual para todas las subredes.
-   En el ejemplo anterior, tenemos 4 subredes de 62 hosts cada una. Pero qué pasa si queremos 70 hosts para la red A, 40 para la red B, y 25 para la red C y D? En este caso tendríamos, vía VLSM:
    -   Red A: 193.168.4.0 con máscara /25
    -   Red B: 193.168.4.128 con máscara /26
    -   Red C: 193.168.4.192 con máscara /27
    -   Red D: 193.168.4.224 con máscara /27

#### CIDR - Supernetting

-   Classless Inter Domain Routing.
-   Las clases dejan de existir.
-   Siempre se especifica la máscara de la dirección IP.

#### Datagrama IPv4

![Datagrama IPv4](https://i.imgur.com/Gl36MZF.png)

-   La versión (4 bits) indica si es IPv4 o IPv6.
-   El header length (4 bits) indica la longitud en múltiplos de 4 bytes.
-   Identification (16 bits) se usa para la fragmentación.
-   Los flags (3 bits) se usan también para la fragmentación.
-   El TTL (1 byte) indica cuántos saltos puede dar el datagrama y evita loops. Por cada router que pasa se decrementa en 1.
-   Protocol (1 byte) indica TCP, UDP, ICMP, etc y se usa para mux/demux.
-   Header checksum (2 bytes) se usa para detectar errores.
-   Options tiene security restrictions, record route, timestamp, etc.

#### Ruteo

-   El ruteo se logra a través de las **tablas de ruteo**, tablas que poseen cada host y router y que indican el salto siguiente.
-   El host **no despacha** mensajes que recibe que no son para él.
-   El router despacha cualquier mensaje mirando su tabla.
-   Es decir, llega un mensaje y según sus datos y la información en la tabla de ruteo se selecciona la interfaz de salida y el próximo salto.

#### Fragmentación

-   Debido a que hay diferentes capas de enlaces con diferentes MTUs (Maximum Transmission Unit), muchas veces ocurre que un datagrama o paquete IP no entra por ende debe ser descompuesto en partes más pequeñas, llamadas **fragmentos**.
-   Los fragmentos son múltiplos de 8 bytes y tienen offset en unidades de 8 bytes.
-   Ejemplo:

![Ejemplo de fragmentación IP](https://i.imgur.com/yrTfqQZ.png)

---

<h1 align="center">Clase 10 - 28 de mayo, 2024</h1>

## Protocolos de ruteo

#### Introducción

-   El **enrutado** se refiere al proceso de enviar paquetes de una red a otra a través de routers.
    -   IP es un protocolo de enrutado.
-   El **enrutamiento** o routing es el proceso de determinar la mejor ruta por la cual los datos deben viajar a través de una red.

    -   Se logra a través de las tablas de ruteo.
    -   Para que los routers puedan construir y mantener sus tablas de ruteo, utilizan **protocolos de enrutamiento** que permiten que los routers se comuniquen entre sí.
        -   Algunos ejemplos son: OSPF (Open Shortest Path First), BGP (Border Gateway Protocol) y RIP (Routing Information Protocol)

-   El forwarding consiste en seleccionar un port de salida en función de la dirección de destino y lo que indique la tabla de ruteo. Lo utiliza el protocolo de **enrutado**.

#### Routing

-   Las decisiones de forwarding en IP se llevan a cabo localmente.
-   Produce conectividad entre los distintos puntos de la red.
-   Se requieren recolectar y procesar un estado global.
-   Se mantiene un estado global localmente en cada router.
-   Los estados locales deben ser consistentes, si no la red no habrá convergido a un estado estable y se generan loops.
-   Se requiere:
    -   Consistencia
    -   Completitud
    -   Escalabilidad
-   Se desea:
    -   Caminos óptimos
    -   Balanceo
    -   Adaptabilidad
-   Existen 2 formas de routing que utilizan los routers:

##### Ruteo estático

-   Las rutas son establecidas de antemano por el administrador de forma manual.
-   Propenso a errores.
-   Si se cambia la topología requiere cambios manuales en los routers.
-   Sirve cuando se tiene una red sencilla.
-   No tiene problemas de seguridad ni de incompatibilidad.
-   No implica costo de procesamiento extra.
-   Hay un mayor control.
-   No es escalable ni tolerante a fallos.

##### Ruteo dinámico

-   Requiere una configuración inicial por el administrador.
-   Si la topología cambia, se adapta automáticamente.
-   Facilita mantenimiento cuando se tiene una red compleja.
-   Implica procesamiento extra.
-   Es escalable y tolerante a fallos.
-   El debugging es más complejo.

## Protocolo ICMP

#### Introducción

-   Se trata de un protocolo de capa de red.
-   Es "helper" de IP, ya que le agrega un control que IP por sí mismo no tiene, y le brinda feedback para poder resolver problemas en la red.
-   Se encapsula **dentro** de IP.
-   No es un protocolo de transporte.

#### Mensajes

1. **Ping (Echo Request/Reply):**
    - Está pensado para probar conectividad IP entre dos hosts.
    - Sirve para medir el RTT y de esta forma diagnosticar problemas.
    - Si un nodo recibe un ICMP Echo Request, debe responder con Echo Reply.
2. **Destinos inalcanzables:**
    - Si el host está apagado o no responde, se recibe ICMP Host Unreachable.
    - Si la red destino no se puede encontrar (el router no la tiene en su tabla), se recibe ICMP Network Unreachable.
    - Si no hay un proceso UDP en el puerto destino, se recibe ICMP Port Unreachable.
3. **TTL expirado:**
    - El tiempo de vida expiró, es decir que el paquete tardó demasiado y no llegó a destino a tiempo, posiblemente porque se quedó en loop.
    - TTL está tanto en ICMP como en IP.
    - Valor máximo de TTL = 255, pero puede empezar con cualquier valor desde 0.
4. **Source Quench (Control de congestión)**
5. **Redirección de ruta**
6. **Address Mask y Timestamp**

## Protocolo DHCP

#### Introducción

Para que un host se pueda conectar a una red IP necesita 3 parámetros + 1:

1. Para conectarse a la red local:
    - Dirección IP
    - Máscara
2. Para conectarse a otras redes:
    - Router default (Default Gateway)
3. Para usar servicios:
    - Servidor de DNS

Estos parámetros se pueden obtener de distintas formas según si la configuración es manual o dinámica:

1. En la configuración manual:

    - Difícil de mantener.
    - No escalable.
    - No sirve para mobilidad.

2. En la configuración dinámica:
    - RARP.
    - ICMP.
    - BOOTP.
    - DHCP.

#### Definición

-   DHCP (Dynamic Host Configuration Protocol) es otro protocolo de red "helper" de IP.
-   Como está montado sobre UDP, se lo suele considerar protocolo de nivel de aplicación.
-   Se utiliza tanto en IPv4 como en IPv6.
-   Permite la **configuración dinámica de los parámetros de red** de los hosts.

#### Funcionamiento

-   Los hosts inicialmente solo tienen acceso a su red local de forma broadcast.
-   En la red local existe uno o más servidores de auto-configuración llamados **DHCP servers**.
-   Los hosts sin parámetros de red envían requerimientos.
-   Estos servidores atienden los pedidos asignando los valores que brindan conectividad.
-   El parámetro se reserva por un tiempo.
-   El cliente está montado sobre UDP en el puerto 68 -> "Bootpc"
-   El server está montado sobre UDP en el puerto 67 -> "Bootps"

#### Mensajes

1. Discover -> Es de tipo broadcast
2. Offer -> Es de tipo unicast pero a veces también broadcast
3. Request -> Es de tipo broadcast
4. ACK
5. Release
6. NAK

## NAT

#### Problemas con IPv4

-   Como IPv4 solo usa 32 bits, su espacio de direcciones se empezó a agotar muy rápido.
-   Debido a esto, las soluciones temporales que surgieron fueron:
    -   CIDR: Reduce las tablas de ruteo
    -   DHCP: Intenta reducir la cantidad de direcciones
    -   NAT: Intenta reducir la cantidad de direcciones
-   La solución definitiva surgió en la forma de IPv6 debido a que este protocolo utiliza 128 bits.

#### Problemas con direcciones privadas

-   Como no son únicas:
    1. Las rutas pueden ser confundidas.
    2. Habitualmente son filtradas por routers de borde.
    3. Algunos protocolos no funcionan adecuadamente: FTP, VoLP, etc.
-   Por esto se requiere un mecanismo de traducir direcciones privadas a públicas:
    1. Vía NAT (Network Address Translation)
        - Estático.
        - Dinámico.
    2. NAPT (Network Address Port Translation)
        - Dinámico sobre pool.
        - Dinámico sobre dir. overload/masquerade.

#### NAT estático vs dinámico

-   De forma básica, se realiza **one-to-one**.
-   Se mappea una dirección IPv4 privada a una pública.
-   Si se hace de forma estática requiere la misma cantidad de direcciones públicas que de privadas.
-   Si se hace de forma dinámica no es necesario, pero sí se requiere un timer por cada entrada. Limita acceso simultáneo de acuerdo al pool pub.

#### NAPT

-   El NAT básico no es implementable cuando se tiene un pool chico de direcciones o no se poseen direcciones públicas asignadas.
-   En este caso se debe trabajar con campos de la capa de transporte o del payload.
-   NAPT también se lo conoce como PAT (Port Address Translation).
-   Es **one-to-many**, a diferencia de NAT.
-   Se utilizan los puertos de los protocolos u otros valores como ICMP Identifier para resolver el mapeo.
-   Se pueden usar timers y sesión del protocolo.
-   En la tabla de traducciones se mantienen el protocolo y los puertos origen y destino.
-   Se intenta conservar el puerto origen, pero si está "ocupado" se debe reemplazar por otro.
-   El dispositivo debe violar los límites impuestos por la división en capas.
-   Existen dos alternativas de funcionamiento:
    1. Utilizando un pool y haciendo PAT sobre éste.
    2. Utilizando la dirección IP externa y haciendo overloading/masquerading sobre ésta.

#### Port Forwarding

-   Overloading/masquerading no permiten acceso "desde fuera hacia adentro", solo se permite entrar tráfico de conexiones generadas internamente.
-   Port Forwarding (re-envío de puerto) permite poder tener servicios en una red privada accesibles desde "afuera".
-   No requiere NAT estático, se implementa con NATP y mapeo inverso estático de puertos.

---

<h1 align="center">Clase 11 - 4 de junio, 2024</h1>

## IPv6

#### Evolución de IPv4

1. Inicialmente IPv4 era Classful.
2. Luego se creó CIDR (Classless Inter Domain Routing).
3. Luego se introdujeron las direcciones IPv4 privadas.
4. Después NAT/NAPT.
5. NAT introdujo problemas al complejizar demasiado la red y dificultar el acceso directo a la red privada, entre otras cosas.
6. Para solucionar esto se fueron agregando varios parches: Port Forwarding, UPnP, CGN, STUN, NAT traversal, etc.
7. Pero nada de esto era escalable, por ende se decidió crear otro protocolo: IPv6.

En resumen:

-   Las direcciones IPv4 se agotaban, y por esto se introdujo NAT.
-   Las tablas de ruteo se volvieron muy grandes.
-   Se empezó a producir congestión en los routers por haber demasiado procesamiento.
-   En el diseño original de IPv4 no se contemplaron varias cuestiones como:
    -   Seguridad.
    -   Extensiones al modelo de Calidad de Servicio.
    -   Fácil auto-configuración y renumeración de direcciones.
    -   Movilidad a nivel de red.

#### Cambios y beneficios en IPv6

IPv6 introdujo:

-   Direcciones más largas (128 bits en vez de 32).
-   Datagramas de 40 bytes.
-   Simplifica la cabecera:
    -   Se saca la fragmentación, se deja solo de extremo a extremo como opción.
    -   Se saca el checksum de la cabecera.
    -   Header de tamaño fijo. No existen más las opciones.
    -   Identificador de flujo (20 bits).
    -   Se renombran los campos: Traffic Class, Hop Limit, Next Header.
    -   Cabeceras de extensión.

Y esto trae los siguientes beneficios:

-   Mayor espacio de direcciones.
-   Formato de cabecera simplificado.
-   Menor overhead de procesamiento.
-   Ordenar las tablas de ruteo.
-   Conectar todo, usar auto-config de direcciones.
-   Arquitectura de red jerárquica para un ruteo eficiente.
-   Seguridad a nivel IP.
-   Jumbogramas, size(datagrama) > 64KB.
-   Movilidad y más direcciones de multicast.

#### Cabecera IPv6

![Cabecera IPv6](https://i.imgur.com/uWTrkvX.png)

#### Servicios básicos IPv6

-   Direccionamiento.
-   Ruteo/Forwarding.
-   Mux/Demux de protocolos superiores.
-   Cómo evitar loops.
-   Descubrimiento de Vecinos (Neighbour Discovery Protocol):
    -   ND propiamente dicho.
    -   Router discovery y auto-configuración.
-   Manejo de grupos de Multicast.
-   Cabeceras de extensión:
    -   Permite extender las funcionalidades del protocolo.
    -   Se encuentran a continuación del header.
    -   En general, son procesadas por los extremos.
    -   El orden de estas cabeceras puede ser:
        -   Hop-by-hop: procesado por cada router.
        -   Dest Opt: procesado por routers incluidos.
        -   Routing: procesado por routers, RH0 desaconsejado.
        -   Frag, Auth, Sec, Dest. procesado por extremos.

#### Tipos de direcciones IPv6

-   Unicast.
-   Anycast (tomadas del rango Unicast).
-   Multicast (no hay direcciones broadcast): FF00::/8

#### Scope de las direcciones Unicast

-   Locales (Link-local): FE80::/64
-   De sitio (site-local, unique-local)
-   Compatibilidad (ipv4-compat, ipv4-mapped)
-   Globales: 2000::/3

#### Notación

-   Hexadecimal en grupos de 16 bits separadas por ":".
-   Los ceros al inicio de cada grupo se pueden obviar:
    -   2001:0db8:1011:0001:36ed:04ff:fe32:0076 == 2001:db8:1011:1:36ed:4ff:fe32:76
-   Los ceros contiguos se pueden eliminar con "::". Esto solo se puede usar una vez por dirección.
    -   2001:db8:1011:1:0:0:0:1 == 2001:db8:1011:1::1
    -   2001:db8:0:0:1011:0:0:1 == 2001:db8:0:0:1011::1
    -   2001:db8:0:1011:0:0:0:1 == 2001:db8:0:1011::1
-   Se usa "[]" para indicar el puerto en una URL:
    -   http://[2001:db8:1011:1:0:0:0:1]:8080
-   No se usa máscara, solo prefix length.

#### Direcciones IPv6 locales

![Direcciones IPv6 locales](https://i.imgur.com/sBxivu6.png)

-   Prefijo asignado: FE809::/10
-   Prefijo utilizado: FE80::/64
-   Alcance: solo la red directamente conectada.
-   IID: Interface Identifier, 64 LSb.
    -   Se genera usando IEEE MAC-64.
    -   Extended Unique Identifier 64, EUI-64 derivado de IEEE MAC-48, EUI-48.
    -   Se genera de forma manual.

#### Direcciones IPv6 de Site-Local

![Direcciones IPv6 de Site-Local](https://i.imgur.com/70Efer4.png)

-   Prefijo: FEC0::/10
-   Alcance: sitio u organización. Similar a las redes privadas de IPv4.
-   Dificultad de establecer límites.

#### Direcciones IPv6 de Sitio Únicas

![Direcciones IPv6 de Sitio Únicas](https://i.imgur.com/huON8vh.png)

-   Prefijo: FC00::/7, dividido en FC00::/8 y FD00::/8
-   Prefijo utilizado: FD00::/8
-   Alcance: sitio u organización.
-   Reemplazan las direcciones de Site-Local.
-   El Unique ID debe ser generado de forma pseudo aleatoria.

#### Direcciones IPv4-compat IPv6

![Direcciones IPv4-compat IPv6](https://i.imgur.com/mYTZvcu.png)

-   Desaconsejadas.
-   Se usaban para la transición
-   Se le asigna a una dirección IPv4 global única una dirección IPv6.

#### Direcciones IPv4-mapped IPv6

![Direcciones IPv4-mapped IPv6](https://i.imgur.com/irhBA6q.png)

#### Direcciones IPv6 globales

![Direcciones IPv6 globales](https://i.imgur.com/QhUzdAB.png)

-   Prefijo: cedidos por un provider.
-   Alcance: Internet. Similar a las direcciones públicas de IPv4.
-   Generación de IID:
    -   Usando IEEE MAC-64.
    -   Extended Unique Identifier 64, EUI-64 derivado de IEEE MAC-48, EUI-48 (desaconsejado para dir. estables).
    -   De forma manual.
-   Finalmente se generan con el prefijo link-local y realiza DAD (Duplicate Address Detection)

#### Direcciones IPv6 Multicast

![Direcciones IPv6 Multicast](https://i.imgur.com/RLsgMMt.png)

-   Prefijo: FF00::/8
-   Flags: permanente, temporaria. Otros reservados.
-   Alcance: 1 -> nodo local. 2 -> link local. 5 -> site local. 8 -> org. local. E -> global.
-   GID: grupo de multicast.

#### Direcciones IPv6 especiales

-   Any (sin especificar): ::0/0
-   Loopback/Localhost: ::1/128
-   Documentación: 2001:db8::/32
-   6Bone: 3FFE::/16

---

<h1 align="center">Clase 12 - 11 de junio, 2024</h1>

## Protocolos dentro de IPv6.

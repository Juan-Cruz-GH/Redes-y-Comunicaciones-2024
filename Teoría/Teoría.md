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

<center>

# Clase 3, 26 de marzo, 2024

</center>

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

<center>

# Clase 4, 9 de abril, 2024

</center>

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

<center>

# Clase 5, 16 de abril, 2024

</center>

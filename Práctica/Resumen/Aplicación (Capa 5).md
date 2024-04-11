<center>

# Capa de Aplicación (Capa 5)

</center>

## Introducción

-   La función de la Capa de Aplicación es proveer servicios para que las aplicaciones de usuario puedan comunicarse entre sí, ya sea que estén en una misma computadora o en computadoras distintas.
-   La idea es hacer la comunicación más amigable y user-friendly.
-   Su PDU es el **Dato**.

---

## HTTP

#### HTTP vs HTML

-   HTML es un **lenguaje** usado para estructurar páginas web mientras que HTTP es un **protocolo** para comunicar clientes con servidores.
-   HTTP usa por defecto el puerto 80.
-   HTTPS usa por defecto el puerto 443.

#### Comando curl

Este comando sirve para realizar peticiones de varios protocolos a una URL. Cuando el HTML de la URL tiene recursos externos, el navegador hará HTTP requests separados de forma transparente para traerlos. Parámetros:

-   -I: Realiza una solicitud HTTP usando el método HEAD, es decir que no muestra el Body recibido, solo los headers.
-   -H: Agregar headers personalizados al request del tipo clave:valor.
-   -X: Permite especificar el método HTTP (GET, PUT, POST, etc).
-   -s: Silencia el progreso y no muestra mensajes de error.
-   -v: Verboso, muestra información detallada del request, headers de la respuesta, información de la conexión, etc.

#### Versiones de HTTP

| Versión | Protocolo de información                            | Tipo de conexiones                                                                                                                                                                             | Dominios           |
| ------- | --------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------ |
| 1.0     | Texto: ASCII por ende user friendly aunque costoso. | No persistentes: Cada request involucra una nueva conexión TCP, y esa conexión solo termina una vez que el servidor contestó.                                                                  | 1 IP -> 1 dominio  |
| 1.1     | Texto: ASCII por ende user friendly aunque costoso. | Persistentes: Podemos enviar muchos requests en una misma conexión TCP de forma secuencial, y además no tenemos que esperar a recibir la respuesta del request X para enviar el request X + 1. | 1 IP -> N dominios |
| 2.0     | Binario: Difícil de leer pero muy eficiente.        | Multiplexing: Similar a 1.1 pero además los requests se realizan de forma concurrente. No hay que esperar a que uno haya terminado de enviarse para enviar el siguiente.                       | 1 IP -> N dominios |

#### Códigos HTTP

| Código | Significado           |
| ------ | --------------------- |
| 1XX    | Información de estado |
| 2XX    | Éxito                 |
| 3XX    | Redirección           |
| 4XX    | Error del Cliente     |
| 5XX    | Error del Servidor    |

#### Headers HTTP

| Header                               | Significado                                                                                                          |
| ------------------------------------ | -------------------------------------------------------------------------------------------------------------------- |
| Accept: application/json, text/html  | Tipos de datos que el cliente espera como respuesta.                                                                 |
| Content-Type: application/json       | Tipos de datos que se enviaron.                                                                                      |
| Authorization: Bearer <token>        | Credenciales de autenticación que envía el cliente.                                                                  |
| User-Agent: curl/7.88.1              | Datos del cliente que realizó el request.                                                                            |
| Location: https://abc.com/index.html | Indica la nueva URL a la que el cliente debería redireccionarse.                                                     |
| Server: Apache2                      | Datos del servidor.                                                                                                  |
| Content-Length: 50                   | Tamaño de la respuesta en bytes.                                                                                     |
| Host: www.example.com                | Especifica el dominio e interés, para evitar ambiguedades. Muy útil cuando un dominio tiene alojados más de 1 sitio. |

#### Telnet

-   Protocolo antiguo obsoleto. Es un cliente que te permite conectarte a un servidor usando un software que atiende y ejecuta los comandos en la máquina.

#### SSH

-   Permite ejecutar una conexión remota con un servidor y ejecutar comandos. La data va **cifrada**.
-   Por defecto usa el **puerto 22**.

---

## DNS

#### Definición

-   DNS (Domain Name System) es un protocolo que traduce nombres de dominio (como www.example.com) a direcciones IP, que son identificadores numéricos separados por puntos. La idea es que no tengamos que buscar sitios web por su IP ya que sería muy engorroso de recordar. Recordamos los sitios web por sus nombres de dominio (url) y DNS los traduce a IPs.
-   Por defecto usa el **puerto 53**.
-   Usa UDP.

#### Root servers

-   Son servidores DNS que están al principio de la jerarquía de servidores DNS.
-   Son los primeros servidores que los resolvers consultan.
-   Tienen información de los Top Level Domains.

#### Top Level Domains

-   Se dividen en 2 tipos: Generic y Country Code.
    -   Generic: indican el propósito general de la página. ".com" (comercial), ".org" (organización), etc.
    -   Country Code: indican de qué país es el dominio. ".uk" (Reino Unido), ".arg" (Argentina), etc.

#### Servidores autoritativos

-   Son aquellos que poseen información original y oficial de los DNS records de un dominio.
-   Siempre tienen la información más actualizada y correcta. Esto es importante ya que las cachés de otros servidores pueden tener información desactualizada de un dominio, y pueden tardar bastante en actualizarse.
-   Se dividen en 1 primario y N-1 secundarios. El primario copia su información a los secundarios rutinariamente, esto se conoce como **transferencia de zona**.

#### Resolver

-   Es un servidor DNS que resuelve todas nuestras querys DNS (tanto recursivas como iterativas). Se comunica con otros servidores DNS via Internet.
-   Cada ISP ofrece su Resolver.

#### Consultas DNS recursivas vs iterativas

-   Las consultas recursivas son aquellas que el resolver resuelve **por completo** una vez el cliente le realiza una solicitud. Si el resolver tiene la respuesta en su caché, le devuelve la respuesta al cliente rápidamente. Si no, va preguntando recursivamente servidor por servidor según lo que le responden hasta traer la respuesta autoritativa.
-   Las consultas iterativas son aquellas donde el resolver le da al cliente una respuesta según lo que tiene en su caché. Si su caché está vacía, devuelve una referencia a otro servidor DNS. El cliente es responsable por ende de hacer las subsecuentes querys según la referencia que el resolver le devolvió.
-   Si está en caché o estás en el servidor autoritativo entonces la respuesta no es recursiva, es iterativa.

#### Respuestas DNS autoritativas vs no autoritativas

-   Las respuestas autoritativas son las que retornan efectivamente la dirección IP del dominio solicitado, y esta respuesta proviene del servidor autoritativo de ese dominio, y si no, de algun otro servidor que aún posea la IP en su caché.
-   Las respuestas no autoritativas provienen de servidores DNS que no son autoritativos.

#### Delegación de dominios

-   Cuando tenemos subdominios como "example.com" - "test.example.com" hay que delegar.
-   test.example.com tendrá sus propios servidores autoritativos separados de los de example.com.
-   Se crean registros NS en el servidor autoritativo de example.com que apuntan a los servidores autoritativos de test.example.com:

```
Servidor autoritativo para el dominio example.com:
test.example.com IN NS ns1.test.example.com
ns1.test.example.com IN A 192.168.1.0
```

#### Flags DNS

| Flag | Significado                                          |
| ---- | ---------------------------------------------------- |
| aa   | Si está en 1, la respuesta fue autoritativa.         |
| rd   | Si está en 1, el cliente pidió una query recursiva.  |
| ra   | Si está en 1, el servidor soporta recursión.         |
| qr   | Si está en 1, es una respuesta. Si no, es una query. |

#### Registros DNS

| Registro | Significado                                                                                                    |
| -------- | -------------------------------------------------------------------------------------------------------------- |
| A        | Mappea un nombre de dominio a una dirección IPv4.                                                              |
| AAAA     | Mappea un nombre de dominio a una dirección IPv6.                                                              |
| PTR      | Mappea una dirección IPv4 a un nombre de dominio.                                                              |
| MX       | Servidores de email responsables de **recibir** emails para un nombre de dominio. **No enviar.**               |
| TXT      | Información del nombre de dominio.                                                                             |
| SRV      | Servicios disponibles dentro de un nombre de dominio.                                                          |
| NS       | Servidores autoritativos para un nombre de dominio.                                                            |
| CNAME    | Alias de un nombre de dominio.                                                                                 |
| SOA      | Información administrativa de una zona DNS. Sirve para configurar y sincronizar a los servers DNS secundarios. |

#### Ejemplo

![Ejemplo DNS comando dig](https://i.gyazo.com/83bd4f177d4ac91c4745d10baab91221.png)

---

## SMTP

#### Definición

-   Simple Mail Transfer Protocol es un protocolo que se usa cuando un cliente envía un email a su mail server.
-   También se usa cuando un servidor de mail le manda un email a otro servidor de mail. Esto es porque SMTP puede actuar como cliente o como servidor.
-   Por defecto usa el **puerto 25**.
-   Usa TCP.

#### POP3

-   Protocolo simple, cuando el usuario quiere acceder a sus emails los descarga y estos se borran del servidor.
-   No guarda estado, por ende es eficiente.
-   Por defecto usa el **puerto 110**.
-   Usa TCP.

#### IMAP

-   Protocolo más complejo que permite al usuario leer, borrar, y organizar sus emails en carpetas como si fuera un filesystem.
-   Guarda estado, por ende es menos eficiente.
-   Por defecto usa el **puerto 143**.
-   Usa TCP.

#### Encoding de imágenes

-   Tanto SMTP, POP3 e IMAP necesitan encodificar las imágenes para transformarlas a texto al enviar o recibir un email que las contenga, ya que así trabajan, con contenido de texto plano únicamente.

#### MUA, MSA, MTA, MDA

-   MUA o Mail User Agent es el cliente o programa user friendly que usan los clientes para enviar o recibir emails. Por ejemplo Outlook o Gmail.
-   MSA o Mail Submission Agent es el responsable de enviar emails salientes de los clientes hacia el MTA.
-   MTA o Mail Transfer Agent es el responsable de enviar emails de un servidor de email a otro, es decir a otro MTA.
-   MDA o Mail Delivery Agent es el responsable de recibir emails de los clientes.
-   De MUA a MSA tenemos una sola conexión. De MTA a MTA depende a qué dominio van los emails. Si 2 emails van al mismo dominio pueden ir en una sola conexión TCP.

#### Ejemplo simple

-   Si **a@test.com envía un email a b@gmail.com**, el MTA de test.com se encarga de consultar por el registro MX de gmail.com (agarra el de mayor prioridad es decir menor número) y una ves que lo obtiene consulta por la IP (registro A) de ese dominio obtenido. **Lo hace el MTA, no el Resolver.**

    <img src="https://inquest.net/wp-content/uploads/how_email_works.png" alt="Diagrama MUA, MSA, MTA, MDA" width="1000" height="350"/>

---

## FTP

-   Protocolo que permite transferir archivos.
-   Necesita 2 conexiones TCP: una de **control** y otra de **datos**.
-   Tiene 2 modos, Pasivo y Activo:
    -   Pasivo: el cliente inicia tanto la conexión de control como la de datos (la de datos en un puerto que el servidor le indica).
    -   Activo: el cliente inicia solo la conexión de control en el puerto y el servidor inicia la conexión de datos en un puerto al azar.

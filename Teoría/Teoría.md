# Recursos

<center>

# Clase 1

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

...

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

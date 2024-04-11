<center>

# Capa de Transporte (Capa 4)

</center>

## Introducción

-   Se encarga de comunicar procesos de la capa de aplicación.
-   Puede implementarse con TCP o UDP.

---

## Puertos

-   Son números de 0 a 65,535 que identifican a un proceso determinado de una aplicación de ambos lados de la comunicación TCP o UDP.
-   En TCP los puertos 0 a 1023 están reservados y no se pueden usar como emisor de una conexión TCP, a no ser que se use el **sudo**.
-   Una comunicación se identifica (clave primaria) por:
    -   IP origen
    -   IP destino
    -   Puerto origen
    -   Puerto destino
    -   Protocolo (TCP o UDP)
        Es decir, no podría nunca tener 2 conexiones con la misma IP, mismo puerto y mismo protocolo.

---

## Comando ss

-   El comando ss muestra información de todas las conexiones TCP y UDP actuales en la máquina.

```bash
ss -l # Sockets que están escuchando.
ss -a # Todos los sockets.
ss -t # Sockets TCP.
ss -u # Sockets UDP.
```

![Ejemplo output ss](https://github.com/guadaevequoz/RyC-2022/raw/master/img/tp5-ej14.png)

-   Hay 8 conexiones establecidas, a pesar de que hayan 9 rows con state ESTAB. Esto es porque 127.0.0.1:22 con 127.0.0.1:41220 aparece 2 veces.
-   El asterisco \* en la IP significa que una vez se estableza la conexión puede elegir cualquier IP. Lo mismo el asterisco en los puertos.
-   En general, los que están estado LISTEN son servidores y los que fueron iniciados desde el localhost son clientes.
-   Podemos inferir qué protocolos está usando cada conexión en base a los puertos. Por ejemplo:
    -   :443 -> HTTPS
    -   :53 -> DNS
    -   :22 -> SSH
    -   :25 -> SMTP

---

## Unicast, Multicast, Broadcast, Anycast

-   Unicast significa que UN emisor envia un mensaje a UN receptor.
-   Multicast significa que UN emisor envia un mensaje a MUCHOS receptores a la vez.
-   Broadcast significa que UN emisor envia un mensaje a TODOS los receptores de la red a la vez.
-   Anycast significa que una misma IP se asigna a muchos dispositivos y un mensaje se envia al más cercano de estos dispositivos.

---

## TCP vs UDP

|                                                | TCP                                                                                                        | UDP                                                                                               |
| ---------------------------------------------- | ---------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------- |
| Confiabilidad                                  | Es confiable, garantiza que la información llegue completa y en orden.                                     | No es confiable, no garantiza que los datos llegan y si llegan no garantiza que lleguen en orden. |
| Multiplexación                                 | La provee por medio del establecimiento de conexión y de los puertos.                                      | La provee por medio de los puertos.                                                               |
| Orientación a la conexión                      | Es orientado a la conexión, siempre la establece mediante S3V.                                             | No establece conexión.                                                                            |
| Control de congestión                          | Lo implementa de forma adaptativa y dinámica.                                                              | No lo implementa.                                                                                 |
| Uso de puertos                                 | Socket = definido por cada lado de la conexión. Por cada Socket solo 2 procesos pueden intercambiar datos. | Socket = N° port destino + IP destino. Por un mismo Socket muchos clientes pueden enviar datos.   |
| Eficiencia                                     | Gran overhead debido a su complejidad.                                                                     | Muy simple, eficiente y rápido (poca latencia).                                                   |
| PDU                                            | Segmento.                                                                                                  | Datagrama.                                                                                        |
| Broadcast                                      | No lo soporta, seria muy ineficiente.                                                                      | Lo soporta.                                                                                       |
| Multicast                                      | No lo soporta, sería muy ineficiente.                                                                      | Lo soporta.                                                                                       |
| No hay proceso escuchando en el puerto elegido | Responde con un segmento vacío con RST = 1.                                                                | Responde con un datagrama especial ICMP Port Unreachable.                                         |

---

## TCP

#### Segmento TCP

![Segmento TCP](https://1.bp.blogspot.com/-oE1QQcPLscU/Xz1n9yemnoI/AAAAAAAACfM/iS8OVSArtZsPEQ4s_jozoZox1zceqCnCQCLcBGAsYHQ/s640/tcpheader.jpg)

-   Source Port: N° de puerto de la aplicación origen.
-   Destination Port: N° de puerto de la aplicación destino.
-   Sequence Number: N° de byte en el segmento actual.
-   Acknowledgement Number: Siguiente Sequence Number.
-   Header Length: Longitud total del segmento, header + options + data.
-   Flags:
    -   URG: el Urgent Pointer debe ser priorizado.
    -   ACK: A recibió correctamente el último dato que B le envió.
    -   PSH: los datos del segmento deben entregarse de inmediato al receptor, sin perder tiempo esperando que se almacenen en el buffer.
    -   RST: terminar la conexión cuando aparece un problema. Si se recibe al intentar iniciar una conexión significa que no hay proceso del host receptor escuchando en el puerto indicado.
    -   SYN: iniciar una conexión TCP, sincronizando los Sequence Numbers iniciales entre remitente y receptor.
    -   FIN: la parte que envía el segmento terminó el envío y quiere cerrar la conexión. Cuando la parte opuesta también envía FIN se cierra la conexión.
-   Checksum: valor computado a partir de la data que el receptor también computa para asegurar que el segmento llegó entero.
-   Urgent Pointer: dice dónde empieza (offset) una parte de la data que requiere atención inmediata.
-   Options: opciones optativas de control y manejo de la conexión.
    -   Maximum Segment Size: tamaño máximo de segmento que puede recibir una de las partes.
-   Data: los datos en cuestión a enviar.

#### Saludo de 3 vías (3 Way Handshake) (SYN > SYN + ACK > ACK)

Es el proceso a través del cual se establece una conexión TCP entre 2 partes. Consiste de 3 pasos como su nombre lo indica:

1. El emisor envía un segmento sin datos, con Sequence Number aleatorio y con flag SYN = 1 al receptor.
2. El receptor responde con un segmento sin datos, con Sequence Number aleatorio, y con flags SYN = 1 y ACK = 1. Además, en su Acknowledgement Number pone el SN aleatorio que le envió el emisor.
3. El emisor responde con un segmento sin datos y con flags ACK = 1 y SYN = 0, indicando que la conexión fue establecida correctamente.

Nota: Si luego del paso 1 el proceso receptor (IP + puerto que el emisor eligió) no está escuchando, se recibe un segmento vacío con flag RST = 1.

#### Tipos de estado TCP

| Estado       | Significado                                                                                           |
| ------------ | ----------------------------------------------------------------------------------------------------- |
| LISTEN       | Se está esperando una solicitud de conexión en un puerto determinado.                                 |
| SYN-SENT     | El cliente envió la solicitud de conexión.                                                            |
| SYN-RECEIVED | El servidor recibió la solicitud y envió SYN = 1.                                                     |
| ESTABLISHED  | Conexión establecidad, ambos pueden enviarse datos.                                                   |
| FIN-WAIT-1   | Una de las partes envió un segmento con FIN = 1 y espera un ACK de la otra parte.                     |
| FIN-WAIT-2   | La otra parte recibió ese ACK.                                                                        |
| CLOSE-WAIT   | La otra parte puede seguir enviando datos hasta recibir un FIN = 1.                                   |
| CLOSING      | La otra parte recibe un FIN = 1, ya no puede enviar datos.                                            |
| LAST-ACK     | La otra parte espera el último ACK.                                                                   |
| TIME-WAIT    | La otra parte (que ya había cerrado la conexión) responde con ACK y espera que el servidor lo reciba. |
| CLOSED       | La conexión se cerro de ambos lados.                                                                  |

#### Control de flujo

-   Consiste en que cada una de las partes le envie a la otra solo tanto como la otra parte pueda manejar, para evitar desbordarlo.
-   No se puede desactivar, está siempre en TCP.
-   Depende del punto de saturación que define cada una de las partes. La idea es intentar no desbordar el buffer del receptor.
-   Se logra usando una Ventana de Recepción (Window Size).

##### Selective Repeat

-   Solo los segmentos erróneos o que se perdieron se retransmitirán. Un tamaño de ventana = N significa que el emisor puede enviar como máximo N segmentos seguidos sin esperar respuesta.
-   El tamaño de ventana debe ser <= (cantidad total de segmentos enviados / 2)
-   Ejemplo: ???

#### Control de congestión

-   El control de congestión lo activa el emisor cuando hay mucha pérdida de paquetes o el RTT empieza a aumentar o se recibe una Notificación Explícita de Congestión.
-   Intenta no desbordar a la red en su totalidad.
-   Slow start: se empieza con una ventana de congestión pequeña y va aumentando exponencialmente cada vez que se recibe un ACK hasta el momento en que se detecta congestión. Ahí, se pasa a Congestion-Avoidance.
-   Congestion-Avoidance: se reinicia la ventana de congestión y ahora crece linealmente en vez de exponencialmente.

#### Ejemplo conexión TCP exitosa

![Ejemplo conexión TCP](https://github.com/bautimercado/Redes-y-Comunicaciones-2023/raw/master/Practicas/Practica%206/img/clipboard3.png)

-   Seq = 0 (3 way handshake)
-   Seq = 0, ACK = 1 (3 way handshake)
-   Seq = **1**, ACK = 1 (3 way handshake)
-   Seq = 1, ACK = 1
-   Seq = 1, ACK = **8** (Seq 1 + len 7 = 8)
-   Seq = 8, ACK = 1
-   Seq = 1, ACK = **17** (Seq 8 + len 9 = 17)
-   Seq = 17, ACK = 1
-   Seq = 1, ACK = **22** (Seq 17 + len 5 = 22)
-   Seq = **22**, ACK = 1
-   Seq = 1, ACK = **23** (Seq 22 + len 1 = 23)
-   Seq = **23**, ACK = 2

#### tcpdump

-   Comando que muestra los paquetes enviados entre tu PC y otros hosts.
-   Ejemplo:

    ![Ejemplo tcpdump](https://i.gyazo.com/ee75a03fd660c2e2efebb7024db6aecf.png)

---

## Datagrama UDP

![Datagrama UDP](https://cheapsslsecurity.com/blog/wp-content/uploads/2022/06/udp-header.png)

-   El checksum es opcional, no obligatorio como en el segmento TCP.
-   Length tiene un tamaño máximo más pequeño que en el segmento TCP.
-   A diferencia de TCP, el source port puede ser sarasa, en caso de que no nos interese recibir una respuesta luego de enviar un datagrama.

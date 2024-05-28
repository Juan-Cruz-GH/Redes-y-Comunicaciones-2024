<h1 align="center">Práctica 6 - Capa de Transporte II</h1>

### 1. ¿Cuál es el puerto por defecto que se utiliza en los siguientes servicios? Web / SSH / DNS / Web Seguro / POP3 / IMAP / SMTP. Investigue en qué lugar en Linux y en Windows está descrita la asociación utilizada por defecto para cada servicio.

| Servicio           | Puerto default |
| ------------------ | -------------- |
| Web (HTTP)         | 80             |
| SSH                | 22             |
| DNS                | 53             |
| Web seguro (HTTPS) | 443            |
| POP3               | 110            |
| IMAP               | 143            |
| SMTP               | 25             |

En Linux el lugar donde se describe la asociación se encuentra en /etc/services.

En Windows se encuentra en C:\Windows\system32\drivers\etc\services

### 2. Investigue qué es multicast. ¿Sobre cuál de los protocolos de capa de transporte funciona? ¿Se podría adaptar para que funcione sobre el otro protocolo de capa de transporte? ¿Por qué?

Multicast es un método de comunicación que permite enviar datos desde un origen a varios destinos previamente elegidos, de forma simultánea.

Funciona sobre el protocolo UDP.

TCP no soporta multicast de forma nativa, ya que TCP trabaja vía conexiones 1 a 1, distinto a la naturaleza 1 a N de multicast.

Técnicamente se podría adaptar TCP de ciertas maneras para utilizar multicast, pero sería ineficiente y complejo.

### 3. Investigue cómo funciona el protocolo de aplicación FTP teniendo en cuenta las diferencias en su funcionamiento cuando se utiliza el modo activo de cuando se utiliza el modo pasivo ¿En qué se diferencian estos tipos de comunicaciones del resto de los protocolos de aplicación vistos?

El protocolo de aplicación FTP se utiliza para transferir archivos entre hosts y funciona mediante 2 conexiones, una de control y otra de datos. Este protocolo posee 2 modos, Activo y Pasivo. La diferencia entre estos modos se encuentra en qué ocurre con la conexión de datos.

En ambos modos, la conexión de control es por donde viajan los comandos y el estado de cada operación. Utiliza el puerto 21 y siempre la inicia el cliente.

En el modo Activo, la conexión de datos la **inicia el servidor** vía su puerto 20 hacia el cliente vía uno de sus puertos no privilegiados.

En el modo Pasivo, la conexión de datos la **inicia el cliente** vía uno de sus puertos no privilegiados hacia el servidor vía uno de sus puertos no privilegiados.

FTP se diferencia del resto de protocolos de aplicación en que necesita 2 conexiones, una de control y una de datos, en vez de solo una.

### 4. Suponiendo Selective Repeat; tamaño de ventana 4 y sabiendo que E indica que el mensaje llegó con errores. Indique en el siguiente gráfico, la numeración de los ACK que el host B envía al Host A.

![Selective Repeat](https://i.imgur.com/Fj4zfng.png)

En Selective Repeat, solo los segmentos perdidos o erróneos son retransmitidos. Que el tamaño de ventana sea 4 significa que host A puede enviar hasta 4 segmentos sin esperar ACKs. Una vez envió el cuarto, debe esperar al menos un ACK.

Host B envía, de arriba hacia abajo, ACKs 0 - 1 - 3 - 4 - 5. Luego de esto ocurre un timeout del lado de host A del segmento 2, ya que no se recibió un ACK de ese segmento pasado un determinado tiempo. Entonces host A retransmite ese segmento, y luego host B le envía ACK 2.

### 5. ¿Qué restricción existe sobre el tamaño de ventanas en el protocolo Selective Repeat?

En Selective Repeat, el tamaño de ventana debe ser:

```
Tamaño de ventana <= (cantidad total de segmentos enviados / 2)
```

Esta restricción evita ambiguedades si se acaba el espacio de bits.

### 6. De acuerdo a la captura TCP de la siguiente figura, indique los valores de los campos borroneados.

![Captura TCP](https://i.imgur.com/z6bVqHx.png)

La captura TCP parece ser un saludo de 3 vías típico.

Primer línea:

```
[SYN] | Seq=3933822137 // Uno menos que el ACK de la siguiente línea.
```

Tercer línea:

```
172.20.1.1 | 172.20.1.100 | 41749 > vce | [ACK] | Seq=3933822138 | ACK=1047471502
```

### 7. Dada la sesión TCP de la figura, completar los valores marcados con un signo de interrogación.

![Conexión TCP](https://i.imgur.com/wH0gJhl.png)

| Seq | ACK |
| --- | --- |
| 0   | -   |
| 0   | 1   |
| 1   | 1   |
| 1   | 1   |
| 1   | 8   |
| 8   | 1   |
| 1   | 17  |
| 17  | 1   |
| 1   | 22  |
| 22  | 1   |
| 1   | 23  |
| 23  | 2   |

### 8. ¿Qué es el RTT y cómo se calcula? Investigue la opción TCP timestamp y los campos TSval y TSecr.

El RTT o Round Time Trip es el tiempo que tarda un segmento en ir y volver. Es igual a:

```
(Tiempo de recepción en el emisor) - (Tiempo de envío en el emisor)
```

La opción TCP timestamp permite que los hosts puedan calcular el RTT.

El campo TSval es el tiempo de envío en el emisor y el campo TSecr es el tiempo de recepción en el emisor.

### 9. Para la captura tcp-captura.pcap, responder las siguientes preguntas.

##### a. ¿Cuántos intentos de conexiones TCP hay?

Los intentos de conexión los podemos identificar porque el emisor envía un SYN y recibe un RST o un SYN + ACK.

1. Lineas 3 4 y 5 (conexión exitosa).
2. Lineas 961 y 962 (conexión rechazada).
3. Lineas 963 y 964 (conexión rechazada).
4. Lineas 967, 968 y 969 (conexión exitosa).
5. Lineas 981, 982 y 983 (conexión exitosa).
6. Lineas 1106, 1107 y 1108 (conexión exitosa).

##### b. ¿Cuáles son la fuente y el destino (IP:port) para c/u?

La fuente es 10.0.2.10:46907 y el destino 10.0.4.10:5001.

##### c. ¿Cuántas conexiones TCP exitosas hay en la captura? ¿Cómo diferencia las exitosas de las que no lo son? ¿Cuáles flags encuentra en cada una?

Como mencioné en el primer inciso, hay 4 conexiones exitosas y 2 fallidas.

Las conexiones exitosas se diferencian ya que poseen los 3 flags del saludo de 3 vías, SYN -> SYN + ACK -> ACK.

##### d. Dada la primera conexión exitosa responder:

###### i. ¿Quién inicia la conexión?

10.0.2.10:46907 inicia la conexión.

###### ii. ¿Quién es el servidor y quién el cliente?

El servidor es 10.0.4.10 y el cliente es 10.0.2.10.

###### iii. ¿En qué segmentos se ve el 3-way handshake?

En los primeros 3 segmentos.

###### iv. ¿Cuáles ISNs se intercambian?

El cliente utiliza el ISN **2218428254** mientras que el servidor utiliza el ISN **1292618479**.

###### v. ¿Cuál MSS se negoció?

Se negoció el MSS 1460.

###### vi. ¿Cuál de los dos hosts envía la mayor cantidad de datos (IP:port)?

El cliente (10.0.2.10:46907).

##### e. Identificar primer segmento de datos (origen, destino, tiempo, número de fila y número de secuencia TCP).

-   Origen: 10.0.2.10:46907
-   Destino: 10.0.4.10:5001
-   Tiempo: 0.151826
-   Número de fila: 6
-   Número de secuencia: 1

###### i. ¿Cuántos datos lleva?

Lleva 24 bytes de datos.

###### ii. ¿Cuándo es confirmado (tiempo, número de fila y número de secuencia TCP)?

-   Tiempo: 0.151925
-   Número de fila: 7
-   Número de secuencia: 1

###### iii. La confirmación, ¿qué cantidad de bytes confirma?

Confirma 25 bytes.

##### f. ¿Quién inicia el cierre de la conexión? ¿Qué flags se utilizan? ¿En cuáles segmentos se ve (tiempo, número de fila y número de secuencia TCP)?

El cliente es quien inicia el cierre de la conexión utilizando los flags FIN PSH y ACK.

Se ve en el segmento 958.

-   Tiempo: 75.090196
-   Número de fila: 958
-   Número de secuencia: 786289

### 10. Responda las siguientes preguntas respecto del mecanismo de control de flujo.

##### a. ¿Quién lo activa? ¿De qué forma lo hace?

El control de flujo lo activa el **receptor**.

Lo hace utilizando una Ventana de Recepción, la cual indica que el emisor solo podrá enviarle como máximo lo que indique esa ventana por cada segmento. Cuando el buffer del receptor se llena, su VR pasa a valer 0.

##### b. ¿Qué problema resuelve?

Resuelve el problema de que el emisor abrume al receptor enviandole datos que aún no puede procesar.

##### c. ¿Cuánto tiempo dura activo y qué situación lo desactiva?

El control de flujo dura activo hasta que se vacíe el buffer de recepción. También se puede ajustar la ventana de recepción dinámicamente.

### 11. Responda las siguientes preguntas respecto del mecanismo de control de congestión.

##### a. ¿Quién activa el mecanismo de control de congestión? ¿Cuáles son los posibles disparadores?

El control de congestión es activado por el **emisor**.

Se puede activar por:

1. Detección de mucha pérdida de paquetes.
2. El RTT empieza a aumentar.
3. ECN (Explicit Congestion Notification)

##### b. ¿Qué problema resuelve?

Resuelve el problema de que la red se sature.

##### c. Diferencie slow start de congestion-avoidance.

**Slow Start:** En una nueva conexión TCP se empieza con una ventana de congestión pequeña que se duplica exponencialmente cada vez que se recibe un ACK hasta que se detecta congestión en cuyo momento se reinicia la VC y se pasa a la fase de Congestion Avoidance.

**Congestion Avoidance:** La ventana de congestión crece linealmente.

### 12. Para la captura udp-captura.pcap, responder las siguientes preguntas.

##### a. ¿Cuántas comunicaciones (srcIP,srcPort,dstIP,dstPort) UDP hay en la captura?

1. 10.0.2.10:0 -> 10.0.30.10:8003.
2. 10.0.2.10:9004 -> 10.0.3.10:9045.
3. 10.0.2.10:9004 -> 1.1.1.1:9045.
4. 10.0.2.10:53300 -> 10.0.4.10:9045.
5. 10.0.2.10:59053 -> 10.0.4.10:8003.
6. 10.0.2.10:8003 -> 10.0.4.10:8003.

##### b. ¿Cómo se podrían identificar las exitosas de las que no lo son?

Podemos identificar las exitosas ya que **no** reciben un mensaje ICMP. Las fallidas si.

##### c. ¿UDP puede utilizar el modelo cliente/servidor?

Si, UDP puede utilizar este modelo.

##### d. ¿Qué servicios o aplicaciones suelen utilizar este protocolo?¿Qué requerimientos tienen?

Los servicios o aplicaciones que suelen utilizar UDP son aquellos que requieren un overhead mínimo y máxima performance, y no les preocupa mucho la confiabilidad de los datos que se envían (no es un gran problema si se pierden o corrompen).

##### e. ¿Qué hace el protocolo UDP en relación al control de errores?

UDP posee un control de errores muy básico. Lo único que ofrece en este sentido es el checksum.

Si el checksum calculado en el receptor no matchea con el checksum que le envía el emisor en el header, puede descartar ese datagrama ya que probablemente esté corrupto.

##### f. Con respecto a los puertos vistos en las capturas, ¿observa algo particular que lo diferencie de TCP?

En UDP el puerto origen puede ser 0 ya que a diferencia de TCP, en UDP no se requiere recibir una respuesta.

##### g. Dada la primera comunicación en la cual se ven datos en ambos sentidos (identificar el primer datagrama):

La primera comunicación en la cual se ven datos en ambos sentidos ocurre en las lineas 79 y 82 entre 10.0.2.10 y 10.0.3.10.

###### i. ¿Cuál es la dirección IP que envía el primer datagrama?,¿desde cuál puerto?

La dirección IP que envía el primer datagrama es 10.0.2.10 desde el puerto 9004.

###### ii. ¿Cuántos datos se envían en un sentido y en el otro?

10.0.2.10 envía primero 4 bytes y luego 5.
10.0.3.10 envía primero 7 bytes y luego 5.

### 13. Dada la salida que se muestra en la imagen, responda los ítems debajo.

![Salida comando ss](https://i.imgur.com/egjqvfM.png)

● Suponga que ejecuta los siguientes comandos desde un host con la IP 10.100.25.90. Responda qué devuelve la ejecución de los siguientes comandos y, en caso que corresponda, especifique los flags.

##### a. hping3 -p 3306 –udp 10.100.25.135

Como 10.100.25.135 no está escuchando tráfico UDP en el puerto 3306, se recibe un ICMP Port Unreachable.

##### b. hping3 -S -p 25 10.100.25.135

Como 10.100.25.135 no está en estado LISTEN para su puerto 25 (no está escuchando TCP en ese puerto), se recibe un segmento con flag RST activado, rechazando el intento de establecimiento de conexión.

##### c. hping3 -S -p 22 10.100.25.135

Como TCP permite multiplexación de puertos, a pesar de que 10.100.25.135 ya está utilizando su puerto 25 con otro host, puede utilizar ese mismo puerto para comunicarse con otro host sin problema, por ende se recibe un segmento SYN + ACK iniciando el establecimiento de conexión.

##### d. hping3 -S -p 110 10.100.25.135

Como 10.100.25.135 no está en estado LISTEN para su puerto 25 (no está escuchando TCP en ese puerto), se recibe un segmento con flag RST activado, rechazando el intento de establecimiento de conexión.

● ¿Cuántas conexiones distintas hay establecidas? Justifique.

Hay 5 lineas con estado LISTEN, pero solo 3 conexiones establecidas, ya que 2 de ellas están duplicadas.

<center>

# Práctica 6 - Capa de Transporte II

</center>

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

##### b. ¿Cuáles son la fuente y el destino (IP:port) para c/u?

##### c. ¿Cuántas conexiones TCP exitosas hay en la captura? ¿Cómo diferencia las exitosas de las que no lo son? ¿Cuáles flags encuentra en cada una?

##### d. Dada la primera conexión exitosa responder:

###### i. ¿Quién inicia la conexión?

###### ii. ¿Quién es el servidor y quién el cliente?

###### iii. ¿En qué segmentos se ve el 3-way handshake?

###### iv. ¿Cuáles ISNs se intercambian?

###### v. ¿Cuál MSS se negoció?

###### vi. ¿Cuál de los dos hosts envía la mayor cantidad de datos (IP:port)?

##### e. Identificar primer segmento de datos (origen, destino, tiempo, número de fila y número de secuencia TCP).

###### i. ¿Cuántos datos lleva?

###### ii. ¿Cuándo es confirmado (tiempo, número de fila y número de secuencia TCP)?

###### iii. La confirmación, ¿qué cantidad de bytes confirma?

##### f. ¿Quién inicia el cierre de la conexión? ¿Qué flags se utilizan? ¿En cuáles segmentos se ve (tiempo, número de fila y número de secuencia TCP)?

### 10. Responda las siguientes preguntas respecto del mecanismo de control de flujo.

##### a. ¿Quién lo activa? ¿De qué forma lo hace?

##### b. ¿Qué problema resuelve?

##### c. ¿Cuánto tiempo dura activo y qué situación lo desactiva?

### 11. Responda las siguientes preguntas respecto del mecanismo de control de congestión.

##### a. ¿Quién activa el mecanismo de control de congestión? ¿Cuáles son los posibles disparadores?

##### b. ¿Qué problema resuelve?

##### c. Diferencie slow start de congestion-avoidance.

### 12. Para la captura udp-captura.pcap, responder las siguientes preguntas.

##### a. ¿Cuántas comunicaciones (srcIP,srcPort,dstIP,dstPort) UDP hay en la captura?

##### b. ¿Cómo se podrían identificar las exitosas de las que no lo son?

##### c. ¿UDP puede utilizar el modelo cliente/servidor?

##### d. ¿Qué servicios o aplicaciones suelen utilizar este protocolo?¿Qué requerimientos tienen?

##### e. ¿Qué hace el protocolo UDP en relación al control de errores?

##### f. Con respecto a los puertos vistos en las capturas, ¿observa algo particular que lo diferencie de TCP?

##### g. Dada la primera comunicación en la cual se ven datos en ambos sentidos (identificar el primer datagrama):

###### i. ¿Cuál es la dirección IP que envía el primer datagrama?,¿desde cuál puerto?

###### ii. ¿Cuántos datos se envían en un sentido y en el otro?

### 13. Dada la salida que se muestra en la imagen, responda los ítems debajo.

![Salida comando ss](https://i.imgur.com/egjqvfM.png)

● Suponga que ejecuta los siguientes comandos desde un host con la IP 10.100.25.90. Responda qué devuelve la ejecución de los siguientes comandos y, en caso que corresponda, especifique los flags.

##### a. hping3 -p 3306 –udp 10.100.25.135

##### b. hping3 -S -p 25 10.100.25.135

##### c. hping3 -S -p 22 10.100.25.135

##### d. hping3 -S -p 110 10.100.25.135

● ¿Cuántas conexiones distintas hay establecidas? Justifique.

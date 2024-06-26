<h1 align="center">Práctica 5 - Capa de Transporte I</h1>

### 1. ¿Cuál es la función de la capa de transporte?

La Capa de Transporte se encarga de llevar los datos de la Capa de Aplicación a la Capa de Red. Es responsable de segmentar y reconstruir la información enviada, controlar el flujo de transmisión, detectar y corregir errores, multiplexar y desmultiplexar streams vía puertos, y asegurar que la información llegue ordenada y completa. Comunica **PROCESOS**.

### 2. Describa la estructura del segmento TCP y UDP.

**Segmento TCP:**

| Campo              | Uso                                                                                                            |
| ------------------ | -------------------------------------------------------------------------------------------------------------- |
| Puerto origen      | N° de puerto de la aplicación origen                                                                           |
| Puerto destino     | N° de puerto de la aplicación destino                                                                          |
| N° secuencia       | N° de byte en el segmento actual                                                                               |
| N° acknowledgement | Siguiente Sequence Number                                                                                      |
| Header de longitud | Longitud total del segmento, header + options + data                                                           |
| Flags              | Existen varios tipos con distintos usos                                                                        |
| Checksum           | Valor computado a partir de la data que el receptor también computa para asegurar que el segmento llegó entero |
| Urgent Pointer     | Dice dónde empieza (offset) una parte de la data que requiere atención inmediata                               |
| Options            | Opciones optativas de control y manejo de la conexión                                                          |
| Datos              | Los datos en cuestión a enviar                                                                                 |

**Datagrama UDP:**

Posee puerto origen, puerto destino, longitud, checksum y data, nada más.

### 3. ¿Cuál es el objetivo del uso de puertos en el modelo TCP/IP?

El objetivo de los puertos es identificar unívocamente al proceso/servicio/aplicación del dispositivo con el cual se quiere comunicar y así poder entablar muchas conexiones simultáneamente en un mismo dispositivo (multiplexación). Si no tuvieramos puertos, una PC podría establecer una sola conexión TCP o UDP con otra PC, en una sola aplicación.

### 4. Compare TCP y UDP en cuanto a:

##### a. Confiabilidad

##### b. Multiplexación.

##### c. Orientado a la conexión.

##### d. Controles de congestión.

##### e. Utilización de puertos.

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

### 5. La PDU de la capa de transporte es el segmento. Sin embargo, en algunos contextos suele utilizarse el término datagrama. Indique cuando.

El PDU de TCP es el segmento.

Sin embargo, el PDU de UDP suele llamarse Datagrama, aunque esto puede resultar confuso ya que el PDU de la Capa de Red (IP) también se llama Datagrama.

### 6. Describa el saludo de tres vías de TCP. ¿UDP tiene esta característica?

El saludo de 3 vías es la forma en que se establece una conexión TCP. Se llama de 3 vías porque posee 3 pasos: Cliente → Servidor, Servidor → Cliente, Cliente → Servidor.

1. El cliente envía un segmento **sin datos** al servidor, con **flag SYN activado** y un **número aleatorio en el campo Sequence Number**.
2. El servidor (el cual está en estado LISTEN en el puerto por el cuál el cliente le habló) responde con otro segmento **sin datos**, con **flag SYN y flag ACK activados**, con otro **número aleatorio en el campo Sequence Number** y finalmente en el campo **Acknowledgment Number pone el Sequence Number que el cliente le había enviado + 1**.
3. El cliente responde con otro segmento **sin datos**, con **flag ACK activado**, con **Sequence Number el primero que había enviado + 1**, y con **Acknowledgement Number el Sequence Number del servidor + 1**.

UDP no posee esta característica ya que no establece conexión.

### 7. Investigue qué es el ISN (Initial Sequence Number). Relaciónelo con el saludo de tres vías.

El ISN es el Sequence Number inicial y aleatorio que cada parte, de forma separada, elige. Es único para cada parte y sirve para hacerle saber a la otra parte cuál es el siguiente segmento que debe enviarle y así mantener el orden.

### 8. Investigue qué es el MSS. ¿Cuándo y cómo se negocia?

El Maximum Segment Size (MSS) es una de las opciones del campo Options que indica el tamaño máximo de segmento que puede albergar la parte de la conexión que envió ese segmento (ya sea el cliente, el servidor o ambos).

### 9. Utilice el comando ss (reemplazo de netstat) para obtener la siguiente información de su PC:

##### a. Para listar las comunicaciones TCP establecidas.

```
ss -t
```

##### b. Para listar las comunicaciones UDP establecidas.

```
ss -u
```

##### c. Obtener sólo los servicios TCP que están esperando comunicaciones

```
ss -t -l
```

##### d. Obtener sólo los servicios UDP que están esperando comunicaciones.

```
ss -u -l
```

##### e. Repetir los anteriores para visualizar el proceso del sistema asociado a la conexión.

##### f. Obtenga la misma información planteada en los items anteriores usando el comando netstat.

Encuentro que **_netstat_** muestra también los sockets con estado TIME_WAIT.

### 10. ¿Qué sucede si llega un segmento TCP con el flag SYN activo a un host que no tiene ningún proceso esperando en el puerto destino de dicho segmento (es decir, el puerto destino no está en estado LISTEN)?

##### a. Utilice hping3 para enviar paquetes TCP al puerto destino 22 de la máquina virtual con el flag SYN activado.

```
sudo hping3 -S -p 22 127.0.0.1
```

##### b. Utilice hping3 para enviar paquetes TCP al puerto destino 40 de la máquina virtual con el flag SYN activado.

```
sudo hping3 -S -p 40 127.0.0.1
```

##### c. ¿Qué diferencias nota en las respuestas obtenidas en los dos casos anteriores? ¿Puede explicar a qué se debe? (Ayuda: utilice el comando ss visto anteriormente).

En el primer caso recibimos flags SA es decir SYN + ACK, indicando que se está estableciendo la conexión.

En el segundo caso recibimos flags RA es decir RST + ACK, indicando que no se está escuchando en ese puerto (40).

Usando el comando ss, podemos ver claramente que hay un Local Address con puerto 22 en estado LISTEN, pero no hay ninguno con puerto 40 en este estado.

Resumiendo, si un host recibe un segmento TCP con SYN = 1 en un puerto donde no está escuchando, le devolverá un segmento vacío con flag RST = 1.

### 11. ¿Qué sucede si llega un datagrama UDP a un host que no tiene ningún proceso esperando en el puerto destino de dicho datagrama (es decir, que dicho puerto no está en estado LISTEN)?

##### a. Utilice hping3 para enviar datagramas UDP al puerto destino 5353 de la máquina virtual.

```
sudo hping3 --udp -p 5353 127.0.0.1
```

##### b. Utilice hping3 para enviar datagramas UDP al puerto destino 40 de la máquina virtual.

```
sudo hping3 --udp -p 40 127.0.0.1
```

##### c. ¿Qué diferencias nota en las respuestas obtenidas en los dos casos anteriores?¿Puede explicar a qué se debe? (Ayuda: utilice el comando ss visto anteriormente).

En el primer caso vemos un mensaje de que el datagrama se envió correctamente.

En el segundo caso recibimos ICMP Port Unreachable.

Usando el comando ss, podemos ver claramente que hay un Local Address con puerto 5353 en estado UNCONN, pero no hay ninguno con puerto 40 en este estado. El estado UNCONN sería similar al LISTEN de TCP.

Resumiendo, si llega un datagrama UDP a un host que no está escuchando tráfico UDP en ese puerto, entonces se devolverá un datagrama UDP especial del protocolo ICMP, "Port Unreachable".

### 12. [Investigue los distintos tipos de estado que puede tener una conexión TCP.](https://users.cs.northwestern.edu/~agupta/cs340/project2/TCPIP_State_Transition_Diagram.pdf)

Una conexión TCP atraviesa multiples estados durante su existencia. El último estado de la siguiente tabla es ficticio, es decir que no existe como tal pero sirve para aprender.

| Estado       | Significado                                                                          |
| ------------ | ------------------------------------------------------------------------------------ |
| LISTEN       | El host está esperando un request de conexión de cualquier otro host y puerto.       |
| SYN-SENT     | Paso 1 del 3 Way Handshake, el cliente envió el SYN.                                 |
| SYN-RECEIVED | Paso 2 del 3 Way Handshake , el servidor recibió el SYN y envió SYN+ACK.             |
| ESTABLISHED  | Paso 3 del 3 Way Handshake, el cliente envió ACK y la conexión se estableció.        |
| FIN-WAIT-1   | El cliente envió un segmento con flag FIN, indicando que ya terminó de enviar datos. |
| FIN-WAIT-2   | El servidor le envió un ACK y ahora debe enviarle un FIN también.                    |
| CLOSE-WAIT   | El cliente espera que se cierra la conexión.                                         |
| CLOSING      | Ambos lados enviaron el FIN.                                                         |
| LAST-ACK     | El cliente espera un ACK.                                                            |
| TIME-WAIT    | Se espera una determinada cantidad de tiempo a que el servidor reciba el ACK final.  |
| CLOSED       | No hay conexión.                                                                     |

### 13. Dada la siguiente salida del comando ss, responda:

![Salida comando ss](https://i.imgur.com/MzCHrqm.png)

##### a. ¿Cuántas conexiones hay establecidas?

Hay 9 conexiones con estado ESTAB, sin embargo solo hay 8 conexiones establecidas ya que una está repetida a la inversa, es decir, el Local Address y Peer Address aparecen invertidos dos veces.

##### b. ¿Cuántos puertos hay abiertos a la espera de posibles nuevas conexiones?

Hay 5 conexiones en estado LISTEN, sin embargo el puerto se repite en 2 de ellas (puerto 53 de UDP) por ende solo hay 4 puertos esperando conexiones:

1. 22 (TCP)
2. 80 (TCP)
3. 53 (UDP)
4. 25 (TCP)

##### c. El cliente y el servidor de las comunicaciones HTTPS (puerto 443), ¿residen en la misma máquina?

El cliente y el servidor de las comunicaciones en el puerto 443 no residen en la misma máquina, porque si lo hicieran la IP Local y la IP Peer sería la misma lo cual no es el caso.

##### d. El cliente y el servidor de la comunicación SSH (puerto 22), ¿residen en la misma máquina?

Como la comunicación SSH (puerto 22) se está realizando sobre la IP localhost (127.0.0.1), es verdad que tanto el cliente como el servidor residen en la misma máquina.

##### e. Liste los nombres de todos los procesos asociados con cada comunicación. Indique para cada uno si se trata de un proceso cliente o uno servidor.

-   sshd: Cliente y Servidor, ya que está en ESTAB y LISTEN.
-   ssh: Cliente, ya que está en estado ESTAB.
-   apache2: Servidor, ya que está en estado LISTEN.
-   named: Servidor, ya que está en estado LISTEN.
-   x-www-browser: Cliente, ya que está en estado ESTAB.
-   postfix: Servidor, ya que está en estado LISTEN.

##### f. ¿Cuáles conexiones tuvieron el cierre iniciado por el host local y cuáles por el remoto?

La conexión con fd=50 fue cerrada por el host remoto ya que tiene estado CLOSE-WAIT.

La conexión con fd=3 fue cerrada por el host local ya que tiene estado TIME-WAIT.

##### g. ¿Cuántas conexiones están aún pendientes por establecerse?

Solo una, la fd=70. El cliente envió el SYN correspondiente al primer paso del 3 Way Handshake y está esperando recibir una respuesta.

### 14. Dadas las salidas de los siguientes comandos ejecutados en el cliente y el servidor, responder:

![Salida comando](https://i.imgur.com/YpUjvnZ.png)

##### a. ¿Qué segmentos llegaron y cuáles se están perdiendo en la red?

El segmento correspondiente al primer paso de establecimiento de conexión, es decir el SYN del cliente, le llegó efectivamente al servidor ya que el servidor tiene estado SYN-RECV (recibió el SYN).

Sin embargo, no podemos saber con exactitud si el servidor ya envió el SYN+ACK (segundo paso del establecimiento de conexión) y todavía no llego, o si se perdió en la red, o si todavía no lo envió.

##### b. ¿A qué protocolo de capa de aplicación y de transporte se está intentando conectar el cliente?

El cliente se está intentando conectar al puerto 110 del servidor, el cual es el puerto default para POP3 en la capa de aplicación.

En cuanto a capa de transporte, el cliente se está queriendo conectar vía TCP.

##### c. ¿Qué flags tendría seteado el segmento perdido?

El segmento perdido tendría flags SYN = 1 y ACK = 1.

### 15. Use CORE para armar una topología como la siguiente, sobre la cual deberá realizar:

##### a. En ambos equipos inspeccionar el estado de las conexiones y mantener abiertas ambas ventanas con el comando corriendo para poder visualizar los cambios a medida que se realiza el ejercicio. Ayuda: watch -n1 ’ss -nat’.

##### b. En Servidor, utilice la herramienta ncat para levantar un servicio que escuche en el puerto 8001/TCP. Utilice la opción -k para que el servicio sea persistente. Verifique el estado de las conexiones.

![Diagrama CORE](https://i.imgur.com/Q6q7zb6.png)

##### c. Desde CLIENTE1 conectarse a dicho servicio utilizando también la herramienta ncat. Inspeccione el estado de las conexiones.

##### d. Iniciar otra conexión desde CLIENTE1 de la misma manera que la anterior y verificar el estado de las conexiones. ¿De qué manera puede identificar cada conexión?

##### e. En base a lo observado en el item anterior, ¿es posible iniciar más de una conexión desde el cliente al servidor en el mismo puerto destino? ¿Por qué? ¿Cómo se garantiza que los datos de una conexión no se mezclarán con los de la otra?

##### f. Analice en el tráfico de red, los flags de los segmentos TCP que ocurren cuando:

###### i. Cierra la última conexión establecida desde CLIENTE1. Evalúe los estados de las conexiones en ambos equipos.

###### ii. Corta el servicio de ncat en el servidor (Ctrl+C). Evalúe los estados de las conexiones en ambos equipos.

###### iii. Cierra la conexión en el cliente. Evalúe nuevamente los estados de las conexiones.

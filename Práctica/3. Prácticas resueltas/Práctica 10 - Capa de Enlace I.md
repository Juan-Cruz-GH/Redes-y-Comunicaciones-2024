<h1 align="center">Práctica 10 - Capa de Enlace I</h1>

### 1. ¿Qué función cumple la capa de enlace? Indique qué servicios presta esta capa.

### 2. Compare los servicios de la capa de enlace con los de la capa de transporte.

### 3. Direccionamiento Ethernet:

##### ¿Cómo se identifican dos máquinas en una red Ethernet?

##### ¿Cómo se llaman y qué características poseen estas direcciones?

##### ¿Cuál es la dirección de broadcast en la capa de enlace? ¿Qué función cumple?

### 4. Sobre los dispositivos de capa de enlace:

##### Enumere dispositivos de capa de enlace y explique sus diferencias.

##### ¿Qué es una colisión?

##### ¿Qué dispositivos dividen dominios de broadcast?

##### ¿Qué dispositivos dividen dominios de colisión?

### 5. Describa el algoritmo de acceso al medio en Ethernet. ¿Es orientado a la conexión?

### 6. ¿Cuál es la finalidad del protocolo ARP?

### 7. Investigue los comandos arp e ip neigh. Inicie una topología con CORE, cree una máquina y utilice en ella los comandos anteriores para:

##### Listar las entradas en la tabla ARP.

##### Borrar una entrada en la tabla de ARP.

##### Agregar una entrada estática en la tabla de ARP.

### 8. Dado el siguiente esquema de red, responda:

![Esquema de red con Switches](https://i.imgur.com/sjiTQtM.png)

##### a. Suponiendo que las tablas de los switches están llenas con la información correcta, responda quién escucha el mensaje si:

###### i. La estación 1 envía una trama al servidor 1.

###### ii. La estación 1 envía una trama a la estación 11.

###### iii. La estación 1 envía una trama a la estación 9.

###### iv. La estación 4 envía una trama a la MAC de broadcast.

###### v. La estación 6 envía una trama a la estación 7.

###### vi. La estación 6 envía una trama a la estación 10.

##### b. ¿En qué situaciones se pueden producir colisiones?

### 9. En la siguiente topología de red indique:

![Topología con 2 switches y 1 HUB](https://i.imgur.com/NO5aaNF.png)

##### a. ¿Cuántos dominios de colisión hay?

##### b. ¿Cuántos dominios de broadcast hay?

##### c. Indique cómo se va llenando la tabla de asociaciones MAC → PORT de los switches SW1 y SW2 durante el siguiente caso:

###### i. A envía una solicitud ARP consultando la MAC de C.

###### ii. C responde esta solicitud ARP.

###### iii. A envía una solicitud ARP consultando la MAC de B.

###### iv. B responde esta solicitud ARP.

##### d. Si la PC E y la PC D hubiesen estado realizando un tcpdump para escuchar todo lo que pasa por su interfaz de red, ¿cuáles de los requerimientos/respuestas anteriores hubiesen escuchado cada una?

### 10. En la siguiente topología:

![Topología sobre ARP](https://i.imgur.com/XlJa7lt.png)

Suponiendo que todas las tablas ARP están vacías, tanto de PCs como de routers. Si la
PC_A le hace un ping a la PC_C, indique:

##### ¿En qué dominios de broadcast hay tráfico ARP? ¿Con qué direcciones de origen y destino?

##### ¿En qué dominios de broadcast hay tráfico ICMP?¿Con qué direcciones de origen y destino de capa 2?¿Con qué direcciones de origen y destino de capa 3?

##### ¿Cuál es la secuencia correcta en la que se suceden los anteriores?

### 11. ¿Existe ARP en IPv6? ¿Por qué? ¿Quién cumple esa función?

### 12. ¿Qué es la IEEE 802.3? ¿Existen diferencias con Ethernet?

### 13. Si la PC A está en una red y se quiere comunicar con la PC B que está en otra red:

##### ¿Cómo se da cuenta la PC A de esto?

##### Si la tabla ARP de la PC A está vacía, ¿qué dirección MAC necesita la PC A para poder comunicarse con la PC B?

##### En base a lo anterior, ¿qué dirección IP destino tiene el requerimiento ARP? ¿Es la dirección IP del default gateway o es la dirección IP de la PC B? Complete los campos:

-   Trama Ethernet:
    -   MAC origen: \_\_\_
    -   MAC destino: \_\_\_
-   Solicitud ARP:
    -   MAC origen: \_\_\_
    -   IP origen: \_\_\_
    -   MAC destino: \_\_\_
    -   IP destino: \_\_\_

##### En base a lo anterior, indique la información de capa 2 y 3 del ICMP ECHO REQUEST que la PC A le envía a la PC B cuando ejecuta un ping, en el segmento de LAN de la PC B.

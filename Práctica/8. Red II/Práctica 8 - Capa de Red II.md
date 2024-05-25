<center>

# Práctica 8 - Capa de Red II

</center>

## Recomendación

Al final de la práctica se encuentra un ejercicio para ser realizado en la herramienta CORE.
Si bien el ejercicio no agrega conceptos nuevos a los vistos previamente, recomendamos su resolución para que puedan configurar, probar y analizar todo lo aprendido en una simulación de una red.

## Fragmentación

### 1. Se tiene la siguiente red con los MTUs indicados en la misma. Si desde pc1 se envía un paquete IP a pc2 con un tamaño total de 1500 bytes (cabecera IP más payload) con el campo Identification = 20543, responder:

![Topología fragmentación](https://i.imgur.com/pghvrGV.png)

##### a. Indicar IPs origen y destino y campos correspondientes a la fragmentación cuando el paquete sale de pc1

-   IP origen: 10.0.0.20/24
-   IP destino: 10.0.2.20/24
-   Tamaño del header: Siempre 20 bytes
-   Total Length: 1500 bytes (significa que el payload es de 1480 bytes)
-   Identificación: 20543
-   Don't Fragment: 0
-   More Fragments: 0
-   Fragment Offset: 0

##### b. ¿Qué sucede cuando el paquete debe ser reenviado por el router R1?

Cuando el paquete debe ser reenviado por el router R1 lo que sucede es que el paquete no cabe, ya que el MTU entre R1 y R2 es de 600 bytes y el paquete es de 1500 bytes.

##### c. Indicar cómo quedarían los paquetes fragmentados para ser enviados por el enlace entre R1 y R2.

Para fragmentar el paquete tomamos el valor del MTU (600 en este caso), le restamos el valor del header (600 - 20 = 580) y luego encontramos el múltiplo de 8 más cercano a ese numero. Pasos:

1. Cantidad de fragmentos que tendremos:
    - 1500 / 600 = 3 (redondeando)
2. Offsets:
    - 600 - 20 / 8 = 72. (0 - 72 - 144 - 216 - etc)
3. Tamaño máximo de un fragmento:
    - Múltiplo de 8 más cercano a 580 -> 576
    - Le sumamos el header (no se suma en el último fragmento) -> 576 + 20 = **596**
4. Los fragmentos serán:
    - Tamaño paquete + (Tamaño header \* Cantidad de fragmentos - 1)
    - 1500 + (20 \* 3 - 1) = 1500 + 40 = **1540**
    - 596 + 596 + x = 1540 -> x = **348**
    - Fragmento 1: 596 bytes; Fragmento 2: 596 bytes; Fragmento 3: 348 bytes.

##### d. ¿Dónde se unen nuevamente los fragmentos? ¿Qué sucede si un fragmento no llega?

Los fragmentos se unen nuevamente en el destino, en este ejemplo, pc2.

Si algun fragmento se pierde y no llega (se terminó su TTL) se retransmiten todos los fragmentos.

##### e. Si un fragmento tiene que ser reenviado por un enlace con un MTU menor al tamaño del fragmento, ¿qué hará el router con ese fragmento?

Si un fragmento no cabe en el MTU, se lo volverá a fragmentar.

## Ruteo

### 2. ¿Qué es el ruteo? ¿Por qué es necesario?

Rutear consiste en hallar el mejor camino, según distintas métricas, entre A y B donde A y B pueden ser hosts, routers, etc.

Es necesario para que los paquetes puedan ser transmitidos efectivamente.

### 3. En las redes IP el ruteo puede configurarse en forma estática o en forma dinámica. Indique ventajas y desventajas de cada método.

-   Ruteo estático:

| Ventajas                                                 | Desventajas                                                                |
| -------------------------------------------------------- | -------------------------------------------------------------------------- |
| Como las rutas las establece el admin, hay mayor control | No escalable                                                               |
| No tiene problemas de seguridad ni de compatibilidad     | No tolerante a fallos                                                      |
| Útil cuando se tiene una red sencilla                    | Propenso a errores                                                         |
| Eficiente en términos de procesamiento                   | Si se cambia la topología requiere cambiar todo manualmente en los routers |

-   Ruteo dinámico:

| Ventajas                                                   | Desventajas                                            |
| ---------------------------------------------------------- | ------------------------------------------------------ |
| Si la topología cambia, el ruteo se adapta automáticamente | Costoso en procesamiento                               |
| Fácil mantenimiento incluso en redes complejas             | Requiere una configuración inicial por parte del admin |
| Escalable                                                  | El debugging es más complejo                           |
| Tolerante a fallos                                         |                                                        |

### 4. Una máquina conectada a una red pero no a Internet, ¿tiene tabla de ruteo?

Una máquina conectada a una red pero no a Internet sí tiene tabla de ruteo, ya que podría necesitarla para rutear en su red local.

### 5. Observando el siguiente gráfico y la tabla de ruteo del router D, responder:

![Topología con tabla de ruteo](https://i.imgur.com/K59zkvW.png)

##### a. ¿Está correcta esa tabla de ruteo? En caso de no estarlo, indicar el o los errores encontrados. Escribir la tabla correctamente (no es necesario agregar las redes que conectan contra los ISPs)

1.  Lo primero que voy a chequear es que no falte ninguna red destino en la tabla del router D:

    -   La tabla tiene 9 entradas y hay 10 redes (5 comunes + 5 punto a punto, no contamos las que conectan contra los ISPs). Por ende hay una red que falta. Puedo ver claramente que la que falta es la red punto a punto 10.0.0.8/30, por ende la agrego con Next-Hop = "-" e Iface = eth3.

2.  Luego voy a cambiar eth5 por eth2 en el router D, ya que los nombres de las interfaces suelen ser incrementales, no tiene sentido pasar de eth1 a eth3 y luego eth5.
3.  La cuarta entrada de la tabla está mal porque no se puede incluir una máscara en el Next-Hop, así que la borramos.
4.  En la sexta entrada de la tabla la Red Destino está mal escrita y el Next-Hop es incorrecto.
    -   205.10.128.0 -> 205.10.0.128.
    -   10.0.0.2 -> 10.0.0.1
5.  En la octava entrada de la tabla la Red Destino está mal escrita y el Next-Hop e Iface no son una ruta óptima.
    -   205.20.0.193 -> 205.20.0.192
    -   10.0.0.1 -> 10.0.0.5
    -   eth5 -> eth0

La tabla final corregida quedaría:

| Red Destino     | Red Destino (IP) | Mask | Next-Hop  | Iface |
| --------------- | ---------------- | ---- | --------- | ----- |
| D               | 153.10.20.128    | /27  | -         | eth1  |
| Rtr-D <-> Rtr-B | 10.0.0.4         | /30  | -         | eth0  |
| Rtr-D <-> Rtr-A | 10.0.0.0         | /30  | -         | eth2  |
| Rtr-D <-> Rtr-C | 10.0.0.8         | /30  | -         | eth3  |
| Rtr-B <-> Rtr-A | 10.0.0.12        | /30  | 10.0.0.5  | eth0  |
| Rtr-C <-> Rtr-A | 10.0.0.16        | /30  | 10.0.0.10 | eth3  |
| C               | 163.10.5.64      | /27  | 10.0.0.10 | eth3  |
| E               | 205.20.0.128     | /26  | 10.0.0.5  | eth0  |
| B               | 205.20.0.192     | /26  | 10.0.0.5  | eth0  |
| A               | 205.10.0.128     | /25  | 10.0.0.1  | eth2  |

##### b. Con la tabla de ruteo del punto anterior, Red D, ¿tiene salida a Internet? ¿Por qué? ¿Cómo lo solucionaría? Suponga que los demás routers están correctamente configurados, con salida a Internet y que Rtr-D debe salir a Internet por Rtr-C.

Con la tabla anterior la Red D no tiene salida a Internet porque no hay ninguna entrada que tenga como Red Destino ninguna de las dos redes que conectan con los 2 ISPs.

Lo solucionaría añadiendo una entrada con la ruta default o "default gateway".

| Red Destino | Mask | Next-Hop  | Iface |
| ----------- | ---- | --------- | ----- |
| 0.0.0.0     | /0   | 10.0.0.10 | eth3  |

##### c. Teniendo en cuenta lo aplicado en el punto anterior, si en Rtr-C estuviese la siguiente entrada en su tabla de ruteo qué sucedería si desde una PC en Red D se quiere acceder un servidor con IP 163.10.5.15.

| Red Destino | Mask | Next-Hop | Iface |
| ----------- | ---- | -------- | ----- |
| 163.10.5.0  | /24  | 10.0.0.9 | eth1  |

Si Rtr-C tuviese esa entrada en su tabla de ruteo, cuando una PC en Red D quiere acceder a un servidor con IP 163.10.5.15, ocurriría lo siguiente:

1. La PC en Red D sale por su router default, Rtr-D.
2. Rtr-D chequea la IP destino que le pidió la PC: 163.10.5.15 y no la encuentra en su tabla, por ende sale por la default gateway.
3. El paquete pasa a Rtr-C.
4. Rtr-C chequea la IP destino que le pasó Rtr-D: 163.10.5.15 y la encuentra en su tabla, por ende se la manda al Rtr-D.
5. El paquete queda en loop hasta que se le termine el TTL.

##### d. ¿Es posible aplicar sumarización en la tabla del router Rtr-D? ¿Por qué? ¿Qué debería suceder para poder aplicarla?

Para poder aplicar sumarización entre 2 o más entradas de la tabla, se deben repetir Mask, Next-Hop e Iface entre ellas.

Por ende sí, es posible aplicar sumarización: entre la entrada de la Red Destino E y la entrada de la Red Destino B.

205.20.0. 10000000
205.20.0. 11000000

Son iguales hasta el bit 25, por ende la máscara es /25, y la fila quedaría:

205.20.0.128 /25 10.0.0.5 eth0

##### e. La sumarización aplicada en el punto anterior, ¿se podría aplicar en Rtr-B? ¿Por qué?

No, no se podría, ya que en el caso de Rtr-B, si bien la máscara y Next-Hop serían iguales, las interfaces no lo son: hacia Red B es eth0 y hacia Red E es eth2.

##### f. Escriba la tabla de ruteo de Rtr-B teniendo en cuenta lo siguiente:

● Debe llegarse a todas las redes del gráfico
● Debe salir a Internet por Rtr-A
● Debe pasar por Rtr-D para llegar a Red D
● Sumarizar si es posible

| Red Destino     | Red Destino (IP) | Mask | Next-Hop  | Iface |
| --------------- | ---------------- | ---- | --------- | ----- |
| B               | 205.20.0.192     | /26  | -         | eth0  |
| E               | 205.20.0.128     | /26  | -         | eth2  |
| Rtr-B <-> Rtr-D | 10.0.0.4         | /30  | -         | eth1  |
| Rtr-B <-> Rtr-A | 10.0.0.12        | /30  | -         | eth3  |
| Rtr-A <-> Rtr-C | 10.0.0.16        | /30  | 10.0.0.13 | eth3  |
| Rtr-A <-> Rtr-D | 10.0.0.0         | /30  | 10.0.0.13 | eth3  |
| Rtr-D <-> Rtr-C | 10.0.0.8         | /30  | 10.0.0.6  | eth1  |
| A               | 205.10.0.128     | /25  | 10.0.0.13 | eth3  |
| D               | 153.10.20.128    | /27  | 10.0.0.6  | eth1  |
| C               | 163.10.5.64      | /27  | 10.0.0.13 | eth3  |
| Rtr-A <-> ISP-1 | 120.0.0.0        | /30  | 10.0.0.13 | eth3  |
| Rtr-C <-> ISP-2 | 130.0.10.0       | /30  | 10.0.0.13 | eth3  |
| Default Gateway | 0.0.0.0          | /0   | 10.0.0.13 | eth3  |

-   Sumarizamos 10.0.0.16 con 10.0.0.0:
    10.0.0. 00010000
    10.0.0. 00000000
    -> 10.0.0.0 /27 10.0.0.13 eth3

##### g. Si Rtr-C pierde conectividad contra ISP-2, ¿es posible restablecer el acceso a Internet sin esperar a que vuelva la conectividad entre esos dispositivos?

Si Rtr-C pierde conexión a Internet vía ISP-2, podemos modificar la tabla de ruteo de ese router para que salga a Internet por ISP-1 en lugar de ISP-2.

### 6. Evalúe para cada caso si el mensaje llegará a destino, saltos que tomará y tipo de respuesta recibida el emisor.

![Topología con 4 tablas de ruteo](https://i.imgur.com/bx2oZIO.png)

● Un mensaje ICMP enviado por PC-B a PC-C.

1. PC-C: 10.0.7.20/24
2. PC-B le pasa el pedido a router2
3. La IP 10.0.7.20/24 no matchea en su tabla, por ende sale por su default gateway hacia router1
4. router1 matchea la IP con la cuarta entrada de su tabla, por ende sale por eth1 hacia router3
5. router3 matchea la IP con la tercer entrada de su tabla, por ende sale por eth2 hacia PC-C.

El emisor recibirá ICMP Echo Reply.

Saltos: PC-B -> router2 -> router1 -> router3 -> PC-C

● Un mensaje ICMP enviado por PC-C a PC-B.

● Un mensaje ICMP enviado por PC-C a 8.8.8.8.
● Un mensaje ICMP enviado por PC-B a 8.8.8.8.

## DHCP y NAT

### 7. Con la máquina virtual con acceso a Internet realice las siguientes observaciones respecto de la autoconfiguración IP vía DHCP:

##### a. Inicie una captura de tráfico Wireshark utilizando el filtro bootp para visualizar únicamente tráfico de DHCP.

##### b. En una terminal de root, ejecute el comando $ sudo /sbin/dhclient eth0 y analice el intercambio de paquetes capturado.

##### c. Analice la información registrada en el archivo /var/lib/dhcp/dhclient.leases, ¿cuál parece su función?

##### d. Ejecute el siguiente comando para eliminar información temporal asignada por el servidor DHCP.

`$ rm /var/lib/dhcp/dhclient.leases`

##### e. En una terminal de root, vuelva a ejecutar el comando $ sudo /sbin/dhclient eth0 y analice el intercambio de paquetes capturado nuevamente ¿a que se debió la diferencia con lo observado en el punto “b”?

##### f. Tanto en “b” como en “e”, ¿qué información es brindada al host que realiza la petición DHCP, además de la dirección IP que tiene que utilizar?

### 8. ¿Qué es NAT y para qué sirve? De un ejemplo de su uso y analice cómo funcionaría en ese entorno. Ayuda: analizar el servicio de Internet hogareño en el cual varios dispositivos usan Internet simultáneamente.

### 9. ¿Qué especifica la RFC 1918 y cómo se relaciona con NAT?

### 10. En la red de su casa o trabajo verifique la dirección IP de su computadora y luego acceda a www.cualesmiip.com. ¿Qué observa? ¿Puede explicar qué sucede?

### 11. Resuelva las consignas que se dan a continuación.

##### a. En base a la siguiente topología y a las tablas que se muestran, complete los datos que faltan

![Topología](https://i.imgur.com/sZG21wz.png)

PC-A (ss)
Local Address:Port Peer Address:Port
192.168.1.2:49273 **\*\*\*\***\_**\*\*\*\***
**\*\*\*\***\_**\*\*\*\*** 190.50.10.63:25
192.168.1.2:**\_** 190.50.10.81:8080

PC-B (ss)
Local Address:Port Peer Address:Port
192.168.1.3:52734 **\*\*\*\***\_**\*\*\*\***
192.168.1.3:39275 **\*\*\*\***\_**\*\*\*\***

RTR-1 (Tabla de NAT)
Lado LAN Lado WAN
192.168.1.2:49273 205.20.0.29:25192
192.168.1.2:51238 **\*\*\*\***\_**\*\*\*\***
192.168.1.3:52734 205.20.0.29:51091
192.168.1.2:37484 205.20.0.29:41823
192.168.1.3:39275 205.20.0.29:9123

SRV-A (ss)
Local Address:Port Peer Address:Port
190.50.10.63:80 205.20.0.29:25192
190.50.10.63:25 205.20.0.29:41823

SRV-B (ss)
Local Address:Port Peer Address:Port
190.50.10.81:8080 205.20.0.29:16345
190.50.10.81:8081 205.20.0.29:51091
190.50.10.81:8080 205.20.0.29:9123

##### b. En base a lo anterior, responda:

###### i. ¿Cuántas conexiones establecidas hay y entre qué dispositivos?

###### ii. ¿Quién inició cada una de las conexiones? ¿Podrían haberse iniciado en sentido inverso? ¿Por qué? Investigue qué es port forwarding y si serviría como solución en este caso.

## Ejercicio de repaso

![Topología con asignaciones](https://i.imgur.com/BFB18al.png)

### 12. Asigne las redes que faltan utilizando los siguientes bloques y las consideraciones debajo:

|                  |                 |                 |                 |
| ---------------- | --------------- | --------------- | --------------- |
| 224.10.0.128/27  | 224.10.0.64/26  | 192.168.10.0/24 | 10.10.10.0/27   |
| 226.10.20.128/27 | 200.30.55.64/26 | 127.0.0.0/24    | 192.168.10.0/29 |

● Red C y la Red D deben ser públicas.
● Los enlaces entre routers deben utilizar redes privadas.
● Se debe desperdiciar la menor cantidad de IP posibles.
● Si va a utilizar un bloque para dividir en subredes, asignar primero la red con más
cantidad de hosts y luego las que tienen menos.
● Las redes elegidas deben ser válidas.

### 13. Asigne IP a todas las interfaces de las redes listadas a continuación. Nota: Los routers deben tener asignadas las primeras IP de la red. Para enlaces entre routers, asignar en el siguiente orden: RouterA, RouterB, RouterC, RouterD y RouterE.

● Red A, Red B, Red C y Red D.
● Red entre RouterA-RouterB-RouterE.
● Red entre RouterC-RouterD.

### 14. Realice las tablas de rutas de RouterE y BORDER considerando:

● Siempre se deberá tomar la ruta más corta.
● Sumarizar siempre que sea posible.
● El tráfico de Internet a la Red D y viceversa debe atravesar el RouterC.
● Todos los hosts deben poder conectarse entre sí y a Internet.

## Aclaración importante

En CORE no se guardan los cambios realizados en una topología al detenerla. Por ello, es deseable completar todo el ejercicio una vez empezado, para no tener que volver a configurar todo. Alternativamente se puede utilizar el script que se encuentra en este repositorio https://github.com/RYSAEI/SaveRestoreScripts para forzar que se guarden los cambios.

### 15. Utilizando la máquina virtual, se configurará ruteo estático en la red que se muestra en el siguiente gráfico:

![Topología](https://i.imgur.com/Q3283DS.png)

##### a. Antes de empezar el ejercicio ejecute en una terminal el siguiente comando: $ sudo iptables -P FORWARD ACCEPT

##### b. Inicie la herramienta CORE y abra el archivo 1-ruteo-estatico.imn.

##### c. Inicie la virtualización de la topología.

##### d. Analice las tablas de ruteo de las diferentes PCs y de los routers. ¿Qué observa? ¿Puede explicar por qué?

##### e. Configure las direcciones IP de las interfaces según lo que muestra el gráfico (para entrar a configurar cada equipo ya sea una PC o un router debe hacer doble click sobre el mismo, lo cual abre una terminal de comandos). Por ejemplo:

■ En la PC n6 debe configurar la interfaz eth0 con la IP 10.0.0.10.
■ En el Router n1 debe configurar la eth0 con la IP 10.0.0.1, la eth1 con la IP 10.0.1.2 y la eth2 con la 10.0.2.1.

##### f. Analice las tablas de ruteo de las diferentes PCs y de los routers. ¿Qué observa? ¿Puede explicar por qué?

##### g. Compruebe conectividad. Para ello, tome por ejemplo la PC n7 y haga un ping a cada una de las diferentes IPs que configuró. ¿Qué ocurre y por qué?

##### h. Configure una ruta por defecto en todas las computadoras y analice los cambios en las tablas de ruteo.

##### i. Compruebe conectividad repitiendo el mismo procedimiento que realizó anteriormente. ¿Qué ocurre y por qué?

##### j. Función de ruteo: un dispositivo que actúe como router requiere tener habilitado el encaminamiento de paquetes entre sus interfaces.

■ Verificar IP_FORWARD, en los routers y las PCs, obteniendo la configuración
con: $ cat /proc/sys/net/ipv4/ip_forward
El valor 0 indica funcionalidad desactivada (esto es correcto para las PCs). 1 indica que está habilitado (esto es requerido para los routers).

##### k. Configure en los routers rutas estáticas a cada una de las redes de la topología (no utilice rutas por defecto).

##### l. Compruebe conectividad entre todos los dispositivos de la red. Si algún dispositivo no puede comunicarse con otro revise las tablas de ruteo y solucione los inconvenientes hasta que la conectividad sea completa.

##### m. Modifique ahora las tablas de ruteo de los routers, eliminando todas las rutas configuradas hasta el momento y vuelva a configurarlas en base al siguiente criterio.

■ Router n1 envía todo el tráfico desconocido a Router n2.
■ Router n2 envía todo el tráfico desconocido a Router n3.
■ Router n3 envía todo el tráfico desconocido a Router n1.

##### n. Compruebe conectividad entre todos los dispositivos de la red. Si algún dispositivo no puede comunicarse con otro revise las tablas de ruteo y solucione los inconvenientes hasta que la conectividad sea completa.

##### o. En base a las dos configuraciones de las tablas de ruteo anteriores, responda:

■ ¿Cuál opción le resultó más sencilla y por qué?
■ Considerando el tamaño de las tablas de ruteo en cada situación, ¿cuál de las dos opciones le parece más conveniente y por qué?
■ ¿Puede pensar en algún caso donde la segunda opción sea la única posible?
■ Suponga que realiza un ping a un host que tiene la IP 190.50.12.34. ¿Qué ocurrirá en cada caso? ¿Cuál le parece mejor?

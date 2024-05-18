<center>

# Práctica 7 - Capa de Red I

</center>

## Introducción

### 1. ¿Qué servicios presta la capa de red? ¿Cuál es la PDU en esta capa? ¿Qué dispositivo es considerado sólo de la capa de red?

La Capa de Red presta los siguientes servicios:

1. Direccionamiento: asignar direcciones IP a las interfaces de los dispositivos.
2. Ruteo: determinar el mejor camino entre redes para poder enviar paquetes.
3. Forwarding: enviar paquetes entre redes.
4. Manejo de errores básico: detectar y manejar errores durante la transmisión de paquetes.
5. Fragmentación y rensamblado: deshacer paquetes en fragmentos chicos y luego re-armar el paquete cuando llega.

El PDU de la capa de red es el paquete o datagrama IP.

Los routers son dispositivos considerados solo de capa de red.

### 2. ¿Por qué se lo considera un protocolo de mejor esfuerzo?

IP es un protocolo de mejor esfuerzo (Best Effort) porque:

1. No garantiza que los paquetes que se envían lleguen a destino.
2. Si llegan a destino no se garantiza que lleguen correctos, es decir sin corromperse.
3. Si llegan a destino no se garantiza que lleguen en orden.

### 3. ¿Cuántas redes clase A, B y C hay? ¿Cuántos hosts como máximo pueden tener cada una?

| Clase | Cantidad de redes          | Cantidad de hosts               |
| ----- | -------------------------- | ------------------------------- |
| A     | 2<sup>7</sup> = 128        | 2<sup>24</sup> - 2 = 16.777.214 |
| B     | 2<sup>14</sup> = 16.384    | 2<sup>16</sup> - 2 = 65.534     |
| C     | 2<sup>21</sup> = 2.097.152 | 2<sup>8</sup> - 2 = 254         |

### 4. ¿Qué son las subredes? ¿Por qué es importante siempre especificar la máscara de subred asociada?

Las subredes son divisiones de redes más grandes con el objetivo de hacer un mejor uso del número de redes y hosts de un bloque.

Es importante siempre especificar la máscara de subred asociada a cada red para así poder conocer cuál parte de la dirección IP es la subred y cuál los hosts.

### 5. ¿Cuál es la finalidad del campo Protocol en la cabecera IP? ¿A qué campos de la capa de transporte se asemeja en su funcionalidad?

El campo Protocol en la cabecera IP especifica qué protocolo deberá usar el **receptor** en su capa de transporte (UDP, TCP, etc).

Se asemeja a los puertos de los segmentos TCP o datagramas UDP.

## División en subredes

### 6. Para cada una de las siguientes direcciones IP (172.16.58.223/26, 163.10.5.49/27, 128.10.1.0/23, 10.1.0.0/24, 8.40.11.179/12) determine:

| Clase | Primer octeto | Máscara default |
| ----- | ------------- | --------------- |
| A     | 0             | /8              |
| B     | 10            | /16             |
| C     | 11            | /24             |
| D     | 111           | -               |
| E     | 1111          | -               |

##### a. ¿De qué clase de red es la dirección dada (Clase A, B o C)?

1. 172.16.58.223/26 -> 172 = **10**10 1100 -> Es clase B.
2. 163.10.5.49/27 -> 163 = **10**10 0011 -> Es clase B.
3. 128.10.1.0/23 -> 128 = **10**00 0000 -> Es clase B.
4. 10.1.0.0/24 -> 10 = **00**00 1010 -> Es clase A.
5. 8.40.11.179/12 -> 8 = **00**00 0100 -> Es clase A.

##### b. ¿Cuál es la dirección de subred?

La dirección de subred se halla haciendo una operación AND entre la dirección IP y la máscara.

Un detalle importante es que los primeros N bits, con N siendo el valor de la máscara, quedan exactamente igual, por ende los omitimos en el cálculo.

1. 172.16.58.223/26

```
172.16.58.11011111
AND
...       11000000
======================
172.16.58.192
```

2. 163.10.5.49/27

```
163.10.5.00110001
AND
...      11100000
======================
163.10.5.32
```

3. 128.10.1.0/23

```
128.10.00000001.00000000
AND
...    11111110.00000000
======================
128.10.0.0
```

4. 10.1.0.0/24

```
10.1.00000000.00000000
AND
...    11111111.00000000
======================
10.1.0.0
```

5. 8.40.11.179/12

```
8.00101000.00001011.10110011
AND
 .11110000.00000000.00000000
======================
8.32.0.0
```

##### c. ¿Cuál es la cantidad máxima de hosts que pueden estar en esa subred?

La cantidad máxima de hosts de una subred se halla primero hallando la cantidad de bits asignados a host (32 - máscara) y luego restándole 2, ya que la dirección de subred y la dirección de broadcast no se pueden usar para direccionar hosts.

1. 172.16.58.223/26 -> 2<sup>32-26</sup> - 2 = 64 - 2 = **62**
2. 163.10.5.49/27 -> 2<sup>32-27</sup> - 2 = 32 - 2 = **30**
3. 128.10.1.0/23 -> 2<sup>32-23</sup> - 2 = 512 - 2 = **510**
4. 10.1.0.0/24 -> 2<sup>32-24</sup> - 2 = 256 - 2 = **254**
5. 8.40.11.179/12 -> 2<sup>32-12</sup> - 2 = 1.048.576 - 2 = **1.048.574**

##### d. ¿Cuál es la dirección de broadcast de esa subred?

La dirección broadcast de una subred se halla poniendo todos los bits de host en 1.

1. 172.16.58.223/26

```
172.16.58.11011111
172.16.58.11111111
======================
172.16.58.255
```

2. 163.10.5.49/27

```
163.10.5.00110001
163.10.5.00111111
======================
163.10.5.63
```

3. 128.10.1.0/23

```
128.10.00000001.00000000
128.10.00000001.11111111
======================
128.10.1.255
```

4. 10.1.0.0/24

```
10.1.00000000.00000000
10.1.00000000.11111111
======================
10.1.0.255
```

5. 8.40.11.179/12

```
8.00101000.00001011.10110011
8.00101111.11111111.11111111
======================
8.47.255.255
```

##### e. ¿Cuál es el rango de direcciones IP válidas dentro de la subred?

El rango de direcciones IP válidas dentro de una subred va desde la dirección de subred + 1 hasta la dirección de broadcast - 1.

1. 172.16.58.223/26 -> Desde 172.16.58.193 hasta 172.16.58.254
2. 163.10.5.49/27 -> Desde 163.10.5.33 hasta 163.10.5.62
3. 128.10.1.0/23 -> Desde 128.10.0.1 hasta 128.10.1.254
4. 10.1.0.0/24 -> Desde 10.1.0.1 hasta 10.1.0.254
5. 8.40.11.179/12 -> Desde 8.32.0.1 hasta 8.47.255.254

### 7. Su organización cuenta con la dirección de red 128.50.10.0. Indique:

##### a. ¿Es una dirección de red o de host?

La dirección 128.50.10.0 es de clase B. Como no se especifica la máscara, asumimos la default que para clase B es la /16. Como la dirección de subred de 128.50.10.0/16 es 128.50.0.0, la dirección dada no es de red, es de **host**.

##### b. Clase a la que pertenece y máscara de clase.

Clase B y máscara /16.

##### c. Cantidad de hosts posibles.

2<sup>32-16</sup> - 2 = 65.534

##### d. Se necesitan crear, al menos, 513 subredes. Indique:

###### i. Máscara necesaria.

Para crear 513 subredes necesitamos que la máscara permita esto. Necesitamos 10 bits para la parte de red ya que 9 no alcanza: 2<sup>9</sup> = 512. Por ende la máscara será /10.

###### ii. Cantidad de redes asignables.

Se pueden asignar 1024 redes.

2<sup>10</sup> = 1024 - 513 = 511 desperdiciadas.

###### iii. Cantidad de hosts por subred.

2<sup>22</sup> - 2 = 4.194.302 hosts por subred.

###### iv. Dirección de la subred 710.

???

###### v. Dirección de broadcast de la subred 710.

???

### 8. Si usted estuviese a cargo de la administración del bloque IP 195.200.45.0/24

##### a. ¿Qué máscara utilizaría si necesita definir al menos 9 subredes?

##### b. Indique la dirección de subred de las primeras 9 subredes.

##### c. Seleccione una e indique dirección de broadcast y rango de direcciones asignables en esa subred.

### 9. Dado el siguiente gráfico:

![Gráfico topología](https://i.imgur.com/egUCnIh.png)

##### a. Verifique si es correcta la asignación de direcciones IP y, en caso de no serlo, modifique la misma para que lo sea.

##### b. ¿Cuántos bits se tomaron para hacer subredes en la red 10.0.10.0/24? ¿Cuántas subredes se podrían generar?

##### c. Para cada una de las redes utilizadas indique si son públicas o privadas.

## CIDR

### 10. ¿Qué es CIDR (Class Interdomain routing)? ¿Por qué resulta útil?

### 11. ¿Cómo publicaría un router las siguientes redes si se aplica CIDR?

##### a. 198.10.1.0/24

##### b. 198.10.0.0/24

##### c. 198.10.3.0/24

##### d. 198.10.2.0/24

### 12. Listar las redes involucradas en los siguientes bloques CIDR:

● 200.56.168.0/21
● 195.24.0.0/13
● 195.24/13

### 13. El bloque CIDR 128.0.0.0/2 o 128/2, ¿Equivale a listar todas las direcciones de red de clase B? ¿Cuál sería el bloque CIDR que agrupa todas las redes de clase A?

## VLSM

### 14. ¿Qué es y para qué se usa VLSM?

### 15. Describa, con sus palabras, el mecanismo para dividir subredes utilizando VLSM.

### 16. Suponga que trabaja en una organización que tiene la red que se ve en el gráfico y debe armar el direccionamiento para la misma, minimizando el desperdicio de direcciones IP. Dicha organización posee la red 205.10.192.0/19, que es la que usted deberá utilizar.

![Gráfico topología](https://i.imgur.com/J2KAf4l.png)

##### a. ¿Es posible asignar las subredes correspondientes a la topología utilizando subnetting sin VLSM? Indique la cantidad de hosts que se desperdicia en cada subred.

##### b. Asigne direcciones a todas las redes de la topología. Tome siempre en cada paso la primera dirección de red posible.

##### c. Para mantener el orden y el inventario de direcciones disponibles, haga un listado de todas las direcciones libres que le quedaron, agrupándolas utilizando CIDR.

##### d. Asigne direcciones IP a todas las interfaces de la topología que sea posible.

### 17. Utilizando la siguiente topología y el bloque asignado, arme el plan de direccionamiento IPv4 teniendo en cuenta las siguientes restricciones:

![Gráfico topología](https://i.imgur.com/DUd3eb5.png)

##### a. Utilizar el bloque IPv4 200.100.8.0/22.

##### b. La red A tiene 125 hosts y se espera un crecimiento máximo de 20 hosts.

##### c. La red X tiene 63 hosts.

##### d. La red B cuenta con 60 hosts

##### e. La red Y tiene 46 hosts y se espera un crecimiento máximo de 18 hosts.

##### f. En cada red, se debe desperdiciar la menor cantidad de direcciones IP posibles. En este sentido, las redes utilizadas para conectar los routers deberán utilizar segmentos de red /30 de modo de desperdiciar la menor cantidad posible de direcciones IP.

### 18. Asigne direcciones IP en los equipos de la topología según el plan anterior.

## ICMP y Configuraciones IP

### 19. Describa qué es y para qué sirve el protocolo ICMP.

##### a. Analice cómo funciona el comando ping.

###### i. Indique el tipo y código ICMP que usa el ping.

###### ii. Indique el tipo y código ICMP que usa la respuesta de un ping.

##### b. Analice cómo funcionan comandos como traceroute/tracert de Linux/Windows y cómo manipulan el campo TTL de los paquetes IP.

##### c. Indique la cantidad de saltos realizados desde su computadora hasta el sitio www.nasa.gov. Analice:

###### i. Cómo hacer para que no muestre el nombre del dominio asociado a la IP de cada salto.

###### ii. La razón de la aparición de \* en parte o toda la respuesta de un salto.

##### d. Verifique el recorrido hacia los servidores de nombre del dominio unlp.edu.ar. En base al recorrido realizado, ¿podría confirmar cuál de ellos toma un camino distinto?

### 20. ¿Para que se usa el bloque 127.0.0.0/8? ¿Qué PC responde a los siguientes comandos?

##### a. ping 127.0.0.1

##### b. ping 127.0.54.43

### 21. Investigue para qué sirven los comandos ifconfig y route. ¿Qué comandos podría utilizar en su reemplazo? Inicie una topología con CORE, cree una máquina y utilice en ella los comandos anteriores para practicar sus diferentes opciones, mínimamente:

● Configurar y quitar una dirección IP en una interfaz.
● Ver la tabla de ruteo de la máquina.

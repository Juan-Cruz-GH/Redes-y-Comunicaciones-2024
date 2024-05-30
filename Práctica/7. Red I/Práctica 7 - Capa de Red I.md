<h1 align="center">Práctica 7 - Capa de Red I</h1>

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

Para crear 513 subredes necesitamos que la máscara permita esto. Necesitamos 10 bits para la parte de red ya que 9 no alcanza: 2<sup>9</sup> = 512. Por ende la máscara será /26, porque teníamos que la default es /16 y ahora le agregamos 10 bits más.

###### ii. Cantidad de redes asignables.

Se pueden asignar 1024 redes.

2<sup>10</sup> = 1024 - 513 = 511 desperdiciadas.

###### iii. Cantidad de hosts por subred.

2<sup>6</sup> - 2 = 62 hosts por subred.

###### iv. Dirección de la subred 710.

Como las redes siempre empiezan desde 0, la subred 710 es en realidad 709.

1. Pasamos 709 a binario: 10110001 01
2. Pasamos la red original a binario: 10000000.00110010.00000000.00000000
3. Ubicamos el 709: 10000000.00110010.10110001.01000000
4. Pasamos a decimal: **128.50.177.64/26**

###### v. Dirección de broadcast de la subred 710.

La dirección de broadcast de 128.50.177.64/26 es su última dirección, o lo que es igual, la dirección anterior a la subred 711 -> **128.50.177.127/26**

### 8. Si usted estuviese a cargo de la administración del bloque IP 195.200.45.0/24

##### a. ¿Qué máscara utilizaría si necesita definir al menos 9 subredes?

Para definir al menos 9 subredes, necesitaría una máscara con 4 bits ya que 3 bits no alcanza: 2<sup>4</sup> = 16. Por ende /4.

Como la máscara del bloque es /24, /24 + /4 = /28.

##### b. Indique la dirección de subred de las primeras 9 subredes.

Como tenemos 4 bits para host, cada subred va en incrementos de 2<sup>4</sup> = 16.

1. 195.200.45.0/28
2. 195.200.45.16/28
3. 195.200.45.32/28
4. 195.200.45.48/28
5. 195.200.45.64/28
6. 195.200.45.80/28
7. 195.200.45.96/28
8. 195.200.45.112/28
9. 195.200.45.128/28

##### c. Seleccione una e indique dirección de broadcast y rango de direcciones asignables en esa subred.

Selecciono la primera.

Su dirección de broadcast es la dirección anterior a la siguiente subred: **195.200.45.15/28**

El rango de direcciones asignables en la primer subred es **195.200.45.1/28 - 195.200.45.14/28**

### 9. Dado el siguiente gráfico:

![Gráfico topología](https://i.imgur.com/egUCnIh.png)

##### a. Verifique si es correcta la asignación de direcciones IP y, en caso de no serlo, modifique la misma para que lo sea.

La dirección 172.26.22.3 para la interfaz eth1 del router A está mal, ya que esa es la dirección de broadcast de esa subred, y por ende no puede asignarse. La dirección correcta para esa interfaz de ese router sería 172.26.22.2

La dirección 172.17.10.17 para la interfaz eth1 del router C también está mal, ya que esa dirección está fuera de la subred (172.17.10.0/28). Una dirección correcta sería por ejemplo 172.17.10.1

##### b. ¿Cuántos bits se tomaron para hacer subredes en la red 10.0.10.0/24? ¿Cuántas subredes se podrían generar?

La red 10.0.10.0 es clase A. Las redes clase A tienen máscara por defecto /8: /24 - /8 = /16.

Se usaron 16 bits para hacer subredes.

Se podrían generar 2<sup>16</sup> subredes.

##### c. Para cada una de las redes utilizadas indique si son públicas o privadas.

IPs privadas de Clase A: 10.0.0.0 – 10.255.255.255
IPs privadas de Clase B: 172.16.0.0 – 172.31.255.255
IPs privadas de Clase C: 192.168.0.0 – 192.168.255.255

-   Red 191.26.145.0/24 -> Pública
-   Red 172.26.22.0/30 -> Privada
-   Red 172.17.10.0/28 -> Privada
-   Red 192.168.5.0/24 -> Privada
-   Red 10.0.10.0/24 -> Privada

## CIDR

### 10. ¿Qué es CIDR (Class Interdomain routing)? ¿Por qué resulta útil?

CIDR es un método de alocación de direcciones IP y ruteo alternativo a las clases. Es más flexible y útil ya que permite el uso de máscaras de longitud variable lo cual reduce masivamente el desperdicio de direcciones.

### 11. ¿Cómo publicaría un router las siguientes redes si se aplica CIDR?

198.10.1.0/24
198.10.0.0/24
198.10.3.0/24
198.10.2.0/24

Un router sumarizaría estas redes en una sola para maximizar la eficiencia. Esto se logra agrupándolas a partir del octeto donde difieren, y ajustando la máscara.

En las direcciones dadas, los primeros dos octetos son exactamente iguales, por ende los ignoramos. Luego miramos los últimos 2 octetos pasando a binario:

0000 0001 . 0000 0000
0000 0000 . 0000 0000
0000 0011 . 0000 0000
0000 0010 . 0000 0000

Podemos ver que las direcciones **son iguales hasta el bit 22**. Por ende la sumarización final es: **198.10.0.0/22**

### 12. Listar las redes involucradas en los siguientes bloques CIDR:

Es lo mismo que subnettear un bloque.

● 200.56.168.0/21

Podrían ser por ejemplo /22 - /21 = /1 = 2 redes:

200.56.1010 1**0**00.0 -> 200.56.168.0/22
200.56.1010 1**1**00.0 -> 200.56.172.0/22

● 195.24.0.0/13

Podrían ser por ejemplo /14 - /13 = /1 = 2 redes:

195.0001 1**0**00.0.0 -> 195.24.0.0/14
195.0001 1**1**00.0.0 -> 195.28.0.0/14

● 195.24/13

Es igual al anterior ya que 195.24 es sinónimo de 195.24.0.0

### 13. El bloque CIDR 128.0.0.0/2 o 128/2, ¿Equivale a listar todas las direcciones de red de clase B? ¿Cuál sería el bloque CIDR que agrupa todas las redes de clase A?

El bloque CIDR 128.0.0.0/2 es clase B que tiene /16 como máscara default. Es decir, tenemos /16 - /2 = /14 -> 14 bits para la parte de red. Lo cual es condice efectivamente con todas las direcciones de red de la clase B.

El bloque CIDR que agrupa todas las redes de clase A es 0.0.0.0/1 ya que
la clase A tiene máscara default /8 - 7 bits que son la cantidad de redes de clase A.

## VLSM

### 14. ¿Qué es y para qué se usa VLSM?

VLSM (Variable Length Subnet Masking) es una técnica y mejora del subnetting tradicional que divide subredes en base a máscaras de longitud variable, es decir que podemos tener por ejemplo:

Red A

Subred A1/X
Subred A2/Y
Subred A3/Z

### 15. Describa, con sus palabras, el mecanismo para dividir subredes utilizando VLSM.

Si tengo una red que quiero subdividir en 3 subredes y una de ellas necesita 60 hosts, otra 30, y la última 4, puedo usar máscara /26 para la primera, /27 para la segunda, y /30 para la última, minimizando el desperdicio.

### 16. Suponga que trabaja en una organización que tiene la red que se ve en el gráfico y debe armar el direccionamiento para la misma, minimizando el desperdicio de direcciones IP. Dicha organización posee la red 205.10.192.0/19, que es la que usted deberá utilizar.

![Gráfico topología](https://i.imgur.com/J2KAf4l.png)

##### a. ¿Es posible asignar las subredes correspondientes a la topología utilizando subnetting sin VLSM? Indique la cantidad de hosts que se desperdicia en cada subred.

Tenemos 5 redes: Red A, Red B, Red C, Red D, y Red punto a punto entre los 2 routers. Esto significa que necesitamos 3 bits para la parte de red, por ende -> /19 + /3 = /22, y nos quedan 10 bits para host.

Como 2<sup>10</sup> = 1024, y esto ni siquiera nos alcanza para asignar los hosts de la Red C por sí sola, podemos ver claramente que no es posible hacer esta asignación sin VLSM.

Si la máscara de la red original fuera /18 sí nos alcanzaría, y en ese caso el desperdicio sería:

2<sup>11</sup> = 2048

-   Red A: 2048 - 128 = 1920
-   Red B: 2048 - 20 = 2028
-   Red C: 2048 - 1530 = 518
-   Red D: 2048 - 7 = 2041
-   Red punto a punto: 2048 - 2 = 2046

##### b. Asigne direcciones a todas las redes de la topología. Tome siempre en cada paso la primera dirección de red posible.

Ahora vamos a utilizar la red original 205.10.192.0/19 para asignar direcciones a las 5 redes utilizando VLSM, yendo de la red más grande a las más chica.

Red C: 1530 hosts, por ende necesita 11 bits para la parte de host -> /21
Red A: 128 hosts, por ende necesita 8 bits para la parte de host -> /24
Red B: 20 hosts, por ende necesita 5 bits para la parte de host -> /27
Red D: 7 hosts, por ende necesita 4 bits para la parte de host -> /28
Red punto a punto: 2 hosts, por ende necesita 2 bits para la parte de host -> /30

Subnetteo la red original 205.10.192.0/19:

-   205.10. 110**0**0000 .0 -> 205.10.192.0/20 -> Subnetteo
-   205.10. 110**1**0000 .0 -> 205.10.208.0/20 -> Libre

Subnetteo la red 205.10.192.0/20:

-   205.10. 1100**0**000 .0 -> **205.10.192.0/21** -> **La asigno a la red C**
-   205.10. 1100**1**000 .0 -> 205.10.200.0/21 -> Subnetteo

Subnetteo la red 205.10.200.0/21:

-   205.10. 1100100**0** .0 -> **205.10.200.0/24** -> **La asigno a la red A**
-   205.10. 1100100**1** .0 -> 205.10.201.0/24 -> Subnetteo

    205.10.202.0/24 -> Libre
    205.10.203.0/24 -> Libre
    205.10.204.0/24 -> Libre
    205.10.205.0/24 -> Libre
    205.10.206.0/24 -> Libre
    205.10.207.0/24 -> Libre

Subnetteo la red 205.10.201.0/24:

-   205.10.201. 00**0**00000 -> **205.10.201.0/27** -> **La asigno a la red B**
-   205.10.201. 00**1**00000 -> 205.10.201.32/27 -> Subnetteo

    205.10.201.64/27 -> Libre
    205.10.201.96/27 -> Libre
    205.10.201.128/27 -> Libre
    205.10.201.160/27 -> Libre
    205.10.201.192/27 -> Libre
    205.10.201.224/27 -> Libre

Subnetteo la red 205.10.201.32/27:

-   205.10.201. 001**0**0000 -> **205.10.201.32/28** -> **La asigno a la red D**
-   205.10.201. 001**1**0000 -> 205.10.201.48/28 -> Subnetteo

Subnetteo la red 205.10.201.48/28:

-   205.10.201. 00110**0**00 -> **205.10.201.48/30** -> **La asigno a la red punto a punto**
-   205.10.201. 00110**1**00 -> 205.10.201.52/30 -> Libre

##### c. Para mantener el orden y el inventario de direcciones disponibles, haga un listado de todas las direcciones libres que le quedaron, agrupándolas utilizando CIDR.

Redes libres que quedaron:

-   205.10.208.0/20
-   205.10.201.52/30

No se pueden agrupar vía CIDR porque tienen distinta máscara.

-   205.10.202.0/24
-   205.10.203.0/24
-   205.10.204.0/24
-   205.10.205.0/24
-   205.10.206.0/24
-   205.10.207.0/24

Las agrupo (tercer octeto):

1100 1010
1100 1011
1100 1100
1100 1101
1100 1110
1100 1111

-> **205.10.128.0/21**

-   205.10.201.64/27
-   205.10.201.96/27
-   205.10.201.128/27
-   205.10.201.160/27
-   205.10.201.192/27
-   205.10.201.224/27

Las agrupo (cuarto octeto):

0100 0000
0110 0000
1000 0000
1010 0000
1100 0000
1110 0000

-> **205.10.201.0/24**

##### d. Asigne direcciones IP a todas las interfaces de la topología que sea posible.

-   Red A: Router_A_eth0 = 205.10.200.1/24
-   Red B: Router_A_eth1 = 205.10.201.1/27
-   Red C: Router_A_eth2 = 205.10.192.1/21
-   Red punto a punto: Router_A_eth3 = 205.10.201.49/30
-   Red punto a punto: Router_B_eth0 = 205.10.201.50/30
-   Red D: Router_B_eth1 = 205.10.201.33/28

### 17. Utilizando la siguiente topología y el bloque asignado, arme el plan de direccionamiento IPv4 teniendo en cuenta las siguientes restricciones:

![Gráfico topología](https://i.imgur.com/DUd3eb5.png)

##### a. Utilizar el bloque IPv4 200.100.8.0/22.

##### b. La red A tiene 125 hosts y se espera un crecimiento máximo de 20 hosts.

##### c. La red X tiene 63 hosts.

##### d. La red B cuenta con 60 hosts

##### e. La red Y tiene 46 hosts y se espera un crecimiento máximo de 18 hosts.

##### f. En cada red, se debe desperdiciar la menor cantidad de direcciones IP posibles. En este sentido, las redes utilizadas para conectar los routers deberán utilizar segmentos de red /30 de modo de desperdiciar la menor cantidad posible de direcciones IP.

Vamos a utilizar el bloque dado (200.100.8.0/22) para asignar direcciones a las 9 redes utilizando VLSM, yendo de la red más grande a las más chica.

-   Red A: 145 hosts, por ende necesita 8 bits para la parte de host -> /24
-   Red Y: 64 hosts, por ende necesita 7 bits para la parte de host -> /25
-   Red X: 63 hosts, por ende necesita 7 bits para la parte de host -> /25
-   Red B: 60 hosts, por ende necesita 6 bits para la parte de host -> /26
-   Red punto a punto n1 a n3: 2 hosts, por ende necesita 2 bits para la parte de host -> /30
-   Red punto a punto n3 a n4: 2 hosts, por ende necesita 2 bits para la parte de host -> /30
-   Red punto a punto n1 a n2: 2 hosts, por ende necesita 2 bits para la parte de host -> /30
-   Red punto a punto n1 a n4: 2 hosts, por ende necesita 2 bits para la parte de host -> /30
-   Red punto a punto n2 a n4: 2 hosts, por ende necesita 2 bits para la parte de host -> /30

Subnetteo la red original 200.100.8.0/22:

-   200.100. 0000100**0** .0 -> **200.100.8.0/24** -> **La asigno a la red A**
-   200.100. 0000100**1** .0 -> 200.100.9.0/24 -> Subnetteo

    200.100.10.0/24 -> Libre
    200.100.11.0/24 -> Libre

Subnetteo la red 200.100.9.0/24:

-   200.100. 00001001 .0 -> **200.100.9.0/25** -> **La asigno a la red Y**
-   200.100. 00001001 .1 -> **200.100.9.128/25** -> **La asigno a la red X**

Subnetteo la red 200.100.10.0/24:

-   200.100. 00001010 .00 -> **200.100.10.0/26** -> **La asigno a la red B**
-   200.100. 00001010 .01 -> 200.100.10.64/26 -> Subnetteo

    200.100.10.128/26 -> Libre
    200.100.10.192/26 -> Libre

Subnetteo la red 200.100.10.64/26:

-   De /26 a /30 tenemos 4 bits, por ende tendremos 2<sup>4</sup> = 16 subredes /30, y asignamos las primeras 5 a las redes punto a punto.
-   **200.100.10.64/30** -> **La asigno a la red punto a punto n1 a n3**
-   **200.100.10.68/30** -> **La asigno a la red punto a punto n3 a n4**
-   **200.100.10.72/30** -> **La asigno a la red punto a punto n1 a n2**
-   **200.100.10.76/30** -> **La asigno a la red punto a punto n1 a n4**
-   **200.100.10.80/30** -> **La asigno a la red punto a punto n2 a n4**
-   200.100.10.84/30 -> Libre
-   200.100.10.88/30 -> Libre
-   200.100.10.92/30 -> Libre
-   200.100.10.96/30 -> Libre
-   200.100.10.100/30 -> Libre
-   200.100.10.104/30 -> Libre
-   200.100.10.108/30 -> Libre
-   200.100.10.112/30 -> Libre
-   200.100.10.116/30 -> Libre
-   200.100.10.120/30 -> Libre
-   200.100.10.124/30 -> Libre

### 18. Asigne direcciones IP en los equipos de la topología según el plan anterior.

Como es buena práctica, asignamos la primera dirección utilizable de cada red al **router**.

Como el enunciado no las muestra, asumo el siguiente orden de las interfaces de los routers:

-   n1: eth0 a la izquierda, eth1 a la derecha, eth2 abajo, eth3 en diagonal hacia abajo.
-   n2: eth0 arriba, eth1 a la izquierda, eth2 abajo.
-   n3: eth0 arriba, eth1 a la derecha.
-   n4: eth0 a la izquierda, eth1 hacia abajo, eth2 a la derecha, eth3 hacia arriba, eth4 en diagonal hacia arriba.

| Red     | Dispositivo | Interfaz | Dirección IP     |
| ------- | ----------- | -------- | ---------------- |
| A       | n1          | eth0     | 200.100.8.1/24   |
| A       | n10         | -        | 200.100.8.2/24   |
| Y       | n4          | eth2     | 200.100.9.1/25   |
| Y       | n13         | -        | 200.100.9.2/25   |
| X       | n2          | eth0     | 200.100.9.129/25 |
| X       | n11         | -        | 200.100.9.130/25 |
| X       | n12         | -        | 200.100.9.131/25 |
| B       | n4          | eth1     | 200.100.10.1/26  |
| B       | n14         | -        | 200.100.10.2/26  |
| B       | n15         | -        | 200.100.10.3/26  |
| B       | n16         | -        | 200.100.10.4/26  |
| n1 a n3 | n1          | eth2     | 200.100.10.65/30 |
| n1 a n3 | n3          | eth0     | 200.100.10.66/30 |
| n3 a n4 | n3          | eth1     | 200.100.10.69/30 |
| n3 a n4 | n4          | eth0     | 200.100.10.70/30 |
| n1 a n2 | n1          | eth1     | 200.100.10.73/30 |
| n1 a n2 | n2          | eth1     | 200.100.10.74/30 |
| n1 a n4 | n1          | eth3     | 200.100.10.77/30 |
| n1 a n4 | n4          | eth4     | 200.100.10.78/30 |
| n2 a n4 | n2          | eth2     | 200.100.10.81/30 |
| n2 a n4 | n4          | eth3     | 200.100.10.82/30 |

## ICMP y Configuraciones IP

### 19. Describa qué es y para qué sirve el protocolo ICMP.

ICMP (Internet Control Message Protocol) es un protocolo de la capa de red que transmite mensajes de error a través de los dispositivos de la red. Es muy útil más que nada para informar sobre congestión o hosts inalcanzables.

##### a. Analice cómo funciona el comando ping.

El comando `ping` hace que el emisor envíe un Echo Request al receptor y éste conteste con un Echo Reply. A partir de esto se calcula el tiempo mínimo, máximo y promedio de respuesta. En caso de no recibir respuesta, la red es inalcanzable.

###### i. Indique el tipo y código ICMP que usa el ping.

-   Echo Request
    -   Tipo 8.
    -   Código 0.

###### ii. Indique el tipo y código ICMP que usa la respuesta de un ping.

-   Echo Reply
    -   Tipo 0.
    -   Código 0.

##### b. Analice cómo funcionan comandos como traceroute/tracert de Linux/Windows y cómo manipulan el campo TTL de los paquetes IP.

El comando `traceroute` muestra el camino o ruta de los paquetes desde nuestra PC a un destino particular. Este comando incrementa el TTL por cada iteración: empieza en 0 y en cada router se va incrementando, identificando así el camino.

##### c. Indique la cantidad de saltos realizados desde su computadora hasta el sitio www.nasa.gov. Analice:

Se ven 14 saltos.

###### i. Cómo hacer para que no muestre el nombre del dominio asociado a la IP de cada salto.

Para ocultar el nombre de dominio asociado a la IP de cada salto tenemos la opción `-d`.

###### ii. La razón de la aparición de \* en parte o toda la respuesta de un salto.

El asterisco (\*) en parte o toda la respuesta de un salto significa que, o el paquete se perdió o el router está configurado para no responder con ICMP, ocultándose de los resultados. Podría ocurrir por Firewall por ejemplo.

##### d. Verifique el recorrido hacia los servidores de nombre del dominio unlp.edu.ar. En base al recorrido realizado, ¿podría confirmar cuál de ellos toma un camino distinto?

???

### 20. ¿Para que se usa el bloque 127.0.0.0/8? ¿Qué PC responde a los siguientes comandos?

El bloque 127.0.0.0/8 se usa para loopback, es decir se reserva para comunicaciones locales dentro de la misma PC (un proceso de la PC se quiere comunicar con otro proceso de la misma PC por ejemplo).

##### a. ping 127.0.0.1

Responde 127.0.0.1

##### b. ping 127.0.54.43

Responde 127.0.54.43

Ambas son IPs localhost ya que ambas pertenecen al bloque de loopback.

### 21. Investigue para qué sirven los comandos ifconfig y route. ¿Qué comandos podría utilizar en su reemplazo? Inicie una topología con CORE, cree una máquina y utilice en ella los comandos anteriores para practicar sus diferentes opciones, mínimamente:

● Configurar y quitar una dirección IP en una interfaz.
● Ver la tabla de ruteo de la máquina.

El comando `ifconfig` muestra la configuración de interfaces de red de nuestra PC, por ejemplo las direcciones IP que tenemos asignadas para cada interfaz.

El comando `route` muestra la configuración de la tabla de ruteo de nuestra PC.

Ambos comandos fueron reemplazados por el comando `ip`.

-   Configurar una interfaz en una IP:
    `ifconfig eth0 192.168.1.0 netmask 255.255.255.0`

-   Quitar una IP de una interfaz:
    `ifconfig eth0 0.0.0.0`

-   Ver la tabla de ruteo de la máquina:
    `route`
    `route -d` (sin nombres de dominio)

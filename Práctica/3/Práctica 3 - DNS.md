<center>

# Práctica 3 - DNS

</center>

### 1. Investigue y describa cómo funciona el DNS. ¿Cuál es su objetivo?

El sistema de DNS es un sistema distribuido de forma jerárquica y conformado por muchos servidores a lo largo del planeta. Cada servidor se ocupa de una parte de la jerarquía de nombres. De forma resumida, funciona como un árbol. Cuando un cliente necesita la IP de un nombre de dominio, se va consultando desde los servidores más generales (root servers) y bajando sub-dominio por sub-dominio hasta obtener la dirección.

DNS tiene como objetivo traducir nombres de dominio a direcciones IP y de esta forma lograr que los usuarios puedan abstraerse de las IPs de los sitios a los que quieren acceder, ingresando simplemente estos nombres de dominio en la barra de búsqueda del navegador y no preocupándose por recordar secuencias de números. Además de esto, DNS permite que los nombres de dominio posean más de una IP.

### 2. ¿Qué es un root server? ¿Qué es un generic top-level domain (gtld)?

Un root server es un servidor DNS que se encuentra en la cima de la jerarquía. Responde con la lista de servidores autoritativos para cualquier TLD (Top Level Domain). Existen 13 de estos servidores distribuidos en el mundo.

Un Top Level Domain es un sub-dominio que se encuentra en la siguiente zona luego de la zona root. Se dividen en Country, Generic, y otros.

Un Generic Top Level Domain o GLTD es un TLD genérico, es decir que indica el propósito general de la página. Por ejemplo .com para comercial, .net para network, etc.

Resumiendo, cuando tenemos un nombre de dominio, los "." (puntos) separan los sub-dominios o zonas de la jerarquía. Además, a la derecha del todo, luego de la última palabra, hay un punto implícito. Por ejemplo:

```
www.google.com

"." es la zona root.
"com" es la zona com (GTLD).
"google" es la zona google.
"www" es la zona www.
```

### 3. ¿Qué es una respuesta del tipo autoritativa?

Una respuesta autoritativa es una que proviene de un servidor autoritativo, y éste es un servidor que posee toda la información actualizada de una zona determinada y puede producir cambios sobre la misma. Los servidores autoritativos también suelen llamarse servidores de nombres o nameservers.

Por ejemplo, un servidor autoritativo para example.com podría ser ns1.example.com.

### 4. ¿Qué diferencia una consulta DNS recursiva de una iterativa?

Una consulta DNS es un pedido que realiza un cliente a un servidor de DNS para traducir un nombre de dominio a su IP.

Una consulta DNS recursiva es aquella en la cual el servidor de DNS consultado primero chequea su caché, y si no encuentra la IP ahí, procede a consultar otros servidores de DNS automáticamente hasta encontrar la información pedida o hasta determinar que el nombre de dominio no existe.

Una consulta DNS iterativa es aquella en la cual el servidor DNS consultado nuevamente primero chequea su caché, y si no encuentra la IP ahí, le devuelve al cliente uno o más servidores DNS alternativos a quienes consultar, no los consulta él.

### 5. ¿Qué es el resolver?

El resolver es el primer paso en la resolución DNS y es previo a los servidores DNS propiamente dichos.

Se trata de un componente de software integrado (una rutina, proceso o librería) en el sistema operativo de cualquier cliente que utilice internet.

Cuando el cliente ingresa un nombre de dominio en el browser o cualquier otra aplicación que necesite conectarse a un servidor de internet, el resolver chequeará primero su caché y si no encuentra la IP ahí, procederá a realizar consultas recursivas (a no ser que esté configurado para hacer consultas iterativas, aunque es raro) al servidor DNS configurado en la máquina (el servidor local).

### 6. Describa para qué se utilizan los siguientes tipos de registros de DNS:

##### a. A

Mappea un nombre de dominio a una dirección IPv4.

##### b. MX

Especifica el servidor de email responsable por **recibir** emails para un determinado dominio.

##### c. PTR

Mappea una dirección IPv4 a un nombre de dominio.

##### d. AAAA

Mappea un nombre de dominio a una dirección IPv6.

##### e. SRV

Especifica la locación de determinados servicios de un dominio.

##### f. NS

Especifica uno o más servidores autoritativos para un dominio.

##### g. CNAME

Especifica un alias de un dominio.

##### h. SOA

Especifica información esencial de un dominio, incluyendo el servidor autoritativo primario, el correo del administrador de ese dominio, y varios parámetros más sobre transferencias de zona y caching.

##### i. TXT

Especifica información de texto plano asociada a un dominio.

### 7. En Internet, un dominio suele tener más de un servidor DNS, ¿por qué cree que esto es así?

Un dominio suele tener más de un servidor autoritativo por 3 razones principales:

1. Para tener mayor tolerancia a fallos, es decir que si deja de funcionar uno de los servidores, aún tenemos los demás.
2. Para balancear la carga y evitar que un único servidor se sature con demasiadas consultas.
3. Para poseer una mayor y más balanceada distribución geográfica y así reducir los tiempos de demora si las consultas se realizan de muchas partes del mundo.

### 8. Cuando un dominio cuenta con más de un servidor, uno de ellos es el primario (o maestro) y todos los demás son secundarios (o esclavos). ¿Cuál es la razón de que sea así?

Cuando un dominio tiene más de un servidor DNS, uno de ellos, el primario, es el que posee la información original y siempre actualizada. Todos los demás son secundarios o esclavos, es decir que copian la información del servidor primario regularmente. Esto es así porque sería imposible que haya varios servidores primarios. No se podrían actualizar ambos a exactamente el mismo tiempo, e incluso si se pudiera generaría ambiguedad, ya que los secundarios no sabrían cuál de los dos primarios copiar.

### 9. Explique brevemente en qué consiste el mecanismo de transferencia de zona y cuál es su finalidad.

La transferencia de zona consiste en que, periódicamente, el servidor primario envía copias de toda su información a todos sus servidores secundarios para que éstos estén actualizados y se mantenga la consistencia.

### 10. Imagine que usted es el administrador del dominio de DNS de la UNLP (unlp.edu.ar). A su vez, cada facultad de la UNLP cuenta con un administrador que gestiona su propio dominio (por ejemplo, en el caso de la Facultad de Informática se trata de info.unlp.edu.ar). Suponga que se crea una nueva facultad, Facultad de Redes, cuyo dominio será redes.unlp.edu.ar, y el administrador le indica que quiere poder manejar su propio dominio. ¿Qué debe hacer usted para que el administrador de la Facultad de Redes pueda gestionar el dominio de forma independiente? (Pista: investigue en qué consiste la delegación de dominios). Indicar qué registros de DNS se deberían agregar.

-   Soy administrador del dominio unlp.edu.ar.
-   info.unlp.edu.ar es manejada por otro administrador de forma independiente.
-   Tenemos un nuevo dominio, redes.unlp.edu.ar que nuevamente debe manejarse de forma independiente.
-   El servidor autoritativo primario de unlp.edu.ar posee, entre otros registros:

```
info.unlp.edu.ar IN NS ns1.info.unlp.edu.ar
ns1.info.unlp.edu.ar IN A X.X.X.X // IP del servidor autoritativo de informática.
```

Para que el administrador de redes.unlp.edu.ar pueda gestionar ese dominio de forma independiente debemos realizar una delegación de dominio:

1. En el servidor autoritativo primario de unlp.edu.ar agregamos los registros:

```
redes.unlp.edu.ar IN NS ns1.redes.unlp.edu.ar
ns1.redes.unlp.edu.ar IN A X.X.X.X // IP del servidor autoritativo de redes.
```

2. El administrador de la Facultad de Redes configura y administra ns1.redes.unlp.edu.ar.

### 11. Responda y justifique los siguientes ejercicios.

##### a. En la VM, utilice el comando dig para obtener la dirección IP del host www.redes.unlp.edu.ar y responda:

###### i. ¿La solicitud fue recursiva? ¿Y la respuesta? ¿Cómo lo sabe?

Tanto la solicitud como la respuesta fueron recursivas.

La solicitud fue recursiva porque podemos ver el flag "rd" activado, que significa Recursion Desired, es decir que el cliente pidió recursión.

La respuesta también fue recursiva porque podemos ver que efectivamente recibimos la respuesta que esperabamos, la IP, y no recibimos un puntero a otro servidor DNS para seguir consultando (respuesta iterativa).

###### ii. ¿Puede indicar si se trata de una respuesta autoritativa? ¿Qué significa que lo sea?

La respuesta fue efectivamente autoritativa, ya que el flag aa (Authoritative Answer) está presente.

Que la respuesta sea autoritativa significa que proviene del servidor que posee la información final, completa y precisa del dominio consultado.

###### iii. ¿Cuál es la dirección IP del resolver utilizado? ¿Cómo lo sabe?

La dirección IP del resolver utilizado es 172.28.0.29. Lo sé porque es la dirección que aparece en la parte final del output del comando ingresado.

##### b. ¿Cuáles son los servidores de correo del dominio redes.unlp.edu.ar? ¿Por qué hay más de uno y qué significan los números que aparecen entre MX y el nombre? Si se quiere enviar un correo destinado a redes.unlp.edu.ar, ¿a qué servidor se le entregará? ¿En qué situación se le entregará al otro?

Podemos averiguar los servidores de correo de redes.unlp.edu.ar con el comando "dig redes.unlp.edu.ar MX":

```
redes.unlp.edu.ar. 86400 IN MX 10 mail2.redes.unlp.edu.ar.
redes.unlp.edu.ar. 86400 IN MX 5 mail.redes.unlp.edu.ar.
```

Hay más de uno por la misma razón que suele haber más de un servidor autoritativo: tolerancia a fallos, redundancia, balanceo de carga, etc.

Los números que aparecen entre MX y el nombre simbolizan la prioridad. Cuanto más bajo el valor, mayor prioridad tiene ese servidor de mail.

Si se quiere enviar un correo destinado a redes.unlp.edu.ar se lo entregará a mail.redes.unlp.edu.ar por tener mayor prioridad (menor valor: 5). Solo en caso de que este servidor no se encuentre disponible se enviará el correo al otro (mail2).

##### c. ¿Cuáles son los servidores de DNS del dominio redes.unlp.edu.ar?

Podemos averiguar los servidores de DNS de redes.unlp.edu.ar con el comando "dig redes.unlp.edu.ar NS":

```
redes.unlp.edu.ar. 86400 IN NS ns-sv-a.redes.unlp.edu.ar.
redes.unlp.edu.ar. 86400 IN NS ns-sv-b.redes.unlp.edu.ar.
```

##### d. Repita la consulta anterior cuatro veces más. ¿Qué observa? ¿Puede explicar a qué se debe?

Al repetir la misma consulta una y otra vez, observo que cambia el id, la cookie, y a veces el orden de los dos servidores de DNS mencionados en el inciso c. A veces b aparece primero y luego a, y a veces lo opuesto.

Esto se debe al balanceo de carga.

##### e. Observe la información que obtuvo al consultar por los servidores de DNS del dominio. En base a la salida, ¿es posible indicar cuál de ellos es el primario?

A priori no puedo determinar cuál es el primario.

##### f. Consulte por el registro SOA del dominio y responda.

###### i. ¿Puede ahora determinar cuál es el servidor de DNS primario?

Podemos averiguar cuál es el servidor de DNS primario de redes.unlp.edu.ar con el comando "dig redes.unlp.edu.ar SOA":

```
redes.unlp.edu.ar. 86400 IN SOA ns-sv-b.redes.unlp.edu.ar. root.redes.unlp.edu.ar. 2020031700 604800 86400 2419200 86400
```

Si, ahora vemos claramente que el servidor B era el primario, ya que es el que aparece cuando consultamos por el SOA.

###### ii. ¿Cuál es el número de serie, qué convención sigue y en qué casos es importante actualizarlo?

El número de serie es 2020031700. El formato que sigue es: YYYY/MM/DD/nn donde:

-   Y es el año
-   M el mes
-   D el día
-   n el número de actualización de ese día que se actualiza cuando hay cambios en el dominio. Por ejemplo, si fuera "3" eso significa que se realizaron 3 cambios ese día.

###### iii. ¿Qué valor tiene el segundo campo del registro? Investigue para qué se usa y cómo se interpreta el valor.

El segundo campo del registro tiene valor root.redes.unlp.edu.ar.

Este campo se usa como email del admin del dominio poniendo @ luego de root.

###### iv. ¿Qué valor tiene el TTL de caché negativa y qué significa?

El último campo del SOA es el TTL de caché negativa y tiene valor 86400 que son 24 horas. Significa que las consultas de registros que no existan en el dominio se almacenarán en caché por 24 horas como máximo.

##### g. Indique qué valor tiene el registro TXT para el nombre saludo.redes.unlp.edu.ar. Investigue para qué es usado este registro.

Podemos averiguar cuál es el valor del registro TXT de saludo.redes.unlp.edu.ar con el comando "dig saludo.redes.unlp.edu.ar TXT":

```
saludo.redes.unlp.edu.ar. 86400 IN TXT "HOLA"
```

El registro tiene valor "HOLA". Este registro es usado como documentación útil del servidor DNS.

##### h. Utilizando dig, solicite la transferencia de zona de redes.unlp.edu.ar, analice la salida y responda.

Podemos solicitar la transferencia de zona de redes.unlp.edu.ar con el comando:

```
dig AXFR redes.unlp.edu.ar
```

###### i. ¿Qué significan los números que aparecen antes de la palabra IN? ¿Cuál es su finalidad?

Los números antes de IN son los TTL (Time To Live). Son la cantidad de segundos que se mantendrá el valor actual de ese registro en caché.

Su finalidad es reducir el tráfico repetitivo o excesivo.

###### ii. ¿Cuántos registros NS observa? Compare la respuesta con los servidores de DNS del dominio redes.unlp.edu.ar que dio anteriormente. ¿Puede explicar a qué se debe la diferencia y qué significa?

Observo 4 registros NS, a diferencia de 2 anteriormente. La diferencia se debe a que un servidor de DNS puede tener registros NS que no son autoritativos para su dominio, si no para otros dominios también. Estos últimos no se muestran cuando hacemos "dig dominio NS", pero si se muestran en la transferencia de zona ya que ésta copia todos los registros.

##### i. Consulte por el registro A de www.redes.unlp.edu.ar y luego por el registro A de www.practica.redes.unlp.edu.ar. Observe los TTL de ambos. Repita la operación y compare el valor de los TTL de cada uno respecto de la respuesta anterior. ¿Puede explicar qué está ocurriendo? (Pista: observar los flags será de ayuda).

-   dig www.redes.unlp.edu.ar A:

```
www.redes.unlp.edu.ar 300 IN A 172.28.0.50
```

-   dig www.practica.redes.unlp.edu.ar A:

```
www.practica.redes.unlp.edu.ar 60 IN A 172.28.0.10
```

Al volver a realizar la consulta en el primer comando, se obtiene la misma respuesta. Sin embargo, al volver a realizar la consulta del segundo comando, podemos ver que el valor del TTL ha decrementado. Lo que está ocurriendo es que la respuesta que estamos obteniendo en el segundo comando proviene de caché y no de un servidor autoritativo (por eso no vemos el flag "aa"). Como proviene de caché, el TTL se decrementa segundo a segundo.

##### j. Consulte por el registro A de www.practica2.redes.unlp.edu.ar. ¿Obtuvo alguna respuesta? Investigue sobre los códigos de respuesta de DNS. ¿Para qué son utilizados los mensajes NXDOMAIN y NOERROR?

No obtuve una respuesta como tal (ANSWER: 0) y recibí status: NXDOMAIN.

Existen 4 status principales:

1. NOERROR significa que la query tuvo éxito (el dominio pedido existe) pero el registro solicitado puede no existir.
2. NXDOMAIN significa que el dominio solicitado no existe.
3. SERVFAIL significa que hay un problema técnico con los servidores de DNS. Puede ser un problema de firewall.
4. REFUSED significa que el servidor rechazó la consulta por políticas de seguridad y/o autorización.

### 12. Investigue los comandos nslookup y host. ¿Para qué sirven? Intente con ambos comandos obtener:

-   Dirección IP de www.redes.unlp.edu.ar.
-   Servidores de correo del dominio redes.unlp.edu.ar.
-   Servidores de DNS del dominio redes.unlp.edu.ar.

nslookup realiza consultas a un sistema de nombres de dominio.

-   nslookup www.redes.unlp.edu.ar: la IP es 172.28.0.50
-   nslookup -query=MX redes.unlp.edu.ar: el servidor de correo prioritario es mail.redes.unlp.edu.ar y el secundario mail2.redes.unlp.edu.ar
-   nslookup -query=NS redes.unlp.edu.ar: devuelve el ns-sv-b y ns-sv-a.

host es como el anterior pero simplificado.

-   host www.redes.unlp.edu.ar: la IP es 172.28.0.50
-   host -t MX redes.unlp.edu.ar: idem lookup
-   host -t NS redes.unlp.edu.ar: idem lookup

### 13. ¿Qué función cumple en Linux/Unix el archivo /etc/hosts o en Windows el archivo \WINDOWS\system32\drivers\etc\hosts?

El archivo **hosts** mappea nombres de dominio a IPs de forma local, evitando que se tenga que realizar una consulta DNS si es que el hostname requerido se encuentra en el archivo.

### 14. Abra el programa Wireshark para comenzar a capturar el tráfico de red en la interfaz con IP 172.28.0.1. Una vez abierto realice una consulta DNS con el comando dig para averiguar el registro MX de redes.unlp.edu.ar y luego, otra para averiguar los registros NS correspondientes al dominio redes.unlp.edu.ar. Analice la información proporcionada por dig y compárelo con la captura.

??

### 15. Dada la siguiente situación: “Una PC en una red determinada, con acceso a Internet, utiliza los servicios de DNS de un servidor de la red”. Analice:

##### a. ¿Qué tipo de consultas (iterativas o recursivas) realiza la PC a su servidor de DNS?

Cuando la PC requiere saber la IP de un dominio realizará una consulta **recursiva** a su servidor de DNS.

##### b. ¿Qué tipo de consultas (iterativas o recursivas) realiza el servidor de DNS para resolver requerimientos de usuario como el anterior? ¿A quién le realiza estas consultas?

El servidor de DNS (si no encuentra la IP en su caché) realiza consultas **iterativas** a otros servidores de DNS, empezando por los root servers, hasta encontrar la IP o determinar que no existe.

### 16. Relacione DNS con HTTP. ¿Se puede navegar si no hay servicio de DNS?

1. HTTP requiere IPs para transferir datos vía web, por ende es **posterior** a DNS.
2. DNS podría no usarse, técnicamente, ya que podemos establecer conexiones HTTP dando la IP explícitamente (por ejemplo, ingresándola en la barra de búsqueda del navegador) en vez de dar un nombre de dominio y que se tenga que realizar el proceso de consultas DNS. Por supuesto que esto sería extremadamente engorroso ya que tendríamos que saber la IP de cualquier sitio que queremos acceder.

### 17. Observar el siguiente gráfico y contestar:

![Gráfico topología DNS](https://i.imgur.com/7qW2Uir.png)

##### Si la PC-A, que usa como servidor de DNS a "DNS Server", desea obtener la IP de www.unlp.edu.ar, cuáles serían, y en qué orden, los pasos que se ejecutarán para obtener la respuesta. ¿Dónde es recursiva la consulta? ¿Y dónde iterativa?

Pasos en orden:

1. PC-A le pide a DNS Server que resuelva la IP de www.unlp.edu.ar (consulta recursiva).
2. DNS Server, como no tiene la respuesta en su caché, realiza una consulta iterativa al root server A (el de la izquierda en la imágen, por ser el más cercano) requiriendo el registro A de www.unlp.edu.ar.
3. Root Server A le devuelve los NS de .ar: **a.dns.ar**.
4. DNS Server realiza una consulta iterativa a a.dns.ar requiriendo el registro A de www.unlp.edu.ar.
5. a.dns.ar le devuelve los NS de .edu.ar: **ns1.riu.edu.ar**.
6. DNS Server realiza una consulta iterativa a ns1.riu.edu.ar requiriendo el registro A de www.unlp.edu.ar.
7. ns1.riu.edu.ar le devuelve los NS de .unlp.edu.ar: **unlp.unlp.edu.ar**.
8. DNS Server realiza una consulta iterativa a unlp.unlp.edu.ar requiriendo el registro A de www.unlp.edu.ar.
9. unlp.unlp.edu.ar le devuelve el registro A de www.unlp.edu.ar.
10. DNS Server le devuelve a PC-A la IP del dominio solicitado.

### 18. ¿A quién debería consultar para que la respuesta sobre www.google.com sea autoritativa?

Para que la respuesta sobre www.google.com sea autoritativa debería consultarle a cualquiera de sus servidores autoritativos, valga la redundancia. Para conocer cuáles son sus servidores autoritativos podemos hacer "dig www.google.com NS".

### 19. ¿Qué sucede si al servidor elegido en el paso anterior se lo consulta por www.info.unlp.edu.ar? ¿Y si la consulta es al servidor 8.8.8.8?

Si al servidor autoritativo de www.google.com del paso anterior se lo consulta por www.info.unlp.edu.ar:

```
dig @ns1.google.com www.info.unlp.edu.ar
```

No recibimos respuesta porque este nameserver no conoce a ese dominio.

Sin embargo, si hacemos:

```
dig @8.8.8.8 www.info.unlp.edu.ar
```

Sí recibimos una respuesta válida, ya que 8.8.8.8 es un servicio de DNS público, gratuito y global que puede resolver IPs de cualquier dominio (Open Name Server).

### Ejercicio de parcial: 20. En base a la siguiente salida de dig, conteste las consignas. Justifique en todos los casos.

![Gráfico resultado comando dig](https://i.imgur.com/7koKi6v.png)

##### a. Complete las líneas donde aparece \_\_ con el registro correcto.

1: **MX**, porque en la sección ANSWER hay 2 servidores de mail y la sección QUESTION indica el tipo de registro que se consultó.
2 y 3: **MX**, porque se indica la prioridad (10 y 5 respectivamente).
4 5 6 y 7: **NS** porque es la sección autoritativa (AUTHORITY).
8 y 10: **A** porque se muestran direcciones IPv4 (4 números entre 0 y 255 separados por puntos).
9 y 11: **AAAA** porque se muestran direcciones IPv6 (IPs mucho más largas y complejas).

##### b. ¿Es una respuesta autoritativa? En caso de no serlo, ¿a qué servidor le preguntaría para obtener una respuesta autoritativa?

No, no es una respuesta autoritativa ya que el flag "aa" no aparece.

Para obtener una respuesta autoritativa le preguntaría a cualquiera de los 4 servidores NS de la sección AUTHORITY.

##### c. ¿La consulta fue recursiva? ¿Y la respuesta?

La consulta fue efectivamente recursiva ya que está el flag rd (Recursion Desired).

La respuesta también fue recursiva ya que está el flag ra (Recursion Available) y además respondió lo que se le pidió (los MX de ejemplo.com) y no pidió consultar a otro servidor (respuesta iterativa).

##### d. ¿Qué representan los valores 10 y 5 en las líneas (1) y (2).

Los valores 10 y 5 representan la prioridad de los mail servers. Como srv00 tiene un valor menor, tiene mayor prioridad, y se utilizará ese servidor siempre que esté disponible.

<center>

# Práctica 3 - DNS

</center>

### 1. Investigue y describa cómo funciona el DNS. ¿Cuál es su objetivo?

El sistema de DNS es un sistema distribuido de forma jerárquica y conformado por muchos servidores a lo largo del planeta. Cada servidor se ocupa de una parte de la jerarquía de nombres. De forma resumida, funciona como un árbol. Cuando un cliente necesita la IP de un nombre de dominio, se va consultando desde los servidores más generales (root servers) y bajando zona por zona hasta obtener la dirección.

DNS tiene como objetivo traducir nombres de dominio a direcciones IP y de esta forma lograr que los usuarios puedan abstraerse de las IPs de los sitios a los que quieren acceder, ingresando simplemente estos nombres de dominio en la barra de búsqueda del navegador y no preocupándose por recordar secuencias de números. Además de esto, DNS permite que los nombres de dominio posean más de una IP.

### 2. ¿Qué es un root server? ¿Qué es un generic top-level domain (gtld)?

Un root server es un servidor DNS que se encuentra en la cima de la jerarquía. Responde con la lista de servidores autoritativos para cualquier TLD (Top Level Domain). Existen 13 de estos servidores distribuidos en el mundo.

Un Top Level Domain es un nombre de dominio que se encuentra en la siguiente zona luego de la zona root. Se dividen en Country, Generic, y otros.

Un Generic Top Level Domain o GLTD es un TLD genérico, es decir que indica el propósito general de la página. Por ejemplo .com para comercial, .net para network, etc.

Resumiendo, cuando tenemos un nombre de dominio, los "." (puntos) separan los niveles o zonas de la jerarquía. Además, a la derecha del todo, luego de la última palabra, hay un punto implícito. Por ejemplo:

```
www.google.com

"." es el nivel 1 o zona root.
"com" es el nivel 2 o GTLD.
"google" es el nivel 3.
"www" es el nivel 4.
```

### 3. ¿Qué es una respuesta del tipo autoritativa?

Una respuesta autoritativa es una que proviene de un servidor autoritativo, y éste es un servidor que posee toda la información actualizada de un dominio determinado. Los servidores autoritativos también suelen llamarse servidores de nombres o nameservers.

Por ejemplo, un servidor autoritativo para example.com podría ser ns1.example.com.

### 4. ¿Qué diferencia una consulta DNS recursiva de una iterativa?

Una consulta DNS es un pedido que realiza un cliente a un servidor de DNS para traducir un nombre de dominio a su IP.

Una consulta DNS recursiva es aquella en la cual el servidor de DNS consultado primero chequea su caché, y si no encuentra la IP ahí, procede a consultar otros servidores de DNS automáticamente hasta encontrar la información pedida o hasta determinar que el nombre de dominio no existe.

Una consulta DNS iterativa es aquella en la cual el servidor DNS consultado nuevamente primero chequea su caché, y si no encuentra la IP ahí, le devuelve al cliente uno o más servidores DNS alternativos a quienes consultar, no los consulta él.

### 5. ¿Qué es el resolver?

El resolver es el primer paso en la resolución DNS y es previo a los servidores DNS propiamente dichos.

Se trata de un componente de software integrado en el sistema operativo de cualquier cliente que utilice internet.

Cuando el cliente ingresa un nombre de dominio en el browser o cualquier otra aplicación que necesite conectarse a un servidor de internet, el resolver chequeará primero su caché y si no encuentra la IP ahí, procederá a realizar consultas recursivas (a no ser que esté configurado para hacer consultas iterativas, aunque es raro).

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

### 11. Responda y justifique los siguientes ejercicios.

##### a. En la VM, utilice el comando dig para obtener la dirección IP del host www.redes.unlp.edu.ar y responda:

###### i. ¿La solicitud fue recursiva? ¿Y la respuesta? ¿Cómo lo sabe?

###### ii. ¿Puede indicar si se trata de una respuesta autoritativa? ¿Qué significa que lo sea?

###### iii. ¿Cuál es la dirección IP del resolver utilizado? ¿Cómo lo sabe?

##### b. ¿Cuáles son los servidores de correo del dominio redes.unlp.edu.ar? ¿Por qué hay más de uno y qué significan los números que aparecen entre MX y el nombre? Si se quiere enviar un correo destinado a redes.unlp.edu.ar, ¿a qué servidor se le entregará? ¿En qué situación se le entregará al otro?

##### c. ¿Cuáles son los servidores de DNS del dominio redes.unlp.edu.ar?

##### d. Repita la consulta anterior cuatro veces más. ¿Qué observa? ¿Puede explicar a qué se debe?

##### e. Observe la información que obtuvo al consultar por los servidores de DNS del dominio. En base a la salida, ¿es posible indicar cuál de ellos es el primario?

##### f. Consulte por el registro SOA del dominio y responda.

###### i. ¿Puede ahora determinar cuál es el servidor de DNS primario?

###### ii. ¿Cuál es el número de serie, qué convención sigue y en qué casos es importante actualizarlo?

###### iii. ¿Qué valor tiene el segundo campo del registro? Investigue para qué se usa y cómo se interpreta el valor.

###### iv. ¿Qué valor tiene el TTL de caché negativa y qué significa?

##### g. Indique qué valor tiene el registro TXT para el nombre saludo.redes.unlp.edu.ar. Investigue para qué es usado este registro.

##### h. Utilizando dig, solicite la transferencia de zona de redes.unlp.edu.ar, analice la salida y responda.

###### i. ¿Qué significan los números que aparecen antes de la palabra IN? ¿Cuál es su finalidad?

###### ii. ¿Cuántos registros NS observa? Compare la respuesta con los servidores de DNS del dominio redes.unlp.edu.ar que dio anteriormente. ¿Puede explicar a qué se debe la diferencia y qué significa?

##### i. Consulte por el registro A de www.redes.unlp.edu.ar y luego por el registro A de www.practica.redes.unlp.edu.ar. Observe los TTL de ambos. Repita la operación y compare el valor de los TTL de cada uno respecto de la respuesta anterior. ¿Puede explicar qué está ocurriendo? (Pista: observar los flags será de ayuda).

##### j. Consulte por el registro A de www.practica2.redes.unlp.edu.ar. ¿Obtuvo alguna respuesta? Investigue sobre los códigos de respuesta de DNS. ¿Para qué son utilizados los mensajes NXDOMAIN y NOERROR?

### 12. Investigue los comandos nslookup y host. ¿Para qué sirven? Intente con ambos comandos obtener:

-   Dirección IP de www.redes.unlp.edu.ar.
-   Servidores de correo del dominio redes.unlp.edu.ar.
-   Servidores de DNS del dominio redes.unlp.edu.ar.

### 13. ¿Qué función cumple en Linux/Unix el archivo /etc/hosts o en Windows el archivo \WINDOWS\system32\drivers\etc\hosts?

### 14. Abra el programa Wireshark para comenzar a capturar el tráfico de red en la interfaz con IP 172.28.0.1. Una vez abierto realice una consulta DNS con el comando dig para averiguar el registro MX de redes.unlp.edu.ar y luego, otra para averiguar los registros NS correspondientes al dominio redes.unlp.edu.ar. Analice la información proporcionada por dig y compárelo con la captura.

### 15. Dada la siguiente situación: “Una PC en una red determinada, con acceso a Internet, utiliza los servicios de DNS de un servidor de la red”. Analice:

##### a. ¿Qué tipo de consultas (iterativas o recursivas) realiza la PC a su servidor de DNS?

##### b. ¿Qué tipo de consultas (iterativas o recursivas) realiza el servidor de DNS para resolver requerimientos de usuario como el anterior? ¿A quién le realiza estas consultas?

### 16. Relacione DNS con HTTP. ¿Se puede navegar si no hay servicio de DNS?

### 17. Observar el siguiente gráfico y contestar:

![Gráfico topología DNS](https://i.imgur.com/7qW2Uir.png)

##### a. Si la PC-A, que usa como servidor de DNS a "DNS Server", desea obtener la IP de www.unlp.edu.ar, cuáles serían, y en qué orden, los pasos que se ejecutarán para obtener la respuesta.

##### b. ¿Dónde es recursiva la consulta? ¿Y dónde iterativa?

### 18. ¿A quién debería consultar para que la respuesta sobre www.google.com sea autoritativa?

### 19. ¿Qué sucede si al servidor elegido en el paso anterior se lo consulta por www.info.unlp.edu.ar? ¿Y si la consulta es al servidor 8.8.8.8?

### Ejercicio de parcial: 20. En base a la siguiente salida de dig, conteste las consignas. Justifique en todos los casos.

![Gráfico resultado comando dig](https://i.imgur.com/7koKi6v.png)

##### a. Complete las líneas donde aparece \_\_ con el registro correcto.

##### b. ¿Es una respuesta autoritativa? En caso de no serlo, ¿a qué servidor le preguntaría para obtener una respuesta autoritativa?

##### c. ¿La consulta fue recursiva? ¿Y la respuesta?

##### d. ¿Qué representan los valores 10 y 5 en las líneas (1) y (2).

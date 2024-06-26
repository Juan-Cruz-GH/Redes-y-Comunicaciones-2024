<h1 align="center">Práctica 4 - Correo Electrónico</h1>

### 1. ¿Qué protocolos se utilizan para el envío de mails entre el cliente y su servidor de correo? ¿Y entre servidores de correo?

Para el envío de mails entre el cliente y su servidor de correo se utiliza el protocolo SMTP que opera con TCP.

Entre servidores de correo también se utiliza SMTP. Esto es porque SMTP puede actuar tanto como cliente (mail server del emisor) así como servidor (mail server del destino).

### 2. ¿Qué protocolos se utilizan para la recepción de mails? Enumere y explique características y diferencias entre las alternativas posibles.

Existen 2 protocolos principales para la recepción de mails: POP3 e IMAP.

**POP3**:

-   Protocolo simple y limitado en ciertos aspectos.
-   Los emails se descargan desde al servidor al dispositivo local.
-   Una vez que se descargaron, se borran del servidor. Esto significa que no hay sincronización si utilizamos varios dispositivos para acceder a nuestra inbox.

**IMAP**:

-   Protocolo moderno y más complejo que POP3.
-   Los emails se guardan en el servidor y se sincronizan a través de todos los dispositivos que accedan a la inbox.
-   Como los emails no se borran a no ser que el usuario/cliente los borre, utiliza mucho más espacio físico que POP3.
-   Permite crear carpetas y demás customizaciones.

### 3. Utilizando la VM y teniendo en cuenta los siguientes datos, abra el cliente de correo (Thunderbird) y configure dos cuentas de correo. Una de las cuentas utilizará POP para solicitar al servidor los mails recibidos para la misma mientras que la otra utilizará IMAP. Al crear cada una de las cuentas, seleccionar Manual config y luego de configurar las mismas según lo indicado, ignorar advertencias por uso de conexión sin cifrado.

● Datos para POP
Cuenta de correo: alumnopop@redes.unlp.edu.ar
Nombre de usuario: alumnopop
Contraseña: alumnopoppass
Puerto: 110

● Datos para IMAP
Cuenta de correo: alumnoimap@redes.unlp.edu.ar
Nombre de usuario: alumnoimap
Contraseña: alumnoimappass
Puerto: 143

● Datos comunes para ambas cuentas
Servidor de correo entrante (POP/IMAP):
• Nombre: mail.redes.unlp.edu.ar
• SSL: None
• Autenticación: Normal password

Servidor de correo saliente (SMTP):
• Nombre: mail.redes.unlp.edu.ar
• Puerto: 25
• SSL: None
• Autenticación: Normal password

##### a. Verificar el correcto funcionamiento enviando un email desde el cliente de una cuenta a la otra y luego desde la otra responder el mail hacia la primera.

##### b. Análisis del protocolo SMTP

###### i. Utilizando Wireshark, capture el tráfico de red contra el servidor de correo mientras desde la cuenta alumnopop@redes.unlp.edu.ar envía un correo a alumnoimap@redes.unlp.edu.ar

###### ii. Utilice el filtro SMTP para observar los paquetes del protocolo SMTP en la captura generada y analice el intercambio de dicho protocolo entre el cliente y el servidor para observar los distintos comandos utilizados y su correspondiente respuesta. Ayuda: filtre por protocolo SMTP y sobre alguna de las líneas del intercambio haga click derecho y seleccione Follow TCP Stream. . .

![Wireshark](https://i.imgur.com/BuLwG5c.png)

![TCP Stream](https://i.imgur.com/mjcrQwj.png)

##### c. Usando el cliente de correo Thunderbird del usuario alumnopop@redes.unlp.edu.ar envíe un correo electrónico alumnoimap@redes.unlp.edu.ar el cual debe tener: un asunto, datos en el body y una imagen adjunta.

###### i. Verifique las fuentes del correo recibido para entender cómo se utiliza el header “Content-Type: multipart/mixed“ para poder realizar el envío de distintos archivos adjuntos.

El multipart/mixed indica que el body posee uno o más archivos adjuntos, cada uno con un Boundary que indica dónde terminan sus datos.

###### ii. Extraiga la imagen adjunta del mismo modo que lo hace el cliente de correo a partir de los fuentes del mensaje.

Podemos copiar y pegar el código en base64 que se encuentra en el mail en cualquier decodificador de base64 online para obtener la imagen, tal cual hace el cliente de correo.

### 4. Análisis del protocolo POP

##### a. Utilizando Wireshark, capture el tráfico de red contra el servidor de correo mientras desde la cuenta alumnoimap@redes.unlp.edu.ar le envía una correo a alumnopop@redes.unlp.edu.ar y mientras alumnopop@redes.unlp.edu.ar recepciona dicho correo.

##### b. Utilice el filtro POP para observar los paquetes del protocolo POP en la captura generada y analice el intercambio de dicho protocolo entre el cliente y el servidor para observar los distintos comandos utilizados y su correspondiente respuesta.

![Wireshark](https://i.imgur.com/Ffi1txZ.png)

![TCP Stream](https://i.imgur.com/QcqbnFF.png)

### 5. Análisis del protocolo IMAP

##### a. Utilizando Wireshark, capture el tráfico de red contra el servidor de correo mientras desde la cuenta alumnopop@redes.unlp.edu.ar le envía un correo a alumnoimap@redes.unlp.edu.ar y mientras alumnoimap@redes.unlp.edu.ar recepciona dicho correo.

##### b. Utilice el filtro IMAP para observar los paquetes del protocolo IMAP en la captura generada y analice el intercambio de dicho protocolo entre el cliente y el servidor para observar los distintos comandos utilizados y su correspondiente respuesta.

![Wireshark](https://i.imgur.com/cZvqgwH.png)

![TCP Stream](https://i.imgur.com/FD3Sxn8.png)

### 6. IMAP vs POP

##### a. Marque como leídos todos los correos que tenga en el buzón de entrada de alumnopop y de alumnoimap. Luego, cree una carpeta llamada POP en la cuenta de alumnopop y una llamada IMAP en la cuenta de alumnoimap. Asegúrese que tiene mails en el inbox y en la carpeta recientemente creada en cada una de las cuentas.

##### b. Cierre la sesión iniciada e ingrese nuevamente identificándose como usuario root y password packer, ejecute el cliente de correos. De esta forma, iniciará el cliente de correo con el perfil del superusuario (diferente del usuario con el que ya configuró las cuentas antes mencionadas). Luego configure las cuentas POP e IMAP de los usuarios alumnopop y alumnoimap como se describió anteriormente pero desde el cliente de correos ejecutado con el usuario root. Responda:

###### i. ¿Qué correos ve en el buzón de entrada de ambas cuentas? ¿Están marcados como leídos o como no leídos? ¿Por qué?

En la cuenta POP los mensajes se encuentran sin leer. Esto es porque POP3 no guarda el estado del servidor.

En la cuenta IMAP se muestra la inbox tal y como estaba en la otra cuenta, ya que sí guarda estado.

###### ii. ¿Qué pasó con las carpetas POP e IMAP que creó en el paso anterior?

La carpeta POP desapareció y los mensajes que tenía se movieron a inbox.

La carpeta IMAP se encuentra intacta.

Nuevamente, la razón es la mencionada en 6. b. i.

##### c. En base a lo observado. ¿Qué protocolo le parece mejor? ¿POP o IMAP? ¿Por qué? ¿Qué protocolo considera que utiliza más recursos del servidor? ¿Por qué?

Me parece mejor el protocolo IMAP ya que conserva el estado del servidor lo cual permite sincronizar los emails en múltiples dispositivos con una misma cuenta.

También es cierto que IMAP usa muchos más recursos del servidor que POP, por ende si la performance es crítica, es mejor usar POP.

### 7. ¿En algún caso es posible enviar más de un correo durante una misma conexión TCP?

Considere:
● Destinatarios múltiples del mismo dominio entre MUA-MSA y entre MTA-MTA
● Destinatarios múltiples de diferentes dominios entre MUA-MSA y entre MTA-MTA

**Primer caso:**

Es posible que el MUA le envie al MSA todos los emails en una misma conexión TCP, ya que SMTP soporta pipelining.
Luego, entre MTA y MTA también se puede, por la misma razón y por ser de mismo dominio, es decir que nuestro MTA se conectará a un solo MTA destino.

**Segundo caso:**
Es posible que el MUA le envie al MSA todos los emails en una misma conexión TCP, por la misma razón.
Luego, entre MTA y MTA no será posible, ya que nuestro MTA deberá establecer una conexión por cada MTA destino distinto (un MTA destino por cada dominio diferente a los que queremos enviar el email).

### 8. Indique sí es posible que el MSA escuche en un puerto TCP diferente a los convencionales y qué implicancias tendría.

Es posible, pero el MSA del otro lado tiene que saberlo, para poder modificar el puerto destino al comunicarse con nosotros.

### 9. Indique sí es posible que el MTA escuche en un puerto TCP diferente a los convencionales y qué implicancias tendría.

Es prácticamente imposible ya que ese MTA debería avisarle a todos los MTA que quieran hablar con él en qué puerto está escuchando, lo cual es muy poco realista ya que hay una infinidad de MTAs en el mundo.

### 10. Ejercicio integrador HTTP, DNS y MAIL

Suponga que registró bajo su propiedad el dominio redes2024.com.ar y dispone de 4 servidores:

● Un servidor DNS instalado configurado como primario de la zona redes2024.com.ar:
(ns1 - IP: 203.0.113.65).

● Un servidor DNS instalado configurado como secundario de la zona redes2024.com.ar:
(ns2 - IP: 203.0.113.66).

● Un servidor de correo electrónico que permitirá a los usuarios envíar y recibir correos a cualquier dominio de Internet.:
(mail - IP: 203.0.113.111).

● Un servidor WEB para el acceso a un webmail que permitirá a los usuarios gestionar vía web sus correos electrónicos a través de la URL https://webmail.redes2024.com.ar:
(correo - IP: 203.0.113.8).

##### a. ¿Qué información debería informar al momento del registro para hacer visible a Internet el dominio registrado?

En el servidor redes2024.com.ar agregaríamos los siguientes registros:

```
redes2024.com.ar  IN  NS  ns1.redes2024.com.ar
ns1.redes2024.com.ar  IN  A  203.0.113.65

redes2024.com.ar  IN  NS  ns2.redes2024.com.ar
ns2.redes2024.com.ar  IN  A  203.0.113.66
```

##### b. ¿Qué registros sería necesario configurar en el servidor de nombres? Indique toda la información necesaria del archivo de zona. Puede utilizar la siguiente tabla de referencia (evalúe la necesidad de usar cada caso los siguientes campos): Nombre del registro, Tipo de registro, Prioridad, TTL, Valor del registro.

En el servidor ns1.redes2024.com.ar agregaríamos los siguientes registros:

```
redes2024.com.ar  IN  MX  5 mail.redes2024.com.ar
mail.redes2024.com.ar  IN  A  203.0.113.111

correo.redes2024.com.ar  IN  A  203.0.113.8
correo.redes2024.com.ar  IN  CNAME  webmail.redes2024.com.ar
```

##### c. ¿Es necesario que el servidor de DNS acepte consultas recursivas? Justifique.

No es necesario, ya que es autoritativo para su dominio y no tiene por qué actuar como resolver de otros dominios, estaríamos desperdiciando recursos.

##### d. ¿Qué servicios/protocolos de capa de aplicación configuraría en cada servidor?

-   En los nameservers configuraría DNS.
-   En el mail server configuraría SMTP y POP3/IMAP.
-   En el webmail configuraría HTTP.

##### e. Para cada servidor, ¿qué puertos considera necesarios dejar abiertos a Internet?. A modo de referencia, para cada puerto indique: servidor, protocolo de transporte y número de puerto.

Considero dejar abiertos a Internet los puertos default de cada protocolo:

-   En los nameservers usaría el puerto 53.
-   En el mail server usaría puerto 25 para SMTP, puerto 110 si es POP, 143 si es IMAP.
-   En el webmail usaría puerto 80.

##### f. ¿Cómo cree que se conectaría el webmail del servidor web con el servidor de correo? ¿Qué protocolos usaría y para qué?

El webmail del servidor web se conectaría con el servidor de correo vía SMTP si se quiere enviar un email, y vía POP/IMAP si se quiere ver el inbox.

##### g. ¿Cómo se podría hacer para que cualquier MTA reconozca como válidos los mails provenientes del dominio redes2024.com.ar solamente a los que llegan de la dirección 203.0.113.111? ¿Afectaría esto a los mails enviados desde el Webmail? Justifique.

???

##### h. ¿Qué característica propia de SMTP, IMAP y POP hace que al adjuntar una imagen o un ejecutable sea necesario aplicar un encoding (ej. base64)?

Los 3 protocolos trabajan únicamente con formato de texto plano (ASCII), no aceptan binario ni imágenes, etc.

##### i. ¿Se podría enviar un mail a un usuario de modo que el receptor vea que el remitente es un usuario distinto? En caso afirmativo, ¿Cómo? ¿Es una indicación de una estafa? Justifique

Si, se puede, cambiando el header MAIL_FROM.

Puede ser indicación de una estafa de tipo phishing.

##### j. ¿Se podría enviar un mail a un usuario de modo que el receptor vea que el destinatario es un usuario distinto? En caso afirmativo, ¿Cómo? ¿Por qué no le llegaría al destinatario que el receptor ve? ¿Es esto una indicación de una estafa?Justifique

???

##### k. ¿Qué protocolo usará nuestro MUA para enviar un correo con remitente redes@info.unlp.edu.ar? ¿Con quién se conectará? ¿Qué información será necesaria y cómo la obtendría?

Para que nuestro MUA pueda enviar un correo con remitente redes@info.unlp.edu.ar deberá primero hallar la IP del mail server (MTA) del dominio info.unlp.edu.ar para lo cual utilizará el protocolo DNS. Luego, nuestro MUA le enviará el email a ese MTA vía SMTP.

##### l. Dado que solo disponemos de un servidor de correo, ¿qué sucederá con los mails que intenten ingresar durante un reinicio del servidor?

Cuando nuestro mailserver esté caído, lo que suceda con los emails que queramos enviar dependerá de la configuración. Puede que recibamos un error, puede que los emails que queremos enviar se almacenen temporalmente en una cola por un determinado tiempo, o puede que se intente volver a enviarlos cada determinado tiempo automáticamente.

##### m. Suponga que contratamos un servidor de correo electrónico en la nube para integrarlo con nuestra arquitectura de servicios. ¿Cómo configuraría el DNS para que ambos servidores de correo se comporten de manera de dar un servicio de correo tolerante a fallos?

Si contratamos un servidor de email en la nube, tendríamos que agregar un nuevo MX en el servidor ns1 y asignarle una prioridad más alta que la de mail.redes2024.com.ar:

```
redes2024.com.ar  IN  MX  5 mail.redes2024.com.ar
mail.redes2024.com.ar  IN  A  203.0.113.111

redes2024.com.ar  IN  MX  10 nube.redes2024.com.ar
nube.redes2024.com.ar  IN  A  ???
```

Esto es tolerante a fallos ya que cuando nuestro mail server principal (mail.redes2024.com.ar) esté caído, se utilizará el de la nube.

### 11. Utilizando la herramienta Swaks envíe un correo electrónico con las siguientes características:

● Dirección destino: Dirección de correo de alumnoimap@redes.unlp.edu.ar
● Dirección origen: redesycomunicaciones@redes.unlp.edu.ar
● Asunto: SMTP-Práctica4
● Archivo adjunto: PDF del enunciado de la práctica
● Cuerpo del mensaje: Esto es una prueba del protocolo SMTP

##### a. Analice tanto la salida del comando swaks como los fuentes del mensaje recibido para responder las siguientes preguntas:

???

###### i. ¿A qué corresponde la información enviada por el servidor destino como respuesta al comando EHLO? Elija dos de las opciones del listado e investigue la funcionalidad de la misma.

???

###### ii. Indicar cuáles cabeceras fueron agregadas por la herramienta swaks.

???

###### iii. ¿Cuál es el message-id del correo enviado? ¿Quién asigna dicho valor?

???

###### iv. ¿Cuál es el software utilizado como servidor de correo electrónico?

???

###### v. Adjunte la salida del comando swaks y los fuentes del correo electrónico.

???

##### b. Descargue de la plataforma la captura de tráfico smtp.pcap y la salida del comando swaks smtp.swaks para responder y justificar los siguientes ejercicios. ¿Por qué el contenido del mail no puede ser leído en la captura de tráfico?

???

##### c. Realice una consulta de DNS por registros TXT al dominio info.unlp.edu.ar y entre dichos registros evalúe la información del registro SPF. ¿Por qué cree que aparecen muchos servidores autorizados?

???

##### d. Realice una consulta de DNS por registros TXT al dominio outlook.com y analice el registro correspondiente a SPF. ¿Cuáles son los bloques de red autorizados para enviar mails?. Investigue para qué se utiliza la directiva "~all"

???

### 12. Observar el gráfico a continuación y teniendo en cuenta lo siguiente, responder:

![Ejercicio 12](https://i.imgur.com/qcXqo5w.png)

● El usuario juan@misitio.com.ar en PC-A desea enviar un mail al usuario alicia@example.com.
● Cada organización tiene sus propios servidores de DNS y Mail.
● El servidor ns1 de misitio.com.ar no tiene la recursión habilitada.

##### a. El servidor de mail, mail1, y de HTTP, www, de example.com tienen la misma IP, ¿es posible esto? Si lo es, ¿cómo lo resolvería?

Esto es posible siempre y cuando utilicen puertos diferentes, lo cual seguramente sea el caso ya que SMTP y HTTP usan puertos distintos por defecto.

##### b. Al enviar el mail, ¿por cuál registro de DNS consultará el MUA?

El MUA consultará por el registro MX de misitio.com.ar que es smtp-5.misitio.com.ar.

##### c. Una vez que el mail fue recibido por el servidor smtp-5, ¿por qué registro de DNS consultará?

Ahora consultará para encontrar el registro MX de example.com, y luego el registro A de ese MX.

##### d. Si en el punto anterior smtp-5 recibiese un listado de nombres de servidores de correo, ¿será necesario realizar una consulta de DNS adicional? Si es afirmativo, ¿por qué tipo de registro y de cuál servidor preguntaría?

Si en el punto anterior smtp-5 recibiese varios MX y no solo uno, buscaría la IP (registro A) del MX con mayor prioridad (menor valor). Solo realizaría una o más consultas DNS adicionales en caso de que el MX de mayor prioridad no se encuentre disponible, en cuyo caso buscaría la IP del MX con segunda mayor prioridad.

##### e. Indicar todo el proceso que deberá realizar el servidor ns1 de misitio.com.ar para obtener los servidores de mail de example.com.

1. Consultar a su servidor resolver local de forma recursiva por los MX de example.com.
2. El resolver local hace una consulta iterativa al root server por el MX de example.com, el cual le retorna el NS de .com.
3. El resolver local hace una consulta iterativa a .com por example.com, el cual le retorna el NS de example.com.
4. El resolver local consulta el MX al servidor NS de example.com y lo obtiene.
5. El resolver local consulta la IP del MX a ese mismo servidor y la obtiene.
6. El resolver local le retorna a ns1.misitio.com.ar la IP de los MX.

##### f. Teniendo en cuenta el proceso de encapsulación/desencapsulación y definición de protocolos, responder V o F y justificar:

###### i. Los datos de la cabecera de SMTP deben ser analizados por el servidor DNS para responder a la consulta de los registros MX

Falso. El servidor DNS no se interesa por los datos de otros protocolos.

###### ii. Al ser recibidos por el servidor smtp-5 los datos agregados por el protocolo SMTP serán analizados por cada una de las capas inferiores

Falso. El modelo en capas funciona de forma opuesta. Las capas inferiores no se preocupan por las capas superiores.

###### iii. Cada protocolo de la capa de aplicación agrega una cabecera con información propia de ese protocolo

Verdadero. Cada capa encapsula y agrega headers propios.

###### iv. Como son todos protocolos de la capa de aplicación, las cabeceras agregadas por el protocolo de DNS puede ser analizadas y comprendidas por el protocolo SMTP o HTTP

Falso. Cada protocolo solo entiende lo propio.

###### v. Para que los clientes en misitio.com.ar puedan acceder el servidor HTTP www.example.com y mostrar correctamente su contenido deben tener el mismo sistema operativo.

Falso. Los protocolos justamente permiten (entre muchas otras cosas) que los dispositivos puedan usar cualquier OS.

##### g. Un cliente web que desea acceder al servidor www.example.com y que no pertenece a ninguno de estos dos dominios puede usar a ns1 de misitio.com.ar como servidor de DNS para resolver la consulta.

Falso. ns1.misitio.com.ar no es autoritativo para example.com y además no es resolver, no realiza consultas recursivas.

##### h. Cuando Alicia quiera ver sus mails desde PC-D, ¿qué registro de DNS deberá consultarse?

Deberán consultarse los registros MX de example.com.

##### i. Indicar todos los protocolos de mail involucrados, puerto y si usan TCP o UDP, en el envío y recepción de dicho mail

1. El MUA consulta el registro MX de su dominio: {DNS, UDP, 53}.
2. El MUA envía el mail a smtp-5 {SMTP, TCP, 25}.
3. smtp-5 consulta la IP del mailserver destino {DNS, UDP, 53}.
4. smtp-5 envía el mail al MTA del mailserver destino {SMTP, TCP, 25}.
5. El MTA recibe el mail {SMTP, TCP, 25}.
6. El MDA obtiene el mail {IMAP, TCP, 143}.
7. PC-D quiere leer el mail, necesita la IP del servidor {DNS, UDP, 53}.
8. El MUA obtiene el mail del MDA {IMAP, TCP, 143}.

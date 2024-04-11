<center>

# General

</center>

-   El proceso es el siguiente:
    -   Capa de Aplicación -> Capa de Transporte -> Capa de Red -> Capa de Enlace -> Capa de Red -> Capa de Transporte -> Capa de Aplicación.
    -   El emisor va encapsulando capa por capa.
    -   El receptor va desencapsulando capa por capa.
-   En el destino se reciben los bits y con esos bits se **re-arma** la trama.

## Notas a agregar en los otros markdowns

-   Detallar todo el proceso de asignación de subredes a varias redes dado un bloque, y luego la asignación de IPs a cada dispositivo. Número de bits, máscaras, redes libres, etc.
-   Agregar más detalle al proceso de las tablas de ruteo.
-   En el comando "ss", cuando tenemos "LISTEN" en la columna State, ese host está escuchando TCP. Cuando tenemos UNCONN, ese host está escuchando UDP.
-   Una PC, por ejemplo 10.10.10.10, puede efectivamente estar comunicandose con 2 o más PCs por un mismo puerto, esto se logra vía multiplexación.
-   Dos PCs en redes distintas pueden tener la misma MAC sin problema, ya que nunca se comunican de forma directa, siempre a través de routers. Lo que si nunca puede pasar es que 2 routers usen la misma MAC.
-   Necesito entender cómo funciona el llenado de la tabla CAM de los switches.
-   En el markdown de Transporte necesito detallar el proceso de fin de una conexión TCP.
-   TCP no usa ICMP.
-   Detallar mejor el control de congestión y de flujo.
-   La ventana de congestión está relacionada a FLUJO.

# General

- 

# Recursos

<center>

# Clase 1

</center>


## Red

- Emisor -> Medio/Dispositivos intermedios -> Receptor
- Sus componentes son:
1. 

## Protocolo

- Serie de normas o reglas que se tienen que respetar y cumplir para que los distintos componentes se comuniquen correctamente.
- Tienen como objetivo comunicar billones y billones de dispositivos distintos entre sí que podrían estar usando sistemas operativos distintos, etc, sin problema.

## Arquitectura en capas

- Una capa ofrece servicios a la capa de arriba y usa servicios de la capa de abajo mediante interfaces.
- El modelo OSI define 7 capas:
    1. Aplicación
    2. Presentación
    3. Sesión
    4. Transporte
    5. Red
    6. Enlace
    7. Física
- El modelo TCP/IP define 4 capas:
    1. Aplicación
    2. Transporte
    3. Red
    4. Enlace

| Comparación| TCP | OSI |
|---| --- |--- |
...

- Cada capa define su Protocol Data unit o PDU, en el caso del modelo TCP/IP las capas son (respectivamente): el Dato, el Segmento, el Paquete, y la Trama. Es decir, el dato se "mete" adentro del segmento, el segmento adentro del paquete, etc, hasta llegar a la última capa donde la Trama se traduce a binario.
- Cada capa se comunica de forma **indirecta** con la misma capa del otro lado de la comunicación (receptor), ya que cada capa del lado opuesto solo "entiende" el contenido de esa misma capa del lado emisor.

## RFC

- Documentación oficial que explica como funcionan los protocolos de TCP/IP.
- Hay muchas categorías de RFC:











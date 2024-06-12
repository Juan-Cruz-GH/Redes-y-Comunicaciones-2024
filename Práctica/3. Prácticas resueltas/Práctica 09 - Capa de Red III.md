<h1 align="center">Práctica 9 - Capa de Red III</h1>

### 1. ¿Qué es IPv6? ¿Por qué es necesaria su implementación?

IPv6 es la versión más nueva del protocolo IP. Provee varias mejoras respecto a la versión anterior (IPv4) como:

1. Direcciones de 128 bits en vez de 32, lo que permite una cantidad enormemente mayor de direcciones IP únicas.
2. Header simplificado.
3. Mejor seguridad debido a IPSec.
4. No requiere NAT.
5. Auto-configuración de direcciones.
6. Mejora el direccionamiento multicast.
7. No utiliza broadcast, lo cual reduce drasticamente la congestión en la red.

Su implementación fue necesaria debido a las limitaciones claras de IPv4, cuyas más graves eran la cantidad muy limitada de direcciones (32 bits es muy poco considerando el crecimiento exponencial de Internet), la poca seguridad y la mala performance.

### 2. ¿Por qué no es necesario el campo “Header Length” en IPv6?

En IPv6 no se requiere el campo Header Length ya que el header de este protocolo tiene un tamaño **fijo** de 40 bytes. En IPv4 se necesitaba ya que el header tenía tamaños varios dependiendo de las opciones.

### 3. ¿En qué se diferencia el checksum de IPv4 e IPv6? Y en cuánto a los campos checksum de TCP y UDP, ¿sufren alguna modificación en cuanto a su obligatoriedad de cálculo?

IPv6 no tiene checksum, simplemente confía en que la capa superior (Transporte) y/o la inferior (Enlace) realicen detección de errores. Es por esto que si se utiliza IPv6 y UDP, UDP debe tener checksum obligatoriamente.

### 4. ¿Qué sucede con el campo “Opciones” en IPv6? ¿Existe, en IPv6, alguna forma de enviar información opcional?

IPv6 no tiene el campo Opciones, pero introduce las extensiones de cabecera que proveen información adicional sobre el paquete en cuestión. Hay varios tipos de extensiones de cabecera, cada uno con sus propiedades.

### 5. Si quisiese que IPv6 soporte una nueva funcionalidad, ¿cómo lo haría?

Si quisiera añadirle una nueva funcionalidad a IPv6, le agregaría una nueva extensión de cabecera personalizada.

### 6. ¿Es necesario el protocolo ICMP en IPv6? ¿Cumple las mismas funciones que en IPv4?

El protocolo ICMP sigue siendo totalmente esencial en IPv6 y cumple las mismas funciones que en IPv4 pero también agrega algunas funcionalidades nuevas como el Neighbour Discovery y la auto-configuración de direcciones.

### 7. ¿Qué funciones cumple el protocolo Neighbour Discovery? ¿Puede funcionar IPv6 sin él?¿Y sin una dirección de tipo link-local?

El protocolo Neighbour Discovery cumple diversas funciones esenciales:

1. Descubrimiento de routers: los hosts descubren a sus routers.
2. Descubrimiento de prefijos: los hosts descubren sus prefijos de red para poder configurar sus propias direcciones IPv6.
3. Auto-configuración: los hosts configuran sus propias IPs automáticamente.
4. Resolución de direcciones: los hosts descubren las MACs de sus vecinos.

IPv6 no puede funcionar sin este protocolo ya que están muy relacionados entre sí.

IPv6 tampoco puede funcionar sin direcciones de tipo link-local porque estas direcciones las usan los hosts y routers para comunicarse en su red local.

### 8. ¿Cuál de las siguientes direcciones IPv6 no son válidas?

| Dirección IPv6                               | Es válida?                                                                                 |
| -------------------------------------------- | ------------------------------------------------------------------------------------------ |
| 2001:0:1019:afde::1                          | ✅ es válida, abrevia 3 grupos que son todos 0.                                            |
| 2001::1871::4                                | ❌ no es válida porque utiliza "::" 2 veces. Solo se puede utilizar una vez por dirección. |
| 3ffg:8712:0:1:0000:aede:aaaa:1211            | ❌ no es válida porque la "g" no es un carácter HEX válido.                                |
| 3::1                                         | ✅ es válida, abrevia 6 grupos que son todos 0.                                            |
| ::                                           | ✅ es válida, representa una dirección con todos sus 8 grupos todos en 0.                  |
| 2001::                                       | ✅ es válida, abrevia 7 grupos que son todos 0.                                            |
| 3ffe:1080:1212:56ed:75da:43ff:fe90:affe      | ✅ es válida, hay 8 grupos y todos poseen caracteres HEX válidos.                          |
| 3ffe:1080:1212:56ed:75da:43ff:fe90:affe:1001 | ❌ no es válida porque hay 9 grupos. Las direcciones IPv6 poseen 8 grupos.                 |

### 9. ¿Cuál sería una abreviatura correcta de 3f80:0000:0000:0a00:0000:0000:0000:0845?

| Dirección IPv6           | Es una abreviatura correcta?                                                   |
| ------------------------ | ------------------------------------------------------------------------------ |
| 3f80::a00::845           | ❌ no es correcta porque usa 2 veces "::".                                     |
| 3f80::a:845              | ❌ no es correcta porque "0a00" no es equivalente a "a".                       |
| 3f80::a00:0:0:0:845:4567 | ❌ no es correcta porque agrega un grupo al final que no estaba.               |
| 3f80:0:0:a00::845        | ✅ es correcta, "0a00" es equivalente a "a00" y "0845" es equivalente a "845". |
| 3f8:0:0:a00::845         | ❌ no es correcta porque "3f8" no es equivalente a "3f80". qu                  |

### 10. Indique si las siguientes direcciones son de link-local, global-address, multicast, etc.

![Tipos de direcciones IPv6](https://cdn.networkacademy.io/sites/default/files/2020-10/ipv6_address_types.png)

| Dirección IPv6             | Tipo?         |
| -------------------------- | ------------- |
| fe80::1/64                 | Link-local    |
| 3ffe:4543:2:100:4398::1/64 | Global        |
| ::                         | Indeterminada |
| ::1                        | Loopback      |
| ff02::2                    | Multicast     |
| 2818:edbc:43e1::8721:122   | Global        |
| ff02::9                    | Multicast     |

### 11. Al autogenerarse una dirección IPv6 sus últimos 64 bits en muchas ocasiones no se deducen de la dirección MAC, se generan de forma random, ¿por qué sucede esto? ¿Qué es lo que se intenta evitar? (Ver direcciones temporarias, RFC 8981)

Originalmente, los últimos 64 bits de las direcciones IPv6 se generaban usando la dirección MAC del dispositivo, lo que permitía rastrear fácilmente a los usuarios ya que la MAC es única. Esto presentó problemas de privacidad y seguridad.

Para mejorar la privacidad, se introdujeron las **direcciones temporales**, las cuales consisten en que los últimos 64 bits de la dirección IPv6 se generan aleatoriamente y además estas direcciones cambian regularmente para evitar el rastreo continuo.

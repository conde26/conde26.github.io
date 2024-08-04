---
title: Comando Nmcli
author: J.Conde
date: 2023-01-08 01:26:00 -03000 
categories: [Linux, Comandos]
tags: [Comandos, guide]
math: true
mermaid: true
---

## Índice
- [Definición](#definición)
- [Ejemplos de uso](#ejemplos-de-uso)

### Definición
Nmcli es la herramienta que permite gestionar el NetworkManager, a traves de cli. El NetworkManager es la gestión de la red a traves de GUI, a diferencia del Networkd, que este es gestionado por los ficheros de configuración de red. Linux, siempre prioriza el NetworkManager. 


### Ejemplos de uso
Antes de ver unos ejemplos de uso, debemos saber que podemos abreviar varios campos, que yo es la forma que uso siempre: 

- g[eneral]
- n[etworking]
- r[adio]
- c[onnection]
- d[evice] 
- a[gent] 
- m[onitor]  

Ver estado de las tarjetas de red o de las conexiones. 
```bash
nmcli dev status
```
```bash
nmcli c
```

Cambiar nombre de la interfaz de red. Ojo esto no cambia el nombre de la tarjeta de red. Este nombre se consigue con el comando anterior. 
```bash
nmcli c modify <nombre-actual> connection.id <nombre-nuevo> connection.interface-name <adaptador>
```

Cambiar IP y mascara de red. 
```bash
nmcli con mod <nombre-interfaz> ipv4.addresses <IP>/<MASK>
```

Cambiar puerta de enlace.
```bash
nmcli con mod <nombre-interfaz> ipv4.gateway <gateway>
```

Cambiar DNS y dominio de buqueda.
```bash
nmcli con mod <nombre-interfaz> ipv4.dns <DNS>
nmcli con mod <nombre-interfaz> ipv4.dns-search <DOMINIO>
```

Poner la tarjeta en modo configuración manual.   
```bash
nmcli con mod <nombre-interfaz> ipv4.method manual
```

Levantar la tarjeta. 
```bash
nmcli con up <nombre-interfaz>
```

Tambien podemos comprobar el estado de nuestra red y esta devuelve varios estados. 

- **none**, El host no está conectado a ninguna red.
- **portal**, El host está detrás de un portal cautivo y no puede acceder a Internet completo
- **limited**, El host está conectado a una red, pero no tiene acceso a Internet.
- **full**, El host está conectado a una red y tiene pleno acceso a Internet.
- **unknown**, No se puede determinar el estado de conectividad.

```bash
nmcli n c check 
```

recordamos que: 
- **n**, networking abreviado 
- **c**, connection abreviado

Podemos conectarnos a una red Wi-Fi de la siguiente forma. 
```bash
nmcli dev wifi connect <SSID> password "PASSWORD"
```

---
title: Comando Grep
author: conde
date: 2022-08-21 11:44:00 -03000 
categories: [Linux, Comandos]
tags: [Comandos, guide]
math: true
mermaid: true
---

## Índice 
- [Definición](#definción)
- [Parámetros](#párametros)
- [Ejemplos de uso](#ejemplos-de-uso)
- [Explicación de expresiones regulares](#explicación-de-expresiones-regulares)

### Definción 
Grep, es un comando que se utiliza para filtrar cadenas de texto en ficheros, dentro del podremos utilizar expresiones regulares y mucho más. 
Este comando es muy potente.

### Párametros
- **-E** ➜ Permite utilizar expresiones regulares extendidas, grep -E, equivale a egrep. 
- **-i** ➜ Case Insensitive (No distingue entre mayúsculas y minúsculas)
- **-n** ➜ Muestra el numero de linea de las coincidencias. 
- **-v** ➜ Elimina del output el patrón indicado 
- **-P** ➜ Expresiones regulares de Perl 
- **-o** ➜ Solo muestra el patrón, no toda la linea. 
- **-A** ➜ Permite mostrar un numero de lineas por debajo del patrón. 
- **-B** ➜ Permite mostrar un numero de lineas por encima del patrón.
- **-C** ➜  Permite mostrar un numero de lineas por encima y debajo del patrón.
- **\--color** ➜ Permite indicar cuando queremos utilizar coloreado para el output (Never, Always y Auto).

### Ejemplos de uso 
Buscar en un fichero la palabra password, sin distinguir entre mayúsculas y minúsculas y mostras la linea donde se encuentra.
```bash 
cat /etc/secret | grep -n -i "password"
```

Mostrar el output siempre en formato colorizado. 
```bash 
cat /etc/secret | grep "root" --color=always
```

Expressión regular con Perl para sacar la mac.
```bash 
macchanger -s eth0 | grep -oP "(\S{2}:){5}\S{2}" --color=always | sort -u
```

Expressión regular con Perl para sacar la ip.
```bash 
ip a | grep -oP "(\d{1,3}\.){3}\d{1,3}" --color=always
```

Expressión regular extendida para sacar la ip.
```bash 
ip a | grep -oE "([[:digit:]]{1,3}\.){3}[[:digit:]]{1,3}" --color=always
```

Filtrar por varios patrones. Esto podemos hacerlo de 3 formas. 
```bash 
cat /etc/passwd | grep "root\|admin"
```
```bash 
cat /etc/passwd  | egrep "root|admin"
```
```bash 
cat /etc/passwd  | grep -E "root|admin"
```

No mostrar el output indicado en el patrón. 
```bash 
cat /etc/passwd  | grep -vE "root|admin"
```

Sacar dos lineas por encima y dos por debajo del output
```bash 
cat /etc/passwd  | grep -i "ROOT" -C 2
```



### Explicación de expresiones regulares
Tenemos varios tipos de datos que podemos utilizar para realizar expresiones regulares. Comenzamos viendo las de perl. 
<div align="center">
<table>
<thead>
  <tr>
    <th>Signo</th>
    <th>Definición</th>
  </tr>
</thead>
<tbody>
  <tr>
    <td>.</td>
    <td>Cualquier caracter salgo el de retorno de carro</td>
  </tr>
  <tr>
    <td>^</td>
    <td>Indica que coincida al principio de la linea</td>
  </tr>
  <tr>
    <td>$</td>
    <td>Indica que coincida al final de la linea</td>
  </tr>
  <tr>
    <td>*</td>
    <td>Aparecera 0 o más veces el caracter que lo procede</td>
  </tr>
  <tr>
    <td>+</td>
    <td>Aparecera 1 o más veces el caracter que lo procede</td>
  </tr>
  <tr>
    <td>?</td>
    <td>Aparecera 0 o 1 veces el caracter que lo procede</td>
  </tr>
  <tr>
    <td>{n}</td>
    <td>Coincide exactamente n veces</td>
  </tr>
  <tr>
    <td>{n,}</td>
    <td>Coincide al menos n veces</td>
  </tr>
  <tr>
    <td>{n,m}</td>
    <td>Coincide al menos n veces y no más de m</td>
  </tr>
  <tr>
    <td>\n</td>
    <td>Salto de linea</td>
  </tr>
  <tr>
    <td>\d</td>
    <td>Un caracter numerico [0-9]</td>
  </tr>
  <tr>
    <td>\t</td>
    <td>Tabulador</td>
  </tr>
  <tr>
    <td>\w</td>
    <td>Caracteres alfanúmericos</td>
  </tr>
  <tr>
    <td>\s</td>
    <td>Un caracter de espciado</td>
  </tr>
  <tr>
    <td>()</td>
    <td>Agrupa una serie de patrones</td>
  </tr>
</tbody>
</table>
</div>


Ahora veremos las que utiliza grep por defecto. 
<div align="center">
<table>
<thead>
  <tr>
    <th>Signo</th>
    <th>Definición</th>
  </tr>
</thead>
<tbody>
  <tr>
    <td>.</td>
    <td>Cualquier caracter salgo el de retorno de carro</td>
  </tr>
  <tr>
    <td>^</td>
    <td>Indica que coincida al principio de la linea</td>
  </tr>
  <tr>
    <td>$</td>
    <td>Indica que coincida al final de la linea</td>
  </tr>
  <tr>
    <td>*</td>
    <td>Aparecera 0 o más veces el caracter que lo procede</td>
  </tr>
  <tr>
    <td>+</td>
    <td>Aparecera 1 o más veces el caracter que lo procede</td>
  </tr>
  <tr>
    <td>?</td>
    <td>Aparecera 0 o 1 veces el caracter que lo procede</td>
  </tr>
  <tr>
    <td>{n}</td>
    <td>Coincide exactamente n veces</td>
  </tr>
  <tr>
    <td>{n,}</td>
    <td>Coincide al menos n veces</td>
  </tr>
  <tr>
    <td>{n,m}</td>
    <td>Coincide al menos n veces y no más de m</td>
  </tr>
  <tr>
    <td>\n</td>
    <td>Salto de linea</td>
  </tr>
  <tr>
    <td>[[:digit:]]</td>
    <td>Un caracter numerico [0-9]</td>
  </tr>
   <tr>
    <td>[[:lower:]]</td>
    <td>Letras en minúscula</td>
  </tr>
   <tr>
    <td>[[:upper:]]</td>
    <td>Letras en mayúscula</td>
  </tr>
  <tr>
    <td>\t</td>
    <td>Tabulador</td>
  </tr>
  <tr>
    <td>\w</td>
    <td>Caracteres alfanúmericos</td>
  </tr>
  <tr>
    <td>\s</td>
    <td>Un caracter de espciado</td>
  </tr>
  <tr>
    <td>()</td>
    <td>Agrupa una serie de patrones</td>
  </tr>
</tbody>
</table>
</div>


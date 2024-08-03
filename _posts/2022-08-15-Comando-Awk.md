---
title: Comando Awk
author: Jose Conde
date: 2022-08-15 23:40:00 -03000 
categories: [Linux, Comandos]
tags: [Comandos, guide]
math: true
mermaid: true
---

![Wallapaper](/assets/img/post/commands/awk.png)

## Índice
- [Definición](#definición)
- [Variables especiales](#variables-especiales)
- [Ejemplos de uso](#ejemplos-de-uso)

### Definición
Awk, es un comando que nos permite filtrar, procesar o analizar los archivos de texto, haciendo por lineas o columnas, este comando es similar a cut, pero mucho más potente. 

### Variables especiales
Dentro del uso de awk, tenemos una variables espciales con las que podemos hacer cosas muy interesantes, que son: 
- **NR** ➜ Mantiene un recuento actual del número de registros de entrada (Lineas).
- **NF** ➜ Número de campos en el registro de entrada actual (Campos).
- **FS** ➜ Indica el deliminador para filtrar.
- **OFS** ➜ Permite indicar el delimitador para la salida del comando.
- **RS** ➜ Indicamos el delimitador y este sera sustituido por un salto de linea (\n). 

### Ejemplos de uso 
Mostrar la primera columna de procesos pero sin la primera linea de titulo. 
```bash
ps | awk 'NR>1{print $1}'
```

Ver los usuarios del sistema en passwd indicando un delimitador. 
```bash
cat /etc/passwd | awk -F ":" '{print $1}'
```

Otra forma de hacer lo anterior, pero usando la variables especial.
```bash
awk '{print $1}' FS=':' /etc/passwd
```

Ver la linea número 10 del fichero indicado. 
```bash
cat /etc/passwd | awk NR==10
```

Uso de awk como grep sin que distinga entre mayúsculas o minúsculas. 
```bash
cat usuarios.txt | awk 'tolower($0) ~ /^conde/'
```

Uso básico como grep, para buscar un patrón. 
```bash
awk '/conde/' usuarios.txt
```

Mostrar el ultimo valor de la linea indicada con el patrón correspondiente y agregando texto. 
```bash
cat /etc/shells | awk -F "/" 'NR>1{print "[!] Shell: "$NF}'
```

Podemos realizar un split de nuestro PATH, de varias formas.  
```bash
echo $PATH | awk '{for (i = 1; i <= NF ;i++) print $i}' FS=':'
```
```bash
echo $PATH | awk '1' RS=':'
```


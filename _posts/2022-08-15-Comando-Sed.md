---
title: Comando Sed
author: conde
date: 2022-08-15 23:56:00 3000 
categories: [Linux, Comandos]
tags: [Comandos, guide]
math: true
mermaid: true
---

## Índice 
- [Definición](#definición)
- [Parámetros](#parámetros)
- [Ejemplos de uso](#ejemplos-de-uso)


### Definición 
Sed, es una herramienta de terminal cuyo uso principal es buscar y reemplazar un texto.

### Parámetros 
- **-i** ➜ Aplica los cambios que le indicamos 
- **-e** ➜ Permite indicar varios scripts de sed 
- **-r** ➜ Expresiones regulares avanzadas

Dentro de sed tenemos otras opciones muy interesantes: 
- s ➜ Reemplaza el patron indicado por otro
- g ➜ Aplica para todas las ocurrencias
- d ➜ Borra las lineas indicadas o el patrón indicado 
- i ➜ Inserta una linea 

### Ejemplos de uso
Eliminar todas las lineas con comentarios. 
```bash
sed "/#.*/d" file.txt
```

Eliminar las lineas en blanco, para compactar todo.
```bash
cat file.txt | sed "/^$/d"
```

Agregar una linea en el numero 11 (La linea debe existir). 
```bash
sed -i '11i\Hello Conde\' file.txt
```

Cambiar un patron por una variable de bash. 
```bash
sed -i "s/usuario/$user/g" file.txt
```

Si queremos que solo cambie la primera ocurrencia, y no todos los *usuario*, indicamos el numero de apareciones. 
```bash
sed -i "s/usuario/$user/1" file.txt
```

Ver las lineas que coindicdan con el patrón que indicamos.
```bash
sed -n "/127.0.0.1/p"  file.txt
```

Eliminar la linea indicada. 
```bash
sed -e '1d' file.txt
```

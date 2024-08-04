---
title: Introducción a NodeJS
author: J.Conde
date: 2023-04-22 22:10:00 -03000 
categories: [Programacion, NodeJS]
tags: [Web]
math: true
mermaid: true
---


### ¿Qué es NodeJS?

**NodeJS**, es un entorno que trabaja en tiempo de ejecución, de código abierto, multi-plataforma, que permite a los desarrolladores crear toda clase de herramientas de lado servidor y aplicaciones en JavaScript.

### Comprobar versiones 

Versión de Node. 

```node 
node --version 
```

Versión de Npm. 

```node
npm --version
```

### ¿Qué es Npm?

**Npm** (Node Package Manager), es el gestor de paquetes de NodeJS. Esto nos permite instalar librerías, y gestionar  los paquetes de NodeJS. 

**Npm**, funciona de una forma similar a Git, por lo que cuando creamos una carpeta, tendremos que iniciarlizala, para indicarle que es un paquete de npm. 

```node 
npm init
```

El comando anterior, creara un fichero llamado **package.json**. Este archivo indica a npm que el directorio en el que se encuentra es en efecto un proyecto o paquete npm. 

### Package.json 

Dentro de este fichero, podemos definir varias parametros en json. Dentro de la pestaña scripts, podemos agregar una nueva linea, para iniciarlizar nuestro proyecto de otra forma, por ejemplo: 

Tenemos el siguiente fichero: 

```json 
{
  "name": "programing",
  "version": "1.0.0",
  "description": "Este es mi primer test",
  "main": "apis.js",
  "scripts": {
    "start": "node --watch apis.js"
  },
  "author": "",
  "license": "ISC"
}
```

Ahora si queremos lanzar nuestra aplicación en Expresss, podemos hacerla de varias formas: 

```node
node --watch apis.js
```

```node
npm start 
```

* En este caso start, ya es un parametro definido en npm, por lo que si le indicamos el comando a ejecutar, hará esa acción, por lo cual, sería lo mismo que es primer comando. 

Ahora que ya sabemos que es Npm, podemos hacer varias acciones con el.

### Instalación de paquetes 

```node 
npm install <modulo> 
npm i <modulo>
```

### Inicializar un proyecto 

```node 
npm init
```

### Conocer versión 

```node 
npm -v 
npm --version 
```

### Parametros especiales

Si tenemos en el package.json, que se llama test, para ejecutarlo haríamos: 

```node 
npm run test 
```

### Paquetes importantes 

```node 
npm install dotenv
npm install express
npm install mysql
```


### Referencias 

* Página oficial de paquetes NPM: [npmjs](https://npmjs.com/package)
* https://www.freecodecamp.org/espanol/news/que-es-npm/
* https://developer.mozilla.org/es/docs/Learn/Server-side/Express_Nodejs/Introduction 
* https://expressjs.com/es/guide/database-integration.html#mysql
* https://www.hostinger.es/tutoriales/que-es-npm
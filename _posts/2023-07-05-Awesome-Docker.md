---
title: Instalación y configuración de Docker
author: conde
date: 2023-06-13 12:58:00 -03000 
categories: [DevOps, Docker]
tags: [DevOps]
math: true
mermaid: true
image: 
   path: /assets/img/post/docker/docker_logo.jpg
   alt: Docker logo
---

## Índice
- [Introducción](#introducción)
- [Instalación](#instalación)
- [Configuración inicial](#configuración-inicial)

## Introducción 

### ¿Que es Docker?

Docker, es una herramienta que permite desplegar aplicaciones en contenedores, de forma
rápida portable. Esto significa que Docker puede generar “aplicaciones de bolsillo” debido a que
usa imágenes y contenedores.

### Arquitectura de Docker

Para entender la arquitectura vemos una imagen con la que podemos hacernos una idea al
completo de como funciona Docker.



![Docker](/assets/img/post/docker/docker.png)



Visto la imagen, tenemos:

* **Docker Host** → Un Docker Host es donde instalamos nuestro Docker (Localhost, AWS, etc.). Dentro del Docker Host, tenemos:
	* **Docker CLI – Cliente** → Linea de comandos con la que nos comunicamos al Server y podemos administrar:
		* Contenedores, Imágenes, Volúmenes y Redes.
	* **API** → Canal de comunicación entre Docker client y Docker server, ambos se almacenan en Docker host.
	* **Docker Daemon – Server** → Servicio de Docker, es el que presta el servicio, y se comunica con el Docker CLI.

### ¿Qué es una imagen?

Una imagen, es un paquete que contiene toda la configuración necesaria para que funcione el
servicio (paquetes, configuraciones, etc.). Una imagen se compone por capas, la cantidad de
capas que nosotros deseemos, en las que por ejemplo, podemos definir una capa “from” para el
sistema operativo vamos a usar, otra capa “run” con la que instalamos paquetes y otra capa
“cmd” que es la linea donde indicamos el comando para que inicie el servicio de la capa dos.
Estas capas son de solo lectura por la que no podemos modificarlas. Esta capas se crean en un
archivo llamado dockerfile. En importante indicar en la capa cmd, que nuestro proceso sea de
primer plano, ya que si muere la capa, también muere el proceso o aplicación.

### ¿Qué es un contenedor?

Un contenedor, es una "capa adicional" que va a ejecutar todas las capas anteriores que
definimos en la imagen. Esta capa si es de lectura y escritura, estos cambios que hagamos en
esta capa serán temporales, para ver un ejemplo de contenedor:

### Contenedores VS Máquinas virtuales

Las ventajas que tendremos son almacenamiento, ram y cpu. Las principales características, es
si necesitamos instalar apache por ejemplo, no es necesario instalar todos los otros programas
que vienen por defecto por lo que podremos tener un contenedor con solo el servicio de apache,
que consumirá muy poco de Ram y CPU a diferencia de las máquinas virtual que pesan GB.



|       Contenedor      	|          VMs          	|
|:---------------------:	|:---------------------:	|
| • HDD → Tamaño menor  	| • HDD → Tamaño mayor  	|
| • RAM → Menor consumo 	| • RAM → Mayor consumo 	|
| • CPU → Menor consumo 	| • CPU → Mayor consumo 	|



## Instalación 

Tenemos varias formas de instalarlo, pero nosotros usaremos los repositorios. 

* Comenzamos actualizando e instalando los paquetes necesarios. 

```bash
apt-get update && apt-get install ca-certificates curl gnupg -y 
```

* Agregamos la clave GPG oficial de Docker.

```bash
sudo install -m 0755 -d /etc/apt/keyrings
curl -fsSL https://download.docker.com/linux/debian/gpg | gpg --dearmor -o /etc/apt/keyrings/docker.gpg
chmod a+r /etc/apt/keyrings/docker.gpg
```

* Agregamos los repositorios. 

```bash
echo \
  "deb [arch="$(dpkg --print-architecture)" signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu \
  "$(. /etc/os-release && echo "$VERSION_CODENAME")" stable" | \
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
```

* Actualizamos los repositorios de nuevos. 

```bash
apt-get update
```

* Por último instalamos docker.

```bash
apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin -y
```

* Podemos verificar que docker funciona correctamente lanzando una imagen. 

```bash
docker run hello-world
```

## Configuración inicial

Si queremos que nuestro usuario actual pueda utilizar docker, haremos la siguiente configuración. 

* Creamos un grupo para docker 

```bash
sudo groupadd docker
```

* Agregamos el usuario actual al grupo 

```bash 
sudo usermod -aG docker $USER
```

* Activar cambios en el grupo

```bash
newgrp docker
```

* Habilitamos los servicios para que inicien en el arranque

```bash
sudo systemctl enable docker.service
sudo systemctl enable containerd.service
```

* Cerramos sesión o reiniciamos para aplicar cambios

```bash
reboot
```

* Testeamos que funciona correctamente. 

```bash 
docker run hello-world
```


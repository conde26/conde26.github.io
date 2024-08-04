---
title: Instalación y configuración de Portainer
author: conde
date: 2023-08-23 19:30:00 -03000 
categories: [DevOps, Portainer]
tags: [DevOps]
math: true
mermaid: true
image: 
   path: /assets/img/post/portainer/portainer_logo.png
   alt: Portainer logo
---

## Índice
- [Definición](#definición)
- [Instalación](#instalación)
- [Actualización](#actualizar-portainer)


### Definición

**Portainer**, es una herramienta web open-source que permite gestionar contenedores Docker.​ Permite administrar contenedores de forma remota o local, la infraestructura de soporte y todos los aspectos de las implementaciones de Kubernetes


### Instalación

Para instalar Portainer, una forma muy comoda y rápida, es hacerlo con docker, para ello comenzamos creando un volumen. 

```bash
docker volume create portainer_data
```

Seguido lo instalamos con la imagen oficial. 

```bash
docker run -d -p 8000:8000 -p 9443:9443 --name portainer --restart=always -v /var/run/docker.sock:/var/run/docker.sock -v portainer_data:/data portainer/portainer-ce:latest
```

Verificamos que nuestro contenedor esta corriendo. 

```bash
docker ps -f "name=portainer"
```

Una vez que verificamos que el contenedor esta corriendo correctamente, vamos a acceder a su panmel web. 

* **https://localhost:9443**

Una vez accedemos, nos pedirá por primera vez, un usuario y una contraseña.  

![IntialConfig](/assets/img/post/portainer/portainer.png)

Listo, ya tenemos acceso a nuestro panel, donde podemos gestionar nuestros **enviroments**, 

![IntialConfig2](/assets/img/post/portainer/Portainer2.png)


### Actualizar Portainer 

Si queremos actualizar nuestro portainer a la ultima version, debemos tener una copia de seguridad, o un volumen como el que creamos al principio de la instalación, así si borramos el contenedor, no habrá ningún problema. 
* Comenzamos buscando el contenedor. 

```bash
docker ps -f "name=portainer" -q
```

* Eliminamos el contenedor actual. 

```bash
docker rm -f $(docker ps -f "name=portainer" -q)
```

* Comprobamos que tenemos la imagen de portainer.

```bash
docker images portainer/portainer-ce
```
* Hacemos un pull, para bajar la nueva versión de la imagen. 

```bash
docker pull portainer/portainer-ce
```

* Volvemos a lanzar el comando para construir el contenedor.  

```bash
docker run -d -p 8000:8000 -p 9443:9443 --name portainer --restart=always -v /var/run/docker.sock:/var/run/docker.sock -v portainer_data:/data portainer/portainer-ce:latest
```

* Comprobamos que esta corriendo. 

```bash
docker ps -f "name=portainer"
```

![Update](/assets/img/post/portainer/portainer3.png)


Si ahora accedemos, podemos ver que funciona correctamente, y en la última versión. 


![Update2](/assets/img/post/portainer/portainer4.png)


### Referencias 

* [Instalación y Actualización portainer](https://docs.portainer.io/start/install-ce?hsCtaTracking=a66b69bb-4970-46b7-bc31-cfc8022c7eb2%7C0d5be9a2-9dac-4ab1-9498-4b07566effd3)
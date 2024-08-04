---
title: Imágenes en Docker
author: conde
date: 2023-08-23 20:05:00 -03000 
categories: [DevOps, Docker]
tags: [DevOps]
math: true
mermaid: true
image: 
   path: /assets/img/post/docker/docker_logo.png
   alt: Docker logo
---

## Índice
- [Repaso de imagenes](#imagen)
- [Gestión de imagenes](#gestión-de-imagenes)


## Imagen

Anteriormente, vimos una introducción teorica a Docker, como se instalaba, y como se hacía una pequeña configuración. Que se encuentra en el siguiente post [Introducción a Docker](https://josecg.com/posts/Awesome-Docker/)

## Gestión de imagenes 

* Listar imagenes de nuestro equipo. 


```bash
docker images
```


* Listar todas las imagenes de nuestro equipo. 


```bash
docker images -a 
```


* Descargar una nueva imagen.


```bash
docker pull "<nombre-imagen>"
```


* Elminar una imagen. 


```bash
docker rmi "<image-ui>" -f 
```


* Crear una imagen a partir de nuestro dockerfile

	* **-t**: Tag/Etiqueta para la imagen.
	* **-f**: Especificamos un nombre diferente al fichero dockerfile (En este caso apache2 = dockerfile).
	* **.** : Se indica que se inizializa en este directorio 


```bash
docker build -t "<DisplayName>" -f "<DockerFileName>" .
```


* Listar imagenes untagged. 


```bash
docker images --filter "dangling=true"
```


* Eliminar todas las imagenes del equipo. 


```bash
docker images -q | xargs docker rmi -f 
```

* Formatear la salida del comando inicial. 


|  Placeholder  |                Description               |   |   |   |
|:-------------:|:----------------------------------------:|---|---|---|
| .ID           | Image ID                                 |   |   |   |
| .Repository   | Image repository                         |   |   |   |
| .Tag          | Image tag                                |   |   |   |
| .Digest       | Image digest                             |   |   |   |
| .CreatedSince | Elapsed time since the image was created |   |   |   |
| .CreatedAt    | Time when the image was created          |   |   |   |
| .Size         | Image disk size                          |   |   |   |


```bash
 docker images --format "\{\{.ID\}\}: \{\{.Repository\}\}"
```




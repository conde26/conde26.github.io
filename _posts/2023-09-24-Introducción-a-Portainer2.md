---
title: Agregar nodos a Portainer
author: conde
date: 2023-08-24 10:45:00 -03000 
categories: [DevOps, Portainer]
tags: [DevOps]
math: true
mermaid: true
image: 
   path: /assets/img/post/portainer/portainer_logo.png
   alt: Portainer logo
---

## Índice

- [Introducción](#introducción)
- [Configuración](#configuración)


### Introducción 

Para este ejemplo, tendremos dos máquinas: 

* **1º Máquina** (Aquí se encuentra el portainer)
* **2º Máquina** (Aquí se encuentra el agente para conectar al portainer)

Es importante, que estas máquinas se vean entre sí. 

### Configuración

Nos dirigimos a nuestro portainer, en el menú de la izquierda vamos a **"Environments"**. Podemos ver todos los que tenemos, le damos a **"+ Add Environment".**


![IntialConfig](/assets/img/post/portainer/portainer5.png)


Una vez le damos, seleccionamos **"Docker Standalone"**. 

![IntialConfig2](/assets/img/post/portainer/portainer6.png)

Lo vamos a conectar desde el agente, es una forma muy cómoda, para ello, si hacemos clic en **"Agent",** podemos ver que ya nos devuelve el comando para lanzar en el otro equipo. 

```bash
# Comando para instalar el agente en el host.
docker run -d \
  -p 9001:9001 \
  --name portainer_agent \
  --restart=always \
  -v /var/run/docker.sock:/var/run/docker.sock \
  -v /var/lib/docker/volumes:/var/lib/docker/volumes \
  portainer/agent:2.19.1
```

* **Name**: "DisplayName para el Nodo"
* **Environment address**: "Ip de la máquina donde instalas el agente." 

![IntialConfig3](/assets/img/post/portainer/portainer7.1.png)


Listo, si ahora vamos al home, podemos ver que ya aparecen varios entornos. 

![IntialConfig4](/assets/img/post/portainer/portainer8.1.png)

Ahora ya podemos gestionar los contenedores locales y remotos desde nuestro portainer. 

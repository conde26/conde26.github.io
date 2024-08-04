---
title: Introducción a Jenkins
author: conde
date: 2023-06-13 12:58:00 -03000 
categories: [DevOps, Jenkins]
tags: [DevOps]
math: true
mermaid: true
---

## Índice
- [Definición](#definición)
- [Conceptos](#conceptos)
- [Instalación](#instalación)
- [Configuración inicial](#configuración-inicial)
- [Referencias](#referencias)

## Definición 

**Jenkins**, es un software utilizado para CI/CD (Integración y despliegue continuo). Escrito en Java, es multiplataforma y accesible mediante interfaz web. Es el software más utilizado en la actualidad para este propósito.

## Conceptos


Algunos conceptos que debemos tener claros son:

- **Maestro**: Servidor principal o nodo central en una arquitectura de CI/CD.
- **Esclavo o Agente**: Es un nodo secundario o máquina adicional que realiza tareas específicas asignadas por el "maestro" de Jenkins.
- **Nodo**: Se refiere a cualquier máquina o dispositivo que se puede agregar al entorno de Jenkins como un agente.

## Instalación 

Podemos instalar jenkins, de varias formas, sobre la propia máquina o en un contendor de docker. 

### Sobre S.O

```bash
sudo apt update
sudo apt install openjdk-11-jre
```

Ejecutamos la siguiente cadena de comandos para instalar jenkins.


```bash
curl -fsSL https://pkg.jenkins.io/debian/jenkins.io-2023.key | sudo tee \
  /usr/share/keyrings/jenkins-keyring.asc > /dev/null
echo deb [signed-by=/usr/share/keyrings/jenkins-keyring.asc] \
  https://pkg.jenkins.io/debian binary/ | sudo tee \
  /etc/apt/sources.list.d/jenkins.list > /dev/null
sudo apt-get update
sudo apt-get install jenkins
```

### Sobre Docker

Podemos hacer una instalación normal: 
```bash
docker run --name "jenkins-server" -h "Master" -dit -p 8080:8080 jenkins/jenkins
```

Tambien con Docker Compose. 

```yml 
version: '3'
services: 
  jenkins:
    container_name: jenkins-server
    image: jenkins:latest
    ports: 
      - "8080:8080"
```

## Configuración inicial

Una vez lo instalamos, accedemos vía web, por defecto usa el puerto 8080. 

* http://localhost:8080 

Nos pedirán la contraseña inical que viene por defecto, y esta almacenada en nuestra máquina o contenedor.

```bash
cat /var/lib/jenkins/secrets/initialAdminPassword
```

![Password](/assets/img/post/jenkins/InitialPassword.png)

Una vez la introducimos ya podemos crear nuestro usuario administrador. 

- **Usuario (Sin puntos)**
- **Password**
- **Nombre completo**
- **Email**

![AdminUser](/assets/img/post/jenkins/AdminUser.png)

Por último ya tenemos acceso a nuestro panel web. 

![DashBoard](/assets/img/post/jenkins/DashBoard.png)



## Referencias 
* Instalación: https://www.jenkins.io/doc/book/installing/linux/
* Configuración: https://www.jenkins.io/doc/book/security/controller-isolation/
* API: https://www.jenkins.io/doc/book/using/remote-access-api/
* LDAP: https://plugins.jenkins.io/ldap/
* Integrar con Bitbucket: https://ricardodev02.medium.com/how-to-connect-jenkins-for-scm-with-bitbucket-github-gitlab-azure-repos-e115f1ca897f
* SSH Know_hosts: https://stackoverflow.com/questions/34906302/add-public-key-to-known-hosts-file
* Instalar openJDK en Windows: https://ed.team/blog/instalar-openjdk-en-windows
* OpenJDK: https://jdk.java.net/archive/
* SSO SAML: https://plugins.jenkins.io/miniorange-saml-sp/
* SSO SAML AZure AD: https://miniorange.com/atlassian/saml-single-sign-on-sso-into-jenkins-using-azure-ad-as-idp/
* SSL Autosigned: https://infotechys.com/install-ssl-certificates-on-jenkins/

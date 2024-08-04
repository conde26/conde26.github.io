---
title: Habilitar 2FA para SSH en Linux
author: conde
date: 2024-08-04 18:15:00 -03000 
categories: [Linux, Security]
tags: [guide]
math: true
mermaid: true
image: 
   path: /assets/img/post/security/2FA.png
   alt: 2FA for SSH 
---

## Índice
- [Definición](#definición)
- [Instalación](#instalación)
- [Configuración](#configuración)
    - [Google Authenticator](#configuración-de-google-authenticator)
    - [Autenticación](#configuración-de-autenticación)
- [Comprobación](#comprobación)
- [Referencias](#referencias)

### Definición

La Autenticación en **Dos Factores (2FA, por sus siglas en inglés)** es un método de seguridad que requiere dos formas distintas de verificación para acceder a una cuenta o sistema. Estos factores suelen incluir algo que el usuario sabe (como una contraseña) y algo que el usuario tiene (como un código enviado a un dispositivo móvil). Esta combinación reduce significativamente el riesgo de acceso no autorizado, ya que incluso si la contraseña es comprometida, el acceso no se otorgará sin el segundo factor.

### Instalación

- Google Authenticator (Paquete en linux)
- Authenticator (App en movil)

Comenzamos **instalando el paquete de google authenticator** en nuestro servidor. 

```bash
sudo apt install libpam-google-authenticator -y
```

![PackageInstall](/assets/img/post/security/2FA-Install.png)

Seguido **instalamos la app en nuestra Android o IOS**, como cualquier aplicación normal. 

* [Google Authenticator en Android](https://play.google.com/store/apps/details?id=com.google.android.apps.authenticator2&hl=es&pli=1)
* [Google Authenticator en IOS](https://apps.apple.com/es/app/google-authenticator/id388497605)

### Configuración 

#### Configuración de Google Authenticator

Una vez que ya tenemos instalado todo lo necesario, vamos a generar el QR para agregarlo a nuesta aplicación. 

```bash
google-authenticator -s ~/.ssh/google_authenticator
```

Escaneamos el siguiente QR dentro de la APP. 

![PackageConfig](/assets/img/post/security/2FA-Config_1.png)

Indicamos el código que nos devuelve la APP en el movil cuando lo escaneamos. Y seguimos configurando de la siguiente forma.

![PackageConfig2](/assets/img/post/security/2FA-Config_2.png)


#### Configuración de autenticación
Ejecutamos el siguiente comando para agregar la linea necesaria al fichero PAM de ssh. 

```bash
echo 'auth required pam_google_authenticator.so secret=/home/${USER}/.ssh/google_authenticator nullok' >> /etc/pam.d/sshd
```

* Tener en cuenta con que usuario lo ejecutamos!! Si es root la ruta no es esa es: 
    * /root/.ssh/google_authenticator 

De forma que quede así: 

![PackageConfig3](/assets/img/post/security/2FA-Config_3.png)

Agregamos las siguiente keys al fichero de ssh. 

* Si es un **ubuntu 22.04 o superior**, ejecutamos la siguiente linea: 

```bash
echo 'KbdInteractiveAuthentication yes' >> /etc/ssh/sshd_config 
```

* Si es **ubuntu 21.10 o menor**, ejecutamos la siguiente: 

```bash
echo 'ChallengeResponseAuthentication yes' >> /etc/ssh/sshd_config 
```

![PackageConfig4](/assets/img/post/security/2FA-Config_4.png)

Reiniciamos el servicio de ssh y listo. 

```bash
service sshd restart
```

### Comprobación 
Intentamos acceder por SSH a nuestro servidor con Termius, ya que aquí se ve claro como lo pide. 

![PackageConfig5](/assets/img/post/security/2FA-Config_5.png)

Introduccimos el código, y listo, ya estamos dentro. 

![PackageConfig6](/assets/img/post/security/2FA-Config_6.png)

### Referencias
* https://4sysops.com/archives/enable-two-factor-authentication-for-ssh-in-linux/ 
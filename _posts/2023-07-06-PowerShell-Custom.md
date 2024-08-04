---
title: Personalización de terminal de PowerShell 
author: J.Conde
date: 2023-06-15 12:58:00 -03000 
categories: [Windows, PowerShell]
tags: [Windows, PowerShell]
math: true
mermaid: true
---

## Índice
- [Introducción](#introducción)
- [Instalación de oh my posh](#instalación-de-oh-my-posh)
- [Instalación de módulos](#instalación-de-módulos)
- [Instalación de fuentes](#instalación-de-fuentes)
- [Configuración](#configuración)
- [Referencias](#referencias)



## Introducción 

Para personalizar nuestra terminal de PowerShell, lo haremos sobre Windows Terminal.

* [Windows Terminal](https://learn.microsoft.com/es-es/windows/terminal/install)

## Instalación de oh my posh

Abrimos nuestra terminal de PowerShell como **Administrador**, y ejecutamos el siguiente comando: 

```powershell
Set-ExecutionPolicy Bypass -Scope Process -Force; Invoke-Expression ((New-Object System.Net.WebClient).DownloadString('https://ohmyposh.dev/install.ps1'))
```

## Instalación de módulos

Instalamos los siguiente módulos, ya que los usaremos tambien. 

```powershell
Install-Module "Terminal-Icons" -Scope CurrentUser 
Install-Module "PSReadLine" -Scope CurrentUser
```

## Instalación de fuentes 

Una vez instalado esos módulos, necesitaremos instalar las fuentes para que todos los caracteres e iconos se vean correctamente. 

```powershell
oh-my-posh font install meslo
```

Hecho eso ya tenemos todo listo, para comenzar la personalización. 

## Configuración

Una vez que ya tenemos todos los paquetes necesarios instalados. Vamos a agregar todo a nuestra terminal, para ello en nuestro consola de powershell, escribimos el siguiente comando: 


```powershell
notepad $PROFILE
```

Se nos abrirá una fichero, sino existe lo creamos, y agregamos la siguiente configuración. 

```powershell
#Tema para la terminal 
$env:Path += ";C:\Users\user\AppData\Local\Programs\oh-my-posh\bin"
oh-my-posh init pwsh --config "$env:POSH_THEMES_PATH\jandedobbeleer.omp.json" | Invoke-Expression

# Modulos 
Import-Module Terminal-Icons
Import-Module PSReadLine

# Funciones para un historial comodo. 
Set-PSReadLineOption -PredictionSource HistoryAndPlugin
Set-PSReadLineOption -PredictionViewStyle ListView
Set-PsReadLineKeyHandler -key tab -Function Complete # Solo disponible en Pwsh 7.
```

Guardamos, reinciamos y listo, ya tenemos nuestro terminal personalizada. Nos falta selecionar la fuente que vamos a utilizar, para ello: 

![fuentes](/assets/img/post/powershell/fuentes.png)

Una vez selecionadas, cerramos y abrimos nuestra terminal y listo, nuestra terminal se debería ver tal que así (Dependiendo el tema selecionado). 

* [Temas disponibles](https://ohmyposh.dev/docs/themes)

![terminal](/assets/img/post/powershell/final.png)


## Referencias 

* [Instalación](https://ohmyposh.dev/docs/installation/windows)
* [Temas](https://ohmyposh.dev/docs/themes)
* [Fuentes](https://www.nerdfonts.com/)
* [Windows Terminal](https://learn.microsoft.com/es-es/windows/terminal/install)
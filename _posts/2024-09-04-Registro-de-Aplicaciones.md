---
title: Registro de aplicaciones en Entra ID
author: conde
date: 2024-09-04 11:06:00 -03000 
categories: [Microsoft, EntraID]
tags: [Microsoft, EntraID]
math: true
mermaid: true
image: 
   path: /assets/img/post/entra-id/Microsoft_Entra_ID.png
   alt: Entra ID
---

## Índice
- [Introducción](#introducción)
- [Requisitos](#requisitos)
- [Operativa](#operativa)
    - [Overview](#overview)
    - [Autenticación](#autenticacíon)
    - [Certificados y secretos](#certificados-y-secretos)
    - [Permisos](#permisos)
- [Conexión con PowerShell](#conexión-con-powershell)

### Introducción

La Autenticación en **Dos Factores (2FA, por sus siglas en inglés)** es un método de seguridad que requiere dos formas distintas de verificación para acceder a una cuenta o sistema. Estos factores suelen incluir algo que el usuario sabe (como una contraseña) y algo que el usuario tiene (como un código enviado a un dispositivo móvil). Esta combinación reduce significativamente el riesgo de acceso no autorizado, ya que incluso si la contraseña es comprometida, el acceso no se otorgará sin el segundo factor.

### Requisitos

Requisitos previos
* Una cuenta de Azure que tenga una suscripción activa. 
* La cuenta de Azure debe ser al menos de un administrador de aplicaciones en la nube.
* Finalización del inicio rápido configuración de un inquilino.

### Operativa

Comenzamos accediendo al panel de administración de Entra ID, anteriormente llamado Azure AD. 

* [Panel de administración de Entra ID](https://entra.microsoft.com/)

Nos dirigimos a **Identity | App Registrations**

#### Overview 

![Step1](/assets/img/post/entra-id/steps_entra_1.png)

Le damos a **New registration**. 

* Indicamos un nombre para la aplicación (Campo obligatorio).
* Tipo de cuentas compatibles (Lo dejamos por defecto).
* Uri de respuesta (Lo dejamos por defecto).

En tipo de cuentas compatibles, tenemos 4 tipos, que son los siguientes: 

|                          Tipos de cuenta admitidos                          |                                                                                                                                                                                           Descripción                                                                                                                                                                                           |   |   |   |
|:---------------------------------------------------------------------------:|:-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------:|---|---|---|
| Solo las cuentas de este directorio organizativo                            | Seleccione esta opción si va a desarrollar una aplicación para que la utilicen usuarios (o invitados) de su inquilino. A menudo denominada aplicación de línea de negocio, se trata de una aplicación de un solo inquilino en la plataforma de identidad de Microsoft.                                                                                                                          |   |   |   |
| Cuentas en cualquier directorio organizativo                                | Seleccione esta opción si desea que los usuarios de cualquier inquilino de Azure Active Directory (Azure AD) pueda usar la aplicación. Esta opción es adecuada si, por ejemplo, va a desarrollar una aplicación de software como servicio (SaaS) que desea proporcionar a varias organizaciones. Este tipo de aplicación se denomina multiinquilino en la plataforma de identidad de Microsoft. |   |   |   |
| Cuentas en cualquier directorio organizativo y cuentas Microsoft personales | Seleccione esta opción para establecer como destino el mayor conjunto posible de clientes. Al seleccionar esta opción, estará registrando una aplicación multiinquilino que también admite usuarios con cuentas personales de Microsoft. Las cuentas personales de Microsoft abarcan las cuentas de Skype, Xbox, Live y Hotmail.                                                                |   |   |   |
| Cuentas personales de Microsoft                                             | Seleccione esta opción si va a crear una aplicación solo para usuarios con cuentas personales de Microsoft. Las cuentas personales de Microsoft abarcan las cuentas de Skype, Xbox, Live y Hotmail.                                                                                                                                                                                             |   |   |   |
Una vez le damos a crear ya tenemos los siguientes datos. 

* **ClientID**, es el ID de aplicación el AppID.
* **ObjectID**, es el ID de objeto dentro de Azure.
* **TenantID**: es el ID de nuestro directorio

![Step2](/assets/img/post/entra-id/steps_entra_2.png)

#### Autenticacíon

Dentro de la pestaña **authentication**, podemos agregar las URL, normalmente son de tipo web/MPA

* **SPA**, son arquitecturas en el que el servidor únicamente dispone de una página y carga todos los estilos y todos los formatos en el cliente y este a partir de ese momento únicamente pide datos al servidor y va mostrando diferentes componentes al usuario que existen en la mega página. Pero nunca navega entre páginas.
* **MPA**, Arquitecturas Web Clásicas en donde uno dispone de múltiples páginas HTML y cada una carga diferentes contenidos apoyándose en la navegación contra el servidor

La URL de obligatorio normalmente cuando hacemos integraciones con aplicaciones SaaS, para integrar en Login con microsoft. 

![Step3](/assets/img/post/entra-id/steps_entra_3.png)

#### Certificados y secretos

Para conectarnos a la aplicación podemos hacerlo utilizando credenciales propias de la aplicación. 

* **Certificados**, son creados en nuestro equipo, y se cargan aquí, solo se puede conectar desde el host, que tiene el certificado instalado. 
* **Secreto**, en una key que se genera desde el panel, que permite conectarse desde cualquier estación.

Vamos a crear un secreto para luego podernos conectar. 

![Step4](/assets/img/post/entra-id/steps_entra_4.png)

Una vez creado nos devuelve el valor, este se debe anotar de inmediato, una vez cambiamos de pestaña, ya no lo podemos visualizar. 

![Step5](/assets/img/post/entra-id/steps_entra_5.png)

#### Permisos

Vamos a agregar 3 permisos básicos, para la prueba. 

* **openid**
* **email** 
* **profile**

Esos 3 permisos son equivalentes al permisos **User.Read**

Debemos tener en cuenta dos terminos al agregar permisos: 

* **Delegated Permissions**, estos permisos son los que se asignan cuando autenticamos como usuario, por ejemplo el login al panel de sharepoint. 
* **Applications Permissions**, estos permisos se cogen cuando autenticas como aplicación desde consola, por ejemplo para hacer consultas con powershell. 

En nuestro caso, los agregaremos delegados, ya que nos conectaremos con powershell. 

![Step6](/assets/img/post/entra-id/steps_entra_6.png)

Una vez se agregan se verá de la siguiente forma. 

![Step7](/assets/img/post/entra-id/steps_entra_7.png)

Listo, ya tenemos una aplicación básica creada, con la que podríamos autenticar usando linea de comandos, o integrandola con un aplicativo, como por ejemplo, jenkins, portainer, etc.

### Conexión con PowerShell

Vamos a conectarnos con powershell, para comprobar esto. Necesitamos instalar el plugin de autenticación. 

```powershell
Install-Module Microsoft.Graph.Authentication -Scope CurrentUser
```

Una vez instalado, vamos a generar el token para conectarnos como aplicación. Para ello creamos la siguiente función. 

```powershell
function Get-MgTokenApp {
  
    <#
        .SYNOPSIS
        Get Graph ApiToken token to connect with Graph 

        .DESCRIPTION
        Get custom Graph Token from API, like aplication. 

        .EXAMPLE
        Get-MgTokenApp

        Generate token with variables from PSData. 

        .EXAMPLE
        Get-MgTokenApp -TenantID $TenantID -ClientID $ClientID -ClientSecret $ClientSecret 

        Generate token with data pass from parameters.
    #>
  
    [CmdletBinding()]
    [alias("ntapp")]
    param(
        [Parameter(
            Mandatory = $True,
            HelpMessage = "Tenant ID of Azure AD tenant"
        )][string] $TenantID,
        [Parameter(
            Mandatory = $True,
            HelpMessage = "Client ID of azure APP"
        )][string] $ClientID,
        [Parameter(
            Mandatory = $True,
            HelpMessage = "Client secret of Azure APP"
        )][string] $ClientSecret

    )
    
    begin {
        
        $Params = @{
            "Uri"    = "https://login.microsoftonline.com/$TenantId/oauth2/v2.0/token"
            "Method" = "POST"
            "Body"   = @{
                'Grant_Type'    = "client_credentials"
                'Scope'         = "https://graph.microsoft.com/.default"
                'Client_Id'     = $ClientID
                'Client_Secret' = $ClientSecret
            }
        }
    }
    
    process {

        try {
 
            $AccessToken = (Invoke-RestMethod @params).access_token
            
        }
        catch {
            Throw $_ ;
        }
        
    }
    
    end {
        Return $AccessToken 
    }
}

```

Creamos el token: 

* TenantID, en la pestaña de overwiev 
* ClientID, en la pestaña de overwiev 
* ClientSecret, en la pestaña de certificados y secretos

```powershell
$AccesToken = Get-MgTokenApp -TenantID $TenantID -ClientID $ClientID -ClientSecret $ClientSecret 
```

Nos conectamos. 

```powershell
Connect-MGGraph -AccessToken (ConvertTo-SecureString $AccesToken -AsPlainText -Force)
```

Una vez conectados, ejecutamos el siguiente comando, y nos deberá aparecer la aplicacíon que nosotros hemos creado.

```powershell
Get-MGContext  | Select -Exp AppName

# OutPut: 
#    josecg.com
```



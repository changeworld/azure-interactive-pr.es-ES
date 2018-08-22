---
title: archivo de inclusión
description: archivo de inclusión
services: functions
author: tdykstra
manager: jeconnoc
ms.service: multiple
ms.topic: include
ms.date: 06/21/2018
ms.author: tdykstra
ms.custom: include file
ms.openlocfilehash: d1f9a07ce3d3b096b498e48b5c4f68c3454b2b37
ms.sourcegitcommit: e721422a57e6deb95245135fd9f4f5677c344d93
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/26/2018
ms.locfileid: "40079709"
---
La autenticación de Azure App Service permite compatibilidad inmediata con la autenticación en una aplicación de función de Azure. Se integra perfectamente con Facebook, Twitter, Microsoft Account, Google y Azure Active Directory. Agregará la autenticación de App Service para proteger la API de back-end de la aplicación web.

## <a name="enable-app-service-authentication"></a>Habilitación de la autenticación de App Service

1. Abra la aplicación de función en Azure Portal.

1. En **Características de la plataforma**, seleccione **Autenticación o autorización**.

    ![Selección de "Autenticación o autorización"](media/functions-first-serverless-web-app/6-authorization.jpg)

1. Seleccione los valores siguientes:
    
    | Configuración      |  Valor sugerido   | DESCRIPCIÓN                                        |
    | --- | --- | ---|
    | **Autenticación de App Service** | Por | Habilita la autenticación. |
    | **Acción necesaria cuando la solicitud no está autenticada** | Iniciar sesión con Azure Active Directory | Seleccione un método de autenticación configurado (a continuación). |
    | **Proveedores de autenticación** | Consulte a continuación | Consulte a continuación |
    | **Almacén de tokens** | Por | Permite que App Service almacene y administre los tokens. |
    | **URL de redirección externas permitidas** | La dirección URL de la aplicación, por ejemplo: https://firstserverlessweb.z4.web.core.windows.net/ | Las direcciones URL a las que puede redirigir App Service una vez autenticado un usuario. |

1. Seleccione **Azure Active Directory** para mostrar **Configuración de Azure Active Directory**.

    1. Seleccione **Rápido** como **Modo de administración** y rellene la información siguiente.
    
        | Configuración      |  Valor sugerido   | DESCRIPCIÓN                                        |
        | --- | --- | ---|
        | **Modo de administración** | Rápido, Crear nueva aplicación de AD | Configura automáticamente una entidad de servicio y la autenticación de Azure Active Directory. |
        | **Creación de la aplicación** | my-serverless-webapp | Escriba un nombre de aplicación único. |
    
    1. Haga clic en **Aceptar** para guardar la configuración de Azure Active Directory.

    ![Autenticación y autorización y configuración de Azure Active Directory](media/functions-first-serverless-web-app/6-create-aad.png)

1. Haga clic en **Save**(Guardar).


## <a name="modify-the-web-app-to-enable-authentication"></a>Modificación de la aplicación web para habilitar la autenticación

1. En Cloud Shell, asegúrese de que el directorio actual sea la carpeta **www/dist**.

    ```azurecli
    cd ~/functions-first-serverless-web-application/www/dist
    ```

1. Para habilitar la autenticación en la aplicación de función, agregue la siguiente línea de código a **settings.js**.

    `window.authEnabled = true`

    Realice el cambio y vea el resultado con los siguientes comandos o mediante un editor de la línea de comandos como VIM.

    ```azurecli
    echo "window.authEnabled = true" >> settings.js
    ```

    Confirme que el cambio se realizó en el archivo.

    ```azurecli
    cat settings.js
    ```

1. Cargue el archivo en Blob Storage.

    ```azurecli
    az storage blob upload -c \$web --account-name <storage account name> -f settings.js -n settings.js
    ```


## <a name="test-the-application"></a>Prueba de la aplicación

1. Abra la aplicación en un explorador. Haga clic en **Iniciar sesión** e inicie sesión.

1. Seleccione un archivo de imagen y cárguelo.

    ![Página de inicio de sesión](media/functions-first-serverless-web-app/6-aad-auth.png)
    

## <a name="summary"></a>Resumen

En este módulo, aprendió a agregar autenticación a la aplicación mediante la autenticación de Azure App Service.

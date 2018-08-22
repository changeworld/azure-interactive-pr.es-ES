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
<span data-ttu-id="5d0dc-103">La autenticación de Azure App Service permite compatibilidad inmediata con la autenticación en una aplicación de función de Azure.</span><span class="sxs-lookup"><span data-stu-id="5d0dc-103">Azure App Service Authentication enables turn-key authentication support in an Azure Function app.</span></span> <span data-ttu-id="5d0dc-104">Se integra perfectamente con Facebook, Twitter, Microsoft Account, Google y Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="5d0dc-104">It integrates seamlessly with Facebook, Twitter, Microsoft Account, Google, and Azure Active Directory.</span></span> <span data-ttu-id="5d0dc-105">Agregará la autenticación de App Service para proteger la API de back-end de la aplicación web.</span><span class="sxs-lookup"><span data-stu-id="5d0dc-105">You'll add App Service Authentication to protect the backend APIs of your web app.</span></span>

## <a name="enable-app-service-authentication"></a><span data-ttu-id="5d0dc-106">Habilitación de la autenticación de App Service</span><span class="sxs-lookup"><span data-stu-id="5d0dc-106">Enable App Service Authentication</span></span>

1. <span data-ttu-id="5d0dc-107">Abra la aplicación de función en Azure Portal.</span><span class="sxs-lookup"><span data-stu-id="5d0dc-107">Open the function app in the Azure Portal.</span></span>

1. <span data-ttu-id="5d0dc-108">En **Características de la plataforma**, seleccione **Autenticación o autorización**.</span><span class="sxs-lookup"><span data-stu-id="5d0dc-108">Under **Platform features**, select **Authentication/Authorization**.</span></span>

    ![Selección de "Autenticación o autorización"](media/functions-first-serverless-web-app/6-authorization.jpg)

1. <span data-ttu-id="5d0dc-110">Seleccione los valores siguientes:</span><span class="sxs-lookup"><span data-stu-id="5d0dc-110">Select the following values:</span></span>
    
    | <span data-ttu-id="5d0dc-111">Configuración</span><span class="sxs-lookup"><span data-stu-id="5d0dc-111">Setting</span></span>      |  <span data-ttu-id="5d0dc-112">Valor sugerido</span><span class="sxs-lookup"><span data-stu-id="5d0dc-112">Suggested value</span></span>   | <span data-ttu-id="5d0dc-113">DESCRIPCIÓN</span><span class="sxs-lookup"><span data-stu-id="5d0dc-113">Description</span></span>                                        |
    | --- | --- | ---|
    | <span data-ttu-id="5d0dc-114">**Autenticación de App Service**</span><span class="sxs-lookup"><span data-stu-id="5d0dc-114">**App Service Authentication**</span></span> | <span data-ttu-id="5d0dc-115">Por</span><span class="sxs-lookup"><span data-stu-id="5d0dc-115">On</span></span> | <span data-ttu-id="5d0dc-116">Habilita la autenticación.</span><span class="sxs-lookup"><span data-stu-id="5d0dc-116">Enable authentication.</span></span> |
    | <span data-ttu-id="5d0dc-117">**Acción necesaria cuando la solicitud no está autenticada**</span><span class="sxs-lookup"><span data-stu-id="5d0dc-117">**Action when request is not authenticated**</span></span> | <span data-ttu-id="5d0dc-118">Iniciar sesión con Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="5d0dc-118">Log in with Azure Active Directory</span></span> | <span data-ttu-id="5d0dc-119">Seleccione un método de autenticación configurado (a continuación).</span><span class="sxs-lookup"><span data-stu-id="5d0dc-119">Select a configured authentication method (below).</span></span> |
    | <span data-ttu-id="5d0dc-120">**Proveedores de autenticación**</span><span class="sxs-lookup"><span data-stu-id="5d0dc-120">**Authentication Providers**</span></span> | <span data-ttu-id="5d0dc-121">Consulte a continuación</span><span class="sxs-lookup"><span data-stu-id="5d0dc-121">See below</span></span> | <span data-ttu-id="5d0dc-122">Consulte a continuación</span><span class="sxs-lookup"><span data-stu-id="5d0dc-122">See below</span></span> |
    | <span data-ttu-id="5d0dc-123">**Almacén de tokens**</span><span class="sxs-lookup"><span data-stu-id="5d0dc-123">**Token store**</span></span> | <span data-ttu-id="5d0dc-124">Por</span><span class="sxs-lookup"><span data-stu-id="5d0dc-124">On</span></span> | <span data-ttu-id="5d0dc-125">Permite que App Service almacene y administre los tokens.</span><span class="sxs-lookup"><span data-stu-id="5d0dc-125">Allow App Service to store and manage tokens.</span></span> |
    | <span data-ttu-id="5d0dc-126">**URL de redirección externas permitidas**</span><span class="sxs-lookup"><span data-stu-id="5d0dc-126">**Allowed external redirect URLs**</span></span> | <span data-ttu-id="5d0dc-127">La dirección URL de la aplicación, por ejemplo: https://firstserverlessweb.z4.web.core.windows.net/</span><span class="sxs-lookup"><span data-stu-id="5d0dc-127">The URL of your application, for example: https://firstserverlessweb.z4.web.core.windows.net/</span></span> | <span data-ttu-id="5d0dc-128">Las direcciones URL a las que puede redirigir App Service una vez autenticado un usuario.</span><span class="sxs-lookup"><span data-stu-id="5d0dc-128">URL(s) that App Service is allowed to redirect to after a user is authenticated.</span></span> |

1. <span data-ttu-id="5d0dc-129">Seleccione **Azure Active Directory** para mostrar **Configuración de Azure Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="5d0dc-129">Select **Azure Active Directory** to reveal **Azure Active Directory Settings**.</span></span>

    1. <span data-ttu-id="5d0dc-130">Seleccione **Rápido** como **Modo de administración** y rellene la información siguiente.</span><span class="sxs-lookup"><span data-stu-id="5d0dc-130">Select **Express** as the **Management Mode** and fill in the following information.</span></span>
    
        | <span data-ttu-id="5d0dc-131">Configuración</span><span class="sxs-lookup"><span data-stu-id="5d0dc-131">Setting</span></span>      |  <span data-ttu-id="5d0dc-132">Valor sugerido</span><span class="sxs-lookup"><span data-stu-id="5d0dc-132">Suggested value</span></span>   | <span data-ttu-id="5d0dc-133">DESCRIPCIÓN</span><span class="sxs-lookup"><span data-stu-id="5d0dc-133">Description</span></span>                                        |
        | --- | --- | ---|
        | <span data-ttu-id="5d0dc-134">**Modo de administración**</span><span class="sxs-lookup"><span data-stu-id="5d0dc-134">**Management mode**</span></span> | <span data-ttu-id="5d0dc-135">Rápido, Crear nueva aplicación de AD</span><span class="sxs-lookup"><span data-stu-id="5d0dc-135">Express, Create new AD app</span></span> | <span data-ttu-id="5d0dc-136">Configura automáticamente una entidad de servicio y la autenticación de Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="5d0dc-136">Automatically set up a service principal and Azure Active Directory authentication.</span></span> |
        | <span data-ttu-id="5d0dc-137">**Creación de la aplicación**</span><span class="sxs-lookup"><span data-stu-id="5d0dc-137">**Create app**</span></span> | <span data-ttu-id="5d0dc-138">my-serverless-webapp</span><span class="sxs-lookup"><span data-stu-id="5d0dc-138">my-serverless-webapp</span></span> | <span data-ttu-id="5d0dc-139">Escriba un nombre de aplicación único.</span><span class="sxs-lookup"><span data-stu-id="5d0dc-139">Enter a unique application name.</span></span> |
    
    1. <span data-ttu-id="5d0dc-140">Haga clic en **Aceptar** para guardar la configuración de Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="5d0dc-140">Click **OK** to save the Azure Active Directory settings.</span></span>

    ![Autenticación y autorización y configuración de Azure Active Directory](media/functions-first-serverless-web-app/6-create-aad.png)

1. <span data-ttu-id="5d0dc-142">Haga clic en **Save**(Guardar).</span><span class="sxs-lookup"><span data-stu-id="5d0dc-142">Click **Save**.</span></span>


## <a name="modify-the-web-app-to-enable-authentication"></a><span data-ttu-id="5d0dc-143">Modificación de la aplicación web para habilitar la autenticación</span><span class="sxs-lookup"><span data-stu-id="5d0dc-143">Modify the web app to enable authentication</span></span>

1. <span data-ttu-id="5d0dc-144">En Cloud Shell, asegúrese de que el directorio actual sea la carpeta **www/dist**.</span><span class="sxs-lookup"><span data-stu-id="5d0dc-144">In the Cloud Shell, ensure that the current directory is the **www/dist** folder.</span></span>

    ```azurecli
    cd ~/functions-first-serverless-web-application/www/dist
    ```

1. <span data-ttu-id="5d0dc-145">Para habilitar la autenticación en la aplicación de función, agregue la siguiente línea de código a **settings.js**.</span><span class="sxs-lookup"><span data-stu-id="5d0dc-145">To enable authentication in your function app, append the following line of code to **settings.js**.</span></span>

    `window.authEnabled = true`

    <span data-ttu-id="5d0dc-146">Realice el cambio y vea el resultado con los siguientes comandos o mediante un editor de la línea de comandos como VIM.</span><span class="sxs-lookup"><span data-stu-id="5d0dc-146">Make the change and view the result by using the following commands or by using a command-line editor like VIM.</span></span>

    ```azurecli
    echo "window.authEnabled = true" >> settings.js
    ```

    <span data-ttu-id="5d0dc-147">Confirme que el cambio se realizó en el archivo.</span><span class="sxs-lookup"><span data-stu-id="5d0dc-147">Confirm the change was made to the file.</span></span>

    ```azurecli
    cat settings.js
    ```

1. <span data-ttu-id="5d0dc-148">Cargue el archivo en Blob Storage.</span><span class="sxs-lookup"><span data-stu-id="5d0dc-148">Upload the file to Blob storage.</span></span>

    ```azurecli
    az storage blob upload -c \$web --account-name <storage account name> -f settings.js -n settings.js
    ```


## <a name="test-the-application"></a><span data-ttu-id="5d0dc-149">Prueba de la aplicación</span><span class="sxs-lookup"><span data-stu-id="5d0dc-149">Test the application</span></span>

1. <span data-ttu-id="5d0dc-150">Abra la aplicación en un explorador.</span><span class="sxs-lookup"><span data-stu-id="5d0dc-150">Open the application in a browser.</span></span> <span data-ttu-id="5d0dc-151">Haga clic en **Iniciar sesión** e inicie sesión.</span><span class="sxs-lookup"><span data-stu-id="5d0dc-151">Click **Log in** and log in.</span></span>

1. <span data-ttu-id="5d0dc-152">Seleccione un archivo de imagen y cárguelo.</span><span class="sxs-lookup"><span data-stu-id="5d0dc-152">Select an image file and upload it.</span></span>

    ![Página de inicio de sesión](media/functions-first-serverless-web-app/6-aad-auth.png)
    

## <a name="summary"></a><span data-ttu-id="5d0dc-154">Resumen</span><span class="sxs-lookup"><span data-stu-id="5d0dc-154">Summary</span></span>

<span data-ttu-id="5d0dc-155">En este módulo, aprendió a agregar autenticación a la aplicación mediante la autenticación de Azure App Service.</span><span class="sxs-lookup"><span data-stu-id="5d0dc-155">In this module, you learned how to add authentication to the applcation using Azure App Service Authentication.</span></span>

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
ms.openlocfilehash: 7e51d3cd0533b4fb64d7dfa783af55266d536f54
ms.sourcegitcommit: e721422a57e6deb95245135fd9f4f5677c344d93
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/26/2018
ms.locfileid: "40079715"
---
<span data-ttu-id="146b4-103">En este momento, la aplicación es una galería funcional que le permite cargar y ver imágenes.</span><span class="sxs-lookup"><span data-stu-id="146b4-103">At this point, the application is a functional gallery that allows you to upload and view images.</span></span> <span data-ttu-id="146b4-104">En este módulo, aprenderá a usar Computer Vision API de Microsoft Cognitive Services para generar los títulos de las imágenes cargadas y guardarlos con los metadatos de imagen en Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="146b4-104">In this module, you learn how to use the Computer Vision API from Microsoft Cognitive Services to generate captions for uploaded images and save the captions with the image metadata in Cosmos DB.</span></span>

## <a name="create-a-computer-vision-account"></a><span data-ttu-id="146b4-105">Creación de una cuenta de Computer Vision</span><span class="sxs-lookup"><span data-stu-id="146b4-105">Create a Computer Vision account</span></span>

<span data-ttu-id="146b4-106">Microsoft Cognitive Services es una colección de servicios disponibles para los desarrolladores para crear sus aplicaciones más inteligentes.</span><span class="sxs-lookup"><span data-stu-id="146b4-106">Microsoft Cognitive Services is a collection of services available to developers to make their applications more intelligent.</span></span> <span data-ttu-id="146b4-107">Computer Vision API es un servicio sin servidor que procesa imágenes mediante algoritmos avanzados y devuelve información sobre cada imagen.</span><span class="sxs-lookup"><span data-stu-id="146b4-107">The Computer Vision API is a serverless service that processes images using advanced algorithms and returns information about each image.</span></span> <span data-ttu-id="146b4-108">Tiene un nivel gratuito que ofrece hasta 5000 llamadas API al mes.</span><span class="sxs-lookup"><span data-stu-id="146b4-108">It has a free tier that provides up to 5000 API calls per month.</span></span>

1. <span data-ttu-id="146b4-109">Asegúrese de que aún tiene la sesión iniciada en Cloud Shell.</span><span class="sxs-lookup"><span data-stu-id="146b4-109">Ensure you are still signed into the Cloud Shell.</span></span> <span data-ttu-id="146b4-110">En caso contrario, seleccione **Enter focus mode** (Entrar en modo de enfoque) para abrir una ventana de Cloud Shell.</span><span class="sxs-lookup"><span data-stu-id="146b4-110">If not, select **Enter focus mode** to open a Cloud Shell window.</span></span> 

1. <span data-ttu-id="146b4-111">Cree una cuenta de Cognitive Services del tipo **ComputerVision** con un nombre único en el grupo de recursos.</span><span class="sxs-lookup"><span data-stu-id="146b4-111">Create a new Cognitive Services account of kind **ComputerVision** with a unique name in your resource group.</span></span> <span data-ttu-id="146b4-112">Para el nivel gratis, use **F0** como SKU.</span><span class="sxs-lookup"><span data-stu-id="146b4-112">For the free tier, use **F0** as the SKU.</span></span> <span data-ttu-id="146b4-113">Si ya tiene una cuenta de Computer Vision, es posible que deba crear una cuenta estándar (S1); sin embargo, esto puede conllevar algunos costos.</span><span class="sxs-lookup"><span data-stu-id="146b4-113">If you already have an existing Computer Vision account, you may need to create a Standard account (S1); however, this may incur some costs.</span></span>

    ```azurecli
    az cognitiveservices account create -g first-serverless-app -n <computer vision account name> --kind ComputerVision --sku F0 -l westcentralus
    ```


## <a name="create-function-app-settings-for-computer-vision-url-and-key"></a><span data-ttu-id="146b4-114">Creación de una configuración de aplicación de función para la dirección URL y la clave de Computer Vision</span><span class="sxs-lookup"><span data-stu-id="146b4-114">Create Function App settings for Computer Vision URL and key</span></span>

<span data-ttu-id="146b4-115">Para llamar a Computer Vision API, se necesita una dirección URL y una clave.</span><span class="sxs-lookup"><span data-stu-id="146b4-115">To call the Computer Vision API, a URL and key are required.</span></span>

1. <span data-ttu-id="146b4-116">Obténgalas y guárdelas en variables de Bash.</span><span class="sxs-lookup"><span data-stu-id="146b4-116">Obtain the Computer Vision API key and URL and save them in bash variables.</span></span>

    ```azurecli
    export COMP_VISION_KEY=$(az cognitiveservices account keys list -g first-serverless-app -n <computer vision account name> --query key1 --output tsv)
    ```
    ```azurecli
    export COMP_VISION_URL=$(az cognitiveservices account show -g first-serverless-app -n <computer vision account name> --query endpoint --output tsv)
    ```

1. <span data-ttu-id="146b4-117">Cree una configuración de la aplicación llamada **COMP_VISION_KEY** y **COMP_VISION_URL**, respectivamente, en la aplicación de función.</span><span class="sxs-lookup"><span data-stu-id="146b4-117">Create app settings named **COMP_VISION_KEY** and **COMP_VISION_URL**, respectively, in the function app.</span></span>

    ```azurecli
    az functionapp config appsettings set -n <function app name> -g first-serverless-app --settings COMP_VISION_KEY=$COMP_VISION_KEY COMP_VISION_URL=$COMP_VISION_URL -o table
    ```


## <a name="call-computer-vision-api-from-resizeimage-function"></a><span data-ttu-id="146b4-118">Llamada a Computer Vision API desde la función ResizeImage</span><span class="sxs-lookup"><span data-stu-id="146b4-118">Call Computer Vision API from ResizeImage function</span></span>

<span data-ttu-id="146b4-119">En los pasos siguientes, modificará la función **ResizeImage** para llamar a Computer Vision API para describir cada imagen cargada y guardar la descripción en Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="146b4-119">In the following steps, you modify the **ResizeImage** function to call the Computer Vision API to describe each uploaded image and save the description in Cosmos DB.</span></span>

1. <span data-ttu-id="146b4-120">Abra la aplicación de función en Azure Portal.</span><span class="sxs-lookup"><span data-stu-id="146b4-120">Open your function app in the Azure Portal.</span></span>

1. <span data-ttu-id="146b4-121">**C#**</span><span class="sxs-lookup"><span data-stu-id="146b4-121">**C#**</span></span>

    1. <span data-ttu-id="146b4-122">(C#) Mediante el panel de navegación izquierdo, busque la función **ResizeImage** y abra su ventana de código.</span><span class="sxs-lookup"><span data-stu-id="146b4-122">(C#) Using the left navigation, locate the **ResizeImage** function and open its code window.</span></span>

    1. <span data-ttu-id="146b4-123">(C#) Reemplace el código por el contenido de [**/csharp/ResizeImage/run-module5.csx**](https://raw.githubusercontent.com/Azure-Samples/functions-first-serverless-web-application/master/csharp/ResizeImage/run-module5.csx).</span><span class="sxs-lookup"><span data-stu-id="146b4-123">(C#) Replace the code with the contents of [**/csharp/ResizeImage/run-module5.csx**](https://raw.githubusercontent.com/Azure-Samples/functions-first-serverless-web-application/master/csharp/ResizeImage/run-module5.csx).</span></span> <span data-ttu-id="146b4-124">Este código usa `HttpClient` para llamar a Computer Vision API y guardar su resultado en Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="146b4-124">This code uses `HttpClient` to call Computer Vision API and save its result in Cosmos DB.</span></span>

1. <span data-ttu-id="146b4-125">**JavaScript**</span><span class="sxs-lookup"><span data-stu-id="146b4-125">**JavaScript**</span></span>

    1. <span data-ttu-id="146b4-126">(JavaScript) Esta función requiere el paquete `axios` de npm para realizar una llamada HTTP a Computer Vision API.</span><span class="sxs-lookup"><span data-stu-id="146b4-126">(JavaScript) This function requires the `axios` package from npm to make an HTTP call to the Computer Vision API.</span></span> <span data-ttu-id="146b4-127">Para instalar el paquete de npm, haga clic en el nombre de la aplicación de función en el panel de navegación izquierdo y haga clic en **Características de la plataforma**.</span><span class="sxs-lookup"><span data-stu-id="146b4-127">To install the npm package, click on the Function App's name on the left navigation and click **Platform features**.</span></span>

    1. <span data-ttu-id="146b4-128">(JavaScript) Haga clic en **Consola** para mostrar una ventana de consola.</span><span class="sxs-lookup"><span data-stu-id="146b4-128">(JavaScript) Click **Console** to reveal a console window.</span></span>

    1. <span data-ttu-id="146b4-129">(JavaScript) Ejecute el comando `npm install axios` en la consola.</span><span class="sxs-lookup"><span data-stu-id="146b4-129">(JavaScript) Run the command `npm install axios` in the console.</span></span> <span data-ttu-id="146b4-130">La operación puede tardar un minuto o dos en completarse.</span><span class="sxs-lookup"><span data-stu-id="146b4-130">It may take a minute or two to complete the operation.</span></span>

    1. <span data-ttu-id="146b4-131">(JavaScript) Haga clic en el nombre de función (**ResizeImage**) en el panel de navegación izquierdo para mostrar la función y, a continuación, reemplace **index.js** por el contenido de [**/javascript/ResizeImage/index-module5.js**](https://raw.githubusercontent.com/Azure-Samples/functions-first-serverless-web-application/master/javascript/ResizeImage/index-module5.js).</span><span class="sxs-lookup"><span data-stu-id="146b4-131">(JavaScript) Click on the function name (**ResizeImage**) in the left navigation to reveal the function, and then replace all of **index.js** with the contents of [**/javascript/ResizeImage/index-module5.js**](https://raw.githubusercontent.com/Azure-Samples/functions-first-serverless-web-application/master/javascript/ResizeImage/index-module5.js).</span></span>

1. <span data-ttu-id="146b4-132">Haga clic en **Registros** debajo de la ventana de código para expandir el panel de registros.</span><span class="sxs-lookup"><span data-stu-id="146b4-132">Click **Logs** below the code window to expand the logs panel.</span></span>

1. <span data-ttu-id="146b4-133">Haga clic en **Save**(Guardar).</span><span class="sxs-lookup"><span data-stu-id="146b4-133">Click **Save**.</span></span> <span data-ttu-id="146b4-134">Compruebe el panel de registros para asegurarse de que la función se guarda correctamente y no hay errores.</span><span class="sxs-lookup"><span data-stu-id="146b4-134">Check the logs panel to ensure the function is successfully saved and there are no errors.</span></span>


## <a name="test-the-application"></a><span data-ttu-id="146b4-135">Prueba de la aplicación</span><span class="sxs-lookup"><span data-stu-id="146b4-135">Test the application</span></span>

1. <span data-ttu-id="146b4-136">Abra la aplicación en un explorador.</span><span class="sxs-lookup"><span data-stu-id="146b4-136">Open the application in a browser.</span></span> <span data-ttu-id="146b4-137">Seleccione un archivo de imagen y cárguelo.</span><span class="sxs-lookup"><span data-stu-id="146b4-137">Select an image file, and upload it.</span></span>

1. <span data-ttu-id="146b4-138">Al cabo de unos segundos, la miniatura de la nueva imagen aparecerá en la página.</span><span class="sxs-lookup"><span data-stu-id="146b4-138">After a few seconds, the thumbnail of the new image should appear on the page.</span></span> <span data-ttu-id="146b4-139">Mantenga el mouse sobre la imagen para ver la descripción generada por Computer Vision.</span><span class="sxs-lookup"><span data-stu-id="146b4-139">Hover over the image to see the description generated by Computer Vision.</span></span>


## <a name="summary"></a><span data-ttu-id="146b4-140">Resumen</span><span class="sxs-lookup"><span data-stu-id="146b4-140">Summary</span></span>

<span data-ttu-id="146b4-141">En este módulo, ha aprendido a generar automáticamente títulos para cada imagen cargada con Computer Vision API de Microsoft Cognitive Services.</span><span class="sxs-lookup"><span data-stu-id="146b4-141">In this module, you learned how to automatically generate captions for each uploaded image using Microsoft Cognitive Services Computer Vision API.</span></span> <span data-ttu-id="146b4-142">A continuación, aprenderá a agregar autenticación a la aplicación mediante la autenticación de Azure App Service.</span><span class="sxs-lookup"><span data-stu-id="146b4-142">Next, you learn how to add authentication to the application using Azure App Service Authentication.</span></span>
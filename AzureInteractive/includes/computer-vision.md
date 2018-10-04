---
title: archivo de inclusión
description: archivo de inclusión
services: functions
author: ggailey777
manager: jeconnoc
ms.service: multiple
ms.topic: include
ms.date: 06/21/2018
ms.author: glenga
ms.custom: include file
ms.openlocfilehash: f51b864cab14273c1e88dd85d22400e0e76ef770
ms.sourcegitcommit: 81587470a181e314242c7a97cd0f91c82d4fe232
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/01/2018
ms.locfileid: "47460012"
---
En este momento, la aplicación es una galería funcional que le permite cargar y ver imágenes. En este módulo, aprenderá a usar Computer Vision API de Microsoft Cognitive Services para generar los títulos de las imágenes cargadas y guardarlos con los metadatos de imagen en Cosmos DB.

## <a name="create-a-computer-vision-account"></a>Creación de una cuenta de Computer Vision

Microsoft Cognitive Services es una colección de servicios disponibles para los desarrolladores para crear sus aplicaciones más inteligentes. Computer Vision API es un servicio sin servidor que procesa imágenes mediante algoritmos avanzados y devuelve información sobre cada imagen. Tiene un nivel gratuito que ofrece hasta 5000 llamadas API al mes.

1. Asegúrese de que aún tiene la sesión iniciada en Cloud Shell. En caso contrario, seleccione **Enter focus mode** (Entrar en modo de enfoque) para abrir una ventana de Cloud Shell. 

1. Cree una cuenta de Cognitive Services del tipo **ComputerVision** con un nombre único en el grupo de recursos. Para el nivel gratis, use **F0** como SKU. Si ya tiene una cuenta de Computer Vision, es posible que deba crear una cuenta estándar (S1); sin embargo, esto puede conllevar algunos costos.

    ```azurecli
    az cognitiveservices account create -g first-serverless-app -n <computer vision account name> --kind ComputerVision --sku F0 -l westcentralus
    ```


## <a name="create-function-app-settings-for-computer-vision-url-and-key"></a>Creación de una configuración de aplicación de función para la dirección URL y la clave de Computer Vision

Para llamar a Computer Vision API, se necesita una dirección URL y una clave.

1. Obténgalas y guárdelas en variables de Bash.

    ```azurecli
    export COMP_VISION_KEY=$(az cognitiveservices account keys list -g first-serverless-app -n <computer vision account name> --query key1 --output tsv)
    ```
    ```azurecli
    export COMP_VISION_URL=$(az cognitiveservices account show -g first-serverless-app -n <computer vision account name> --query endpoint --output tsv)
    ```

1. Cree una configuración de la aplicación llamada **COMP_VISION_KEY** y **COMP_VISION_URL**, respectivamente, en la aplicación de función.

    ```azurecli
    az functionapp config appsettings set -n <function app name> -g first-serverless-app --settings COMP_VISION_KEY=$COMP_VISION_KEY COMP_VISION_URL=$COMP_VISION_URL -o table
    ```


## <a name="call-computer-vision-api-from-resizeimage-function"></a>Llamada a Computer Vision API desde la función ResizeImage

En los pasos siguientes, modificará la función **ResizeImage** para llamar a Computer Vision API para describir cada imagen cargada y guardar la descripción en Cosmos DB.

1. Abra la aplicación de función en Azure Portal.

1. **C#**

    1. (C#) Mediante el panel de navegación izquierdo, busque la función **ResizeImage** y abra su ventana de código.

    1. (C#) Reemplace el código por el contenido de [**/csharp/ResizeImage/run-module5.csx**](https://raw.githubusercontent.com/Azure-Samples/functions-first-serverless-web-application/master/csharp/ResizeImage/run-module5.csx). Este código usa `HttpClient` para llamar a Computer Vision API y guardar su resultado en Cosmos DB.

1. **JavaScript**

    1. (JavaScript) Esta función requiere el paquete `axios` de npm para realizar una llamada HTTP a Computer Vision API. Para instalar el paquete de npm, haga clic en el nombre de la aplicación de función en el panel de navegación izquierdo y haga clic en **Características de la plataforma**.

    1. (JavaScript) Haga clic en **Consola** para mostrar una ventana de consola.

    1. (JavaScript) Ejecute el comando `npm install axios` en la consola. La operación puede tardar un minuto o dos en completarse.

    1. (JavaScript) Haga clic en el nombre de función (**ResizeImage**) en el panel de navegación izquierdo para mostrar la función y, a continuación, reemplace **index.js** por el contenido de [**/javascript/ResizeImage/index-module5.js**](https://raw.githubusercontent.com/Azure-Samples/functions-first-serverless-web-application/master/javascript/ResizeImage/index-module5.js).

1. Haga clic en **Registros** debajo de la ventana de código para expandir el panel de registros.

1. Haga clic en **Save**(Guardar). Compruebe el panel de registros para asegurarse de que la función se guarda correctamente y no hay errores.


## <a name="test-the-application"></a>Prueba de la aplicación

1. Abra la aplicación en un explorador. Seleccione un archivo de imagen y cárguelo.

1. Al cabo de unos segundos, la miniatura de la nueva imagen aparecerá en la página. Mantenga el mouse sobre la imagen para ver la descripción generada por Computer Vision.


## <a name="summary"></a>Resumen

En este módulo, ha aprendido a generar automáticamente títulos para cada imagen cargada con Computer Vision API de Microsoft Cognitive Services. A continuación, aprenderá a agregar autenticación a la aplicación mediante la autenticación de Azure App Service.

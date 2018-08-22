---
title: archivo de inclusión
description: archivo de inclusión
services: functions
author: tdykstra
manager: jeconnoc
ms.service: multiple
ms.topic: include
ms.date: 06/25/2018
ms.author: tdykstra
ms.custom: include file
ms.openlocfilehash: 1c4aaf7afd9fbc54dd34c2cca13a8b74e947c16a
ms.sourcegitcommit: e721422a57e6deb95245135fd9f4f5677c344d93
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/26/2018
ms.locfileid: "40079699"
---
<span data-ttu-id="aee65-103">Ha creado correctamente una aplicación sin servidor completa mediante servicios de Azure.</span><span class="sxs-lookup"><span data-stu-id="aee65-103">You've successfully created a full-featured, serverless application using Azure services.</span></span>

## <a name="clean-up-resources"></a><span data-ttu-id="aee65-104">Limpieza de recursos</span><span class="sxs-lookup"><span data-stu-id="aee65-104">Clean up resources</span></span>

<span data-ttu-id="aee65-105">Cuando haya terminado de trabajar con esta aplicación, puede usar el siguiente comando para eliminar todos los recursos creados durante el tutorial:</span><span class="sxs-lookup"><span data-stu-id="aee65-105">When you are done working with this application, you can use the following command to delete all resources created during the tutorial:</span></span>

```azurecli
az group delete --name first-serverless-app
```

<span data-ttu-id="aee65-106">Cuando se le solicite, escriba `y`.</span><span class="sxs-lookup"><span data-stu-id="aee65-106">Type `y` when prompted.</span></span>  

## <a name="next-steps"></a><span data-ttu-id="aee65-107">Pasos siguientes</span><span class="sxs-lookup"><span data-stu-id="aee65-107">Next steps</span></span>

<span data-ttu-id="aee65-108">En este tutorial aprendió lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="aee65-108">In this tutorial, you learned how to:</span></span>
> [!div class="checklist"]
> * <span data-ttu-id="aee65-109">Configurar Azure Blob Storage para hospedar un sitio web estático y las imágenes cargadas.</span><span class="sxs-lookup"><span data-stu-id="aee65-109">Configure Azure Blob storage to host a static website and uploaded images.</span></span>
> * <span data-ttu-id="aee65-110">Cargar imágenes en Azure Blob Storage mediante Azure Functions.</span><span class="sxs-lookup"><span data-stu-id="aee65-110">Upload images to Azure Blob storage using Azure Functions.</span></span>
> * <span data-ttu-id="aee65-111">Cambiar el tamaño de las imágenes con Azure Functions.</span><span class="sxs-lookup"><span data-stu-id="aee65-111">Resize images using Azure Functions.</span></span>
> * <span data-ttu-id="aee65-112">Almacenar los metadatos de imagen en Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="aee65-112">Store image metadata in Azure Cosmos DB.</span></span>
> * <span data-ttu-id="aee65-113">Usar Vision API de Cognitive Services para generar automáticamente títulos de imágenes.</span><span class="sxs-lookup"><span data-stu-id="aee65-113">Use Cognitive Services Vision API to auto-generate image captions.</span></span>
> * <span data-ttu-id="aee65-114">Usar Azure Active Directory para proteger la aplicación web mediante la autenticación de los usuarios.</span><span class="sxs-lookup"><span data-stu-id="aee65-114">Use Azure Active Directory to secure the web app by authenticating users.</span></span>

<span data-ttu-id="aee65-115">Para aprender a conectar incluso más servicios a Functions, siga con el tutorial de Logic Apps.</span><span class="sxs-lookup"><span data-stu-id="aee65-115">To learn how to connect even more services to Functions, continue to the Logic Apps tutorial.</span></span> 

> [!div class="nextstepaction"]
> [<span data-ttu-id="aee65-116">Functions con Logic Apps</span><span class="sxs-lookup"><span data-stu-id="aee65-116">Functions with Logic Apps</span></span>](https://docs.microsoft.com/azure/azure-functions/functions-twitter-email)
---
title: archivo de inclusión
description: archivo de inclusión
services: functions
author: ggailey777
manager: jeconnoc
ms.service: multiple
ms.topic: include
ms.date: 06/25/2018
ms.author: glenga
ms.custom: include file
ms.openlocfilehash: a66fcc3a406c79fcf9881ddaaaf8330f5b373043
ms.sourcegitcommit: 81587470a181e314242c7a97cd0f91c82d4fe232
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/01/2018
ms.locfileid: "47460011"
---
<span data-ttu-id="698b5-103">Ha creado correctamente una aplicación sin servidor completa mediante servicios de Azure.</span><span class="sxs-lookup"><span data-stu-id="698b5-103">You've successfully created a full-featured, serverless application using Azure services.</span></span>

## <a name="clean-up-resources"></a><span data-ttu-id="698b5-104">Limpieza de recursos</span><span class="sxs-lookup"><span data-stu-id="698b5-104">Clean up resources</span></span>

<span data-ttu-id="698b5-105">Cuando haya terminado de trabajar con esta aplicación, puede usar el siguiente comando para eliminar todos los recursos creados durante el tutorial:</span><span class="sxs-lookup"><span data-stu-id="698b5-105">When you are done working with this application, you can use the following command to delete all resources created during the tutorial:</span></span>

```azurecli
az group delete --name first-serverless-app
```

<span data-ttu-id="698b5-106">Cuando se le solicite, escriba `y`.</span><span class="sxs-lookup"><span data-stu-id="698b5-106">Type `y` when prompted.</span></span>  

## <a name="next-steps"></a><span data-ttu-id="698b5-107">Pasos siguientes</span><span class="sxs-lookup"><span data-stu-id="698b5-107">Next steps</span></span>

<span data-ttu-id="698b5-108">En este tutorial aprendió lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="698b5-108">In this tutorial, you learned how to:</span></span>
> [!div class="checklist"]
> * <span data-ttu-id="698b5-109">Configurar Azure Blob Storage para hospedar un sitio web estático y las imágenes cargadas.</span><span class="sxs-lookup"><span data-stu-id="698b5-109">Configure Azure Blob storage to host a static website and uploaded images.</span></span>
> * <span data-ttu-id="698b5-110">Cargar imágenes en Azure Blob Storage mediante Azure Functions.</span><span class="sxs-lookup"><span data-stu-id="698b5-110">Upload images to Azure Blob storage using Azure Functions.</span></span>
> * <span data-ttu-id="698b5-111">Cambiar el tamaño de las imágenes con Azure Functions.</span><span class="sxs-lookup"><span data-stu-id="698b5-111">Resize images using Azure Functions.</span></span>
> * <span data-ttu-id="698b5-112">Almacenar los metadatos de imagen en Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="698b5-112">Store image metadata in Azure Cosmos DB.</span></span>
> * <span data-ttu-id="698b5-113">Usar Vision API de Cognitive Services para generar automáticamente títulos de imágenes.</span><span class="sxs-lookup"><span data-stu-id="698b5-113">Use Cognitive Services Vision API to auto-generate image captions.</span></span>
> * <span data-ttu-id="698b5-114">Usar Azure Active Directory para proteger la aplicación web mediante la autenticación de los usuarios.</span><span class="sxs-lookup"><span data-stu-id="698b5-114">Use Azure Active Directory to secure the web app by authenticating users.</span></span>

<span data-ttu-id="698b5-115">Para aprender a conectar incluso más servicios a Functions, siga con el tutorial de Logic Apps.</span><span class="sxs-lookup"><span data-stu-id="698b5-115">To learn how to connect even more services to Functions, continue to the Logic Apps tutorial.</span></span> 

> [!div class="nextstepaction"]
> [<span data-ttu-id="698b5-116">Functions con Logic Apps</span><span class="sxs-lookup"><span data-stu-id="698b5-116">Functions with Logic Apps</span></span>](https://docs.microsoft.com/azure/azure-functions/functions-twitter-email)

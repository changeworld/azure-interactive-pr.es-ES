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
ms.openlocfilehash: 0f86f2698a3a0c1e20272c335b63faf03b4b92d6
ms.sourcegitcommit: 81587470a181e314242c7a97cd0f91c82d4fe232
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/01/2018
ms.locfileid: "47460031"
---
<span data-ttu-id="76e5c-103">En este tutorial, implementará una aplicación web sencilla que presenta una interfaz de usuario basada en HTML.</span><span class="sxs-lookup"><span data-stu-id="76e5c-103">In this tutorial, you deploy a simple web application that presents an HTML-based user interface.</span></span> <span data-ttu-id="76e5c-104">Un back-end sin servidor permite que la aplicación cargue imágenes y obtenga automáticamente títulos que las describan.</span><span class="sxs-lookup"><span data-stu-id="76e5c-104">A serverless backend enables the application to upload images and automatically get captions describing them.</span></span>

![Ejecución de la aplicación web](media/functions-first-serverless-web-app/0-app-screenshot-finished.png)

<span data-ttu-id="76e5c-106">El siguiente diagrama muestra los servicios de Azure utilizados por la aplicación:</span><span class="sxs-lookup"><span data-stu-id="76e5c-106">The following diagram shows the Azure services used by the application:</span></span>

1. <span data-ttu-id="76e5c-107">Blob Storage sirve contenido web estático (HTML, CSS, JS) y almacena imágenes.</span><span class="sxs-lookup"><span data-stu-id="76e5c-107">Blob Storage serves static web content (HTML, CSS, JS) and stores images.</span></span>
2. <span data-ttu-id="76e5c-108">Azure Functions administra la carga, el cambio de tamaño y el almacenamiento de metadatos de las imágenes.</span><span class="sxs-lookup"><span data-stu-id="76e5c-108">Azure Functions manages image uploads, resizing, and metadata storage.</span></span>
3. <span data-ttu-id="76e5c-109">Cosmos DB almacena metadatos de imagen.</span><span class="sxs-lookup"><span data-stu-id="76e5c-109">Cosmos DB stores image metadata.</span></span>
4. <span data-ttu-id="76e5c-110">Logic Apps obtiene los títulos de imágenes de Computer Vision API.</span><span class="sxs-lookup"><span data-stu-id="76e5c-110">Logic Apps gets image captions from Computer Vision API.</span></span>
5. <span data-ttu-id="76e5c-111">Azure Active Directory administra la autenticación del usuario.</span><span class="sxs-lookup"><span data-stu-id="76e5c-111">Azure Active Directory manages user authentication.</span></span>

![Diagrama de la arquitectura de la solución](media/functions-first-serverless-web-app/0-architecture.jpg)

<span data-ttu-id="76e5c-113">En este tutorial, aprenderá a:</span><span class="sxs-lookup"><span data-stu-id="76e5c-113">In this tutorial, you learn how to:</span></span>
> [!div class="checklist"]
> * <span data-ttu-id="76e5c-114">Configurar Azure Blob Storage para hospedar un sitio web estático y las imágenes cargadas.</span><span class="sxs-lookup"><span data-stu-id="76e5c-114">Configure Azure Blob storage to host a static website and uploaded images.</span></span>
> * <span data-ttu-id="76e5c-115">Cargar imágenes en Azure Blob Storage mediante Azure Functions.</span><span class="sxs-lookup"><span data-stu-id="76e5c-115">Upload images to Azure Blob storage using Azure Functions.</span></span>
> * <span data-ttu-id="76e5c-116">Cambiar el tamaño de las imágenes con Azure Functions.</span><span class="sxs-lookup"><span data-stu-id="76e5c-116">Resize images using Azure Functions.</span></span>
> * <span data-ttu-id="76e5c-117">Almacenar los metadatos de imagen en Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="76e5c-117">Store image metadata in Azure Cosmos DB.</span></span>
> * <span data-ttu-id="76e5c-118">Usar Vision API de Cognitive Services para generar automáticamente títulos de imágenes.</span><span class="sxs-lookup"><span data-stu-id="76e5c-118">Use Cognitive Services Vision API to auto-generate image captions.</span></span>
> * <span data-ttu-id="76e5c-119">Usar Azure Active Directory para proteger la aplicación web mediante la autenticación de los usuarios.</span><span class="sxs-lookup"><span data-stu-id="76e5c-119">Use Azure Active Directory to secure the web app by authenticating users.</span></span>

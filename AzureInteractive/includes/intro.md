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
ms.openlocfilehash: d651fd3d03f2678d625e60f9ab1e9f59e623964f
ms.sourcegitcommit: e721422a57e6deb95245135fd9f4f5677c344d93
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/26/2018
ms.locfileid: "40079684"
---
<span data-ttu-id="357a1-103">En este tutorial, implementará una aplicación web sencilla que presenta una interfaz de usuario basada en HTML.</span><span class="sxs-lookup"><span data-stu-id="357a1-103">In this tutorial, you deploy a simple web application that presents an HTML-based user interface.</span></span> <span data-ttu-id="357a1-104">Un back-end sin servidor permite que la aplicación cargue imágenes y obtenga automáticamente títulos que las describan.</span><span class="sxs-lookup"><span data-stu-id="357a1-104">A serverless backend enables the application to upload images and automatically get captions describing them.</span></span>

![Ejecución de la aplicación web](media/functions-first-serverless-web-app/0-app-screenshot-finished.png)

<span data-ttu-id="357a1-106">El siguiente diagrama muestra los servicios de Azure utilizados por la aplicación:</span><span class="sxs-lookup"><span data-stu-id="357a1-106">The following diagram shows the Azure services used by the application:</span></span>

1. <span data-ttu-id="357a1-107">Blob Storage sirve contenido web estático (HTML, CSS, JS) y almacena imágenes.</span><span class="sxs-lookup"><span data-stu-id="357a1-107">Blob Storage serves static web content (HTML, CSS, JS) and stores images.</span></span>
2. <span data-ttu-id="357a1-108">Azure Functions administra la carga, el cambio de tamaño y el almacenamiento de metadatos de las imágenes.</span><span class="sxs-lookup"><span data-stu-id="357a1-108">Azure Functions manages image uploads, resizing, and metadata storage.</span></span>
3. <span data-ttu-id="357a1-109">Cosmos DB almacena metadatos de imagen.</span><span class="sxs-lookup"><span data-stu-id="357a1-109">Cosmos DB stores image metadata.</span></span>
4. <span data-ttu-id="357a1-110">Logic Apps obtiene los títulos de imágenes de Computer Vision API.</span><span class="sxs-lookup"><span data-stu-id="357a1-110">Logic Apps gets image captions from Computer Vision API.</span></span>
5. <span data-ttu-id="357a1-111">Azure Active Directory administra la autenticación del usuario.</span><span class="sxs-lookup"><span data-stu-id="357a1-111">Azure Active Directory manages user authentication.</span></span>

![Diagrama de la arquitectura de la solución](media/functions-first-serverless-web-app/0-architecture.jpg)

<span data-ttu-id="357a1-113">En este tutorial, aprenderá a:</span><span class="sxs-lookup"><span data-stu-id="357a1-113">In this tutorial, you learn how to:</span></span>
> [!div class="checklist"]
> * <span data-ttu-id="357a1-114">Configurar Azure Blob Storage para hospedar un sitio web estático y las imágenes cargadas.</span><span class="sxs-lookup"><span data-stu-id="357a1-114">Configure Azure Blob storage to host a static website and uploaded images.</span></span>
> * <span data-ttu-id="357a1-115">Cargar imágenes en Azure Blob Storage mediante Azure Functions.</span><span class="sxs-lookup"><span data-stu-id="357a1-115">Upload images to Azure Blob storage using Azure Functions.</span></span>
> * <span data-ttu-id="357a1-116">Cambiar el tamaño de las imágenes con Azure Functions.</span><span class="sxs-lookup"><span data-stu-id="357a1-116">Resize images using Azure Functions.</span></span>
> * <span data-ttu-id="357a1-117">Almacenar los metadatos de imagen en Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="357a1-117">Store image metadata in Azure Cosmos DB.</span></span>
> * <span data-ttu-id="357a1-118">Usar Vision API de Cognitive Services para generar automáticamente títulos de imágenes.</span><span class="sxs-lookup"><span data-stu-id="357a1-118">Use Cognitive Services Vision API to auto-generate image captions.</span></span>
> * <span data-ttu-id="357a1-119">Usar Azure Active Directory para proteger la aplicación web mediante la autenticación de los usuarios.</span><span class="sxs-lookup"><span data-stu-id="357a1-119">Use Azure Active Directory to secure the web app by authenticating users.</span></span>
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
En este tutorial, implementará una aplicación web sencilla que presenta una interfaz de usuario basada en HTML. Un back-end sin servidor permite que la aplicación cargue imágenes y obtenga automáticamente títulos que las describan.

![Ejecución de la aplicación web](media/functions-first-serverless-web-app/0-app-screenshot-finished.png)

El siguiente diagrama muestra los servicios de Azure utilizados por la aplicación:

1. Blob Storage sirve contenido web estático (HTML, CSS, JS) y almacena imágenes.
2. Azure Functions administra la carga, el cambio de tamaño y el almacenamiento de metadatos de las imágenes.
3. Cosmos DB almacena metadatos de imagen.
4. Logic Apps obtiene los títulos de imágenes de Computer Vision API.
5. Azure Active Directory administra la autenticación del usuario.

![Diagrama de la arquitectura de la solución](media/functions-first-serverless-web-app/0-architecture.jpg)

En este tutorial, aprenderá a:
> [!div class="checklist"]
> * Configurar Azure Blob Storage para hospedar un sitio web estático y las imágenes cargadas.
> * Cargar imágenes en Azure Blob Storage mediante Azure Functions.
> * Cambiar el tamaño de las imágenes con Azure Functions.
> * Almacenar los metadatos de imagen en Azure Cosmos DB.
> * Usar Vision API de Cognitive Services para generar automáticamente títulos de imágenes.
> * Usar Azure Active Directory para proteger la aplicación web mediante la autenticación de los usuarios.

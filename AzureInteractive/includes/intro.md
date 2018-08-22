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
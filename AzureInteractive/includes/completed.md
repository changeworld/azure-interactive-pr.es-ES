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
Ha creado correctamente una aplicación sin servidor completa mediante servicios de Azure.

## <a name="clean-up-resources"></a>Limpieza de recursos

Cuando haya terminado de trabajar con esta aplicación, puede usar el siguiente comando para eliminar todos los recursos creados durante el tutorial:

```azurecli
az group delete --name first-serverless-app
```

Cuando se le solicite, escriba `y`.  

## <a name="next-steps"></a>Pasos siguientes

En este tutorial aprendió lo siguiente:
> [!div class="checklist"]
> * Configurar Azure Blob Storage para hospedar un sitio web estático y las imágenes cargadas.
> * Cargar imágenes en Azure Blob Storage mediante Azure Functions.
> * Cambiar el tamaño de las imágenes con Azure Functions.
> * Almacenar los metadatos de imagen en Azure Cosmos DB.
> * Usar Vision API de Cognitive Services para generar automáticamente títulos de imágenes.
> * Usar Azure Active Directory para proteger la aplicación web mediante la autenticación de los usuarios.

Para aprender a conectar incluso más servicios a Functions, siga con el tutorial de Logic Apps. 

> [!div class="nextstepaction"]
> [Functions con Logic Apps](https://docs.microsoft.com/azure/azure-functions/functions-twitter-email)
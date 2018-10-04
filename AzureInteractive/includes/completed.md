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

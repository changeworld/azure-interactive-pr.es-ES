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
ms.openlocfilehash: 2202cdebe77668972372983a0e802d00edabf6dd
ms.sourcegitcommit: e721422a57e6deb95245135fd9f4f5677c344d93
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/26/2018
ms.locfileid: "40079689"
---
Azure Cosmos DB es la base de datos multimodelo, globalmente distribuida y sin servidor de Microsoft. En este módulo, aprenderá a usar Azure Functions para almacenar y recuperar metadatos de imagen como documentos JSON en Cosmos DB.

## <a name="create-a-cosmos-db-account-database-and-collection"></a>Creación de una cuenta, una base de datos y una colección de Cosmos DB

Una cuenta de Cosmos DB es un recurso de Azure que contiene bases de datos de Cosmos DB.

1. Asegúrese de que aún tiene la sesión iniciada en Cloud Shell.  En caso contrario, seleccione **Enter focus mode** (Entrar en modo de enfoque) para abrir una ventana de Cloud Shell. 

1. Cree una cuenta de Cosmos DB con un nombre único en el mismo grupo de recursos que los otros recursos de este tutorial.

    ```azurecli
    az cosmosdb create -g first-serverless-app -n <cosmos db account name>
    ```

1. Después de crear la cuenta de Cosmos DB, cree una base de datos llamada **imagesdb** en la cuenta.

    ```azurecli
    az cosmosdb database create -g first-serverless-app -n <cosmos db account name> --db-name imagesdb
    ```

1. Una vez creada la base de datos, cree una colección llamada **images** en la base de datos con un rendimiento de 400 unidades de solicitud (RU).

    ```azurecli
    az cosmosdb collection create -g first-serverless-app -n <cosmos db account name> --db-name imagesdb --collection-name images --throughput 400
    ```


## <a name="save-a-document-to-cosmos-db-when-a-thumbnail-is-created"></a>Guardar un documento en Cosmos DB cuando se crea una miniatura

El enlace de salida de Cosmos DB le permite crear documentos en una colección de Cosmos DB desde Azure Functions. En los pasos siguientes, configurará un enlace de salida de Cosmos DB en la función **ResizeImage** y modificará la función para devolver un documento (objeto) para guardar.

1. Abra la aplicación de función en Azure Portal.

1. En el panel de navegación izquierdo, expanda la función **ResizeImage** y, a continuación, seleccione **Integrar**.

1. En **Salida**, haga clic en **Nueva salida**.

1. Busque el elemento **Azure Cosmos DB** y selecciónelo. Después, haga clic en **Seleccionar**.

    ![Selección de la nueva salida](media/functions-first-serverless-web-app/4-new-output.jpg)

1. Rellene los campos de **Azure Cosmos DB output** (Salida de Azure Cosmos DB) con los valores siguientes.

    | Configuración      |  Valor sugerido   | DESCRIPCIÓN                                        |
    | --- | --- | ---|
    | **Nombre del parámetro de documento** | Seleccione **Use function return value** (Usar valor de devolución de función). | El valor del cuadro de texto se establece automáticamente en **$return**. |
    | **Nombre de la base de datos** | imagesdb | Use el nombre de la base de datos que ha creado. |
    | **Nombre de colección** | images | Use el nombre de la colección que ha creado. |

1. Junto a **Conexión de la cuenta de Azure Cosmos DB**, haga clic en **new** (nuevo). Seleccione la cuenta de Cosmos DB que creó anteriormente.

    ![Especificación de la configuración del enlace de salida de Azure Cosmos DB](media/functions-first-serverless-web-app/4-cosmos-db-output.png)

1. Haga clic en **Guardar** para crear el enlace de salida de Cosmos DB.

1. Haga clic en el nombre de función **ResizeImage** a la izquierda para abrir la función.

1. **C#**

    1. (C#) Cambie el tipo de valor devuelto de la función de **void** a **object**.

    1. (C#) Al final de la función, agregue el siguiente bloque de código para devolver el documento que se va a guardar:
    
        ```csharp
        return new {
            id = name,
            imgPath = "/images/" + name,
            thumbnailPath = "/thumbnails/" + name
        };
        ```
    
        ![run.csx para la función ResizeImages tras las modificaciones](media/functions-first-serverless-web-app/4-update-function.png)

1. **JavaScript**

    1. (JavaScript) Cambie la instrucción `context.done()` de la cláusula `else` para devolver el documento que se va a guardar a Cosmos DB.

    ```javascript
    if (error) {
        context.done(error);
    } else {
        context.bindings.thumbnail = stream;
        context.done(null, {
            id: context.bindingData.name,
            imgPath: "/images/" + context.bindingData.name,
            thumbnailPath: "/thumbnails/" + context.bindingData.name
        });
    }
    ```

1. Haga clic en **Registros** debajo de la ventana de código para expandir el panel de registros.

1. Haga clic en **Save**(Guardar). Compruebe el panel de registros para asegurarse de que la función se guarda correctamente y no hay errores.


## <a name="create-a-function-to-list-images-from-cosmos-db"></a>Creación de una función para mostrar imágenes de Cosmos DB

La aplicación web requiere una API para recuperar metadatos de imagen de Cosmos DB. En los siguientes pasos, creará una función desencadenada mediante HTTP que usa un enlace de entrada de Cosmos DB para consultar la colección de base de datos.

1. En la aplicación de función, mantenga el mouse sobre **Funciones** a la izquierda y haga clic en **+** para crear una función.

1. Busque la plantilla **HttpTrigger** y selecciónela.

1. Use estos valores para crear una función que genere una dirección URL para obtener imágenes.

    | Configuración      |  Valor sugerido   | DESCRIPCIÓN                                        |
    | --- | --- | ---|
    | **Asigne un nombre a la función** | GetImages | Escriba este nombre tal y como se muestra para que la aplicación pueda detectar la función. |
    | **Nivel de autorización** | Anónimas | Permita que la función sea accesible públicamente. |

1. Haga clic en **Create**(Crear).

1. Cuando se cree la nueva función, haga clic en **Integrar** en el nombre de la función en el panel de navegación izquierdo.

1. Haga clic en **Nueva entrada** y seleccione **Azure Cosmos DB**. 

    ![Selección de la nueva entrada](media/functions-first-serverless-web-app/4-new-input.jpg)

1. Haga clic en **Seleccionar**.

1. Rellene los siguientes valores:

    | Configuración      |  Valor sugerido   | DESCRIPCIÓN                                        |
    | --- | --- | ---|
    | **Nombre del parámetro de documento** | documents | Busca la coincidencia del nombre del parámetro en la función. |
    | **Nombre de la base de datos** | imagesdb |  |
    | **Nombre de colección** | images |  |
    | **SQL query** | select * from c order by c._ts desc | Obtiene documentos, los más recientes primero. |
    | **Conexión de la cuenta de Azure Cosmos DB** | Seleccionar cadena de conexión existente |  |

1. Haga clic en **Guardar** para crear el enlace de entrada.

1. **C#**

    1. Haga clic en el nombre de la función para abrir la ventana de código y, a continuación, reemplace **run.csx** por el contenido de [**/csharp/GetImages/run.csx**](https://raw.githubusercontent.com/Azure-Samples/functions-first-serverless-web-application/master/csharp/GetImages/run.csx).

1. **JavaScript**

    1. Haga clic en el nombre de la función para abrir la ventana de código y, a continuación, reemplace **index.js** por el contenido de [**/javascript/GetImages/index.js**](https://raw.githubusercontent.com/Azure-Samples/functions-first-serverless-web-application/master/javascript/GetImages/index.js).

1. Haga clic en **Registros** debajo de la ventana de código para expandir el panel de registros.

1. Haga clic en **Save**(Guardar). Compruebe el panel de registros para asegurarse de que la función se guarda correctamente y no hay errores.


## <a name="test-the-application"></a>Prueba de la aplicación

1. Abra la aplicación en un explorador. Seleccione un archivo de imagen y cárguelo.

1. Al cabo de unos segundos, la miniatura de la nueva imagen aparece en la página.

1. En Azure Portal, use el cuadro de búsqueda para buscar la cuenta de Cosmos DB por su nombre. Haga clic en ella para abrirla.

1. Haga clic en **Explorador de datos** a la izquierda para examinar las colecciones y los documentos.

1. En la base de datos **imagesdb**, seleccione la colección **images**.

1. Confirme que se creó un documento para la imagen cargada.

    ![Explorador de datos que muestra un documento para una imagen cargada](media/functions-first-serverless-web-app/4-data-explorer.png)



## <a name="summary"></a>Resumen

En este módulo, aprendió a crear una cuenta, una base de datos y una colección de Cosmos DB. También aprendido a usar los enlaces de Cosmos DB para guardar y recuperar metadatos de imagen en la colección de Cosmos DB. A continuación, aprenderá a generar automáticamente un título para cada imagen cargada con Microsoft Cognitive Services.
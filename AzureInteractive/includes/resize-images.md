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
ms.openlocfilehash: d19a9d0e7e0347b38fc16f85fa440281be5347f2
ms.sourcegitcommit: 81587470a181e314242c7a97cd0f91c82d4fe232
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/01/2018
ms.locfileid: "47460096"
---
En el módulo anterior, vimos cómo una función sin servidor puede facilitar la carga segura de imágenes en Blob Storage desde una aplicación web. En este módulo, creará otra función sin servidor para inspeccionar las imágenes cargadas y crear miniaturas a partir de ellas.

## <a name="create-a-blob-storage-container-for-thumbnails"></a>Creación de un contenedor de almacenamiento de blobs para miniaturas

Las imágenes de tamaño completo se almacenan en un contenedor llamado **images**. Necesitará otro contenedor para almacenar las miniaturas de esas imágenes.

1. Asegúrese de que sigue conectado a Cloud Shell (Bash).  En caso contrario, seleccione **Enter focus mode** (Entrar en modo de enfoque) para abrir una ventana de Cloud Shell. 

1. Cree un contenedor llamado **thumbnails** en su cuenta de almacenamiento con acceso público a todos los blobs.

    ```azurecli
    az storage container create -n thumbnails --account-name <storage account name> --public-access blob
    ```


## <a name="create-a-blob-triggered-serverless-function"></a>Creación de una función sin servidor desencadenada mediante blobs

Un desencadenador define cómo se invoca una función. La función que se crea a continuación usa un desencadenador de blobs: la función se invoca automáticamente cuando se carga un blob (archivo de imagen) en el contenedor **images**. Una función debe tener un desencadenador. Los desencadenadores tienen datos asociados, que suelen ser la carga que desencadenó la función.

Los enlaces definen cómo una función lee o escribe datos en servicios de Azure o de terceros. Esta función crea una versión en miniatura de la imagen que la desencadena y guarda la miniatura en un contenedor *thumbnails*.

1. Abra la aplicación de función en Azure Portal.

1. En el panel de navegación izquierdo de la ventana de aplicación de función, mantenga el mouse sobre **Funciones** y haga clic en **+** para comenzar a crear una función sin servidor. Si aparece una página de inicio rápido, haga clic en **Función personalizada** para ver una lista de plantillas de función.

1. Busque la plantilla **BlobTrigger** y selecciónela.

1. Use estos valores para crear una función que crea miniaturas a medida que se cargan las imágenes.

    | Configuración      |  Valor sugerido   | DESCRIPCIÓN                                        |
    | --- | --- | ---|
    | **Lenguaje** | C# o JavaScript | Elija su lenguaje preferido. |
    | **Asigne un nombre a la función** | ResizeImage | Escriba este nombre tal y como se muestra para que la aplicación pueda detectar la función. |
    | **Path** | images/{name} | Ejecute la función cuando aparezca un archivo en el contenedor **images**. |
    | **Información de la cuenta de almacenamiento** | AZURE_STORAGE_CONNECTION_STRING | Use la variable de entorno creada anteriormente con la cadena de conexión. |

    ![Especificación de la configuración de la nueva función](media/functions-first-serverless-web-app/3-new-function.png)

1. Haga clic en **Crear** para crear la función.

1. Cuando se cree la función, haga clic en **Integrar** para ver sus enlaces de desencadenador, entrada y salida.

1. Haga clic en **Nueva salida** para crear un nuevo enlace de salida.

    ![Selección de Nueva salida en la pestaña Integrar](media/functions-first-serverless-web-app/3-new-output.jpg)

1. Seleccione **Azure Blob Storage** y haga clic en **Seleccionar**. Es posible que deba desplazarse para ver el botón **Seleccionar**.

    ![Seleccionar Azure Blob Storage](media/functions-first-serverless-web-app/3-storage-output.jpg)

1. Escriba los siguientes valores:

    | Configuración      |  Valor sugerido   | DESCRIPCIÓN                                        |
    | --- | --- | ---|
    | **Nombre de parámetro de blob** | thumbnail | La función usa el parámetro con este nombre para escribir la miniatura. |
    | **Usar el valor devuelto de la función** | Sin  |  |
    | **Path** | thumbnails/{name} | Las miniaturas se escriben en un contenedor llamado **thumbnails**. |
    | **Conexión de la cuenta de almacenamiento** | AZURE_STORAGE_CONNECTION_STRING | Use la variable de entorno creada anteriormente con la cadena de conexión. |

    ![Escribir la configuración del enlace de salida de blob](media/functions-first-serverless-web-app/3-blob-output.png)

1. **JavaScript**

    1. (JavaScript) Haga clic en **Editor avanzado** en la esquina superior derecha de la ventana para ver el código JSON que representa los enlaces.

    1. (JavaScript) En el enlace `blobTrigger`, agregue una propiedad llamada `dataType` con un valor de `binary`. De esta forma, se configura el enlace para pasar el contenido del blob a la función como datos binarios.

    ```json
    {
      "name": "myBlob",
      "type": "blobTrigger",
      "direction": "in",
      "path": "images/{name}",
      "connection": "AZURE_STORAGE_CONNECTION_STRING",
      "dataType": "binary"
    }
    ```

1. Haga clic en **Guardar** para crear el enlace.

1. **C#**

    1. (C#) Seleccione el nombre de función **ResizeImage** en el panel de navegación izquierdo para abrir el código fuente de la función.

    1. (C#) La función requiere un paquete NuGet llamado **ImageResizer** para generar las miniaturas. Los paquetes NuGet se agregan a las funciones de C# mediante un archivo **project.json**. Para crear el archivo, haga clic en **Ver archivos** a la derecha para mostrar los archivos que constituyen la función.
    
    1. (C#) Haga clic en **Agregar** para agregar un nuevo archivo llamado **project.json**.
    
    1. (C#) Copie el contenido de [**/csharp/ResizeImage/project.json**](https://raw.githubusercontent.com/Azure-Samples/functions-first-serverless-web-application/master/csharp/ResizeImage/project.json) en el archivo recién creado. Guarde el archivo. Los paquetes se restauran automáticamente cuando se actualiza el archivo.
    
        ![archivo project.json con ImageResizer](media/functions-first-serverless-web-app/3-project-json.png)
    
    1. (C#) Seleccione **run.csx** en **Ver archivos** y reemplace su contenido por el contenido de [**/csharp/ResizeImage/run.csx**](https://raw.githubusercontent.com/Azure-Samples/functions-first-serverless-web-application/master/csharp/ResizeImage/run.csx).

1. **JavaScript** 

    1. (JavaScript) Esta función requiere el paquete `jimp` de npm para cambiar el tamaño de la foto. Para instalar el paquete de npm, haga clic en el nombre de la aplicación de función en el panel de navegación izquierdo y haga clic en **Características de la plataforma**.

    1. (JavaScript) Haga clic en **Consola** para mostrar una ventana de consola.

    1. (JavaScript) Ejecute el comando `npm install jimp` en la consola. La operación puede tardar un minuto o dos en completarse.

    1. (JavaScript) Haga clic en el nombre de la función **ResizeImage** en el panel de navegación izquierdo para mostrar la función y remplace **index.js** por el contenido de [**/javascript/ResizeImage/index.js**](https://raw.githubusercontent.com/Azure-Samples/functions-first-serverless-web-application/master/javascript/ResizeImage/index.js).

1. Haga clic en **Registros** debajo de la ventana de código para expandir el panel de registros.

1. Haga clic en **Save**(Guardar). Compruebe el panel de registros para asegurarse de que la función se guarda correctamente y no hay errores.


## <a name="test-the-serverless-function"></a>Prueba de la función sin servidor

1. Abra la aplicación en un explorador. Seleccione un archivo de imagen y cárguelo. La carga finaliza, pero como aún no se ha agregado la capacidad para mostrar imágenes, la aplicación no muestra la foto cargada.

1. En Cloud Shell, confirme que se cargó la imagen en el contenedor **images**.

    ```azurecli
    az storage blob list --account-name <storage account name> -c images -o table
    ```

1. Confirme que se ha creado la miniatura en un contenedor llamado **thumbnails**.

    ```azurecli
    az storage blob list --account-name <storage account name> -c thumbnails -o table
    ```

1. Obtenga la dirección URL de la miniatura.

    ```azurecli
    az storage blob url --account-name <storage account name> -c thumbnails -n <filename> --output tsv
    ```

    Abra la dirección URL en un explorador y confirme que la miniatura se creó correctamente.

1. Antes de pasar al siguiente tutorial, elimine todos los archivos de los contenedores **images** y **thumbnails**.

    ```azurecli
    az storage blob delete-batch -s images --account-name <storage account name>
    ```
    ```azurecli
    az storage blob delete-batch -s thumbnails --account-name <storage account name>
    ```


## <a name="summary"></a>Resumen

En este módulo, creó una función sin servidor para crear una miniatura cada vez que se carga una imagen en un contenedor de almacenamiento de blobs. A continuación, aprenderá a usar Azure Cosmos DB para almacenar y mostrar metadatos de imagen.

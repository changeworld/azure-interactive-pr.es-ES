---
title: archivo de inclusión
description: archivo de inclusión
services: functions
author: ggailey777
manager: jeconnoc
ms.service: multiple
ms.topic: include
ms.date: 10/12/2018
ms.author: glenga
ms.custom: include file
ms.openlocfilehash: 3779c2e130afa7ee8d5879f30a924e258b7a41e9
ms.sourcegitcommit: fdb43556b8dcf67cb39c18e532b5fab7ac53eaee
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/15/2018
ms.locfileid: "49315983"
---
La aplicación que va a crear es una galería de fotos. Se usará JavaScript del lado cliente para llamar a API para cargar y mostrar imágenes. En este módulo, creará una API mediante una función sin servidor que genera una dirección URL de tiempo limitado para cargar una imagen. La aplicación web usa la dirección URL generada para cargar una imagen en Blob Storage mediante la [API REST de Blob Storage](https://docs.microsoft.com/rest/api/storageservices/blob-service-rest-api).

## <a name="create-a-blob-storage-container-for-images"></a>Creación de un contenedor de almacenamiento de blobs para imágenes

La aplicación requiere un contenedor de almacenamiento independiente para cargar y hospedar imágenes.

1. Asegúrese de que sigue conectado a Cloud Shell (Bash). En caso contrario, seleccione **Enter focus mode** (Entrar en modo de enfoque) para abrir una ventana de Cloud Shell.

1.  Cree un contenedor llamado **images** en su cuenta de almacenamiento con acceso público a todos los blobs.

    ```azurecli
    az storage container create -n images --account-name <storage account name> --public-access blob
    ```

## <a name="create-an-azure-function-app"></a>Creación de una Function App de Azure

Azure Functions es un servicio para ejecutar funciones sin servidor. Una función sin servidor se puede desencadenar (llamar) por medio de eventos como una solicitud HTTP o cuando se crea un blob en un contenedor de almacenamiento.

Una aplicación de función de Azure es un contenedor de una o más funciones sin servidor.

Cree una aplicación de función de Azure con un nombre único en el grupo de recursos que creó anteriormente llamado **first-serverless-app**. Las aplicaciones de función requieren una cuenta de Storage; en este tutorial, usará la cuenta de almacenamiento existente.

```azurecli
az functionapp create -n <function app name> -g first-serverless-app -s <storage account name> -c westcentralus
```

## <a name="configure-the-function-app"></a>Configuración de la Function App

La aplicación de función de este tutorial requiere la versión 1.x del entorno de ejecución de Functions. Si establece la configuración de la aplicación `FUNCTIONS_EXTENSION_VERSION` en `~1` anclará la aplicación de función a la última versión 1.x. Establezca la configuración de aplicación con el comando [az functionapp config appsettings set](https://docs.microsoft.com/cli/azure/functionapp/config/appsettings#set).

En el siguiente comando de la CLI de Azure, <app_name> es el nombre de la aplicación de función.

```azurecli
az functionapp config appsettings set --name <function app name> -g first-serverless-app --settings FUNCTIONS_EXTENSION_VERSION=~1
```

## <a name="create-an-http-triggered-serverless-function"></a>Creación de una función sin servidor desencadenada mediante HTTP

La aplicación web de galería de fotos realiza una solicitud HTTP a la función sin servidor para generar una dirección URL de tiempo limitado para cargar una imagen de forma segura en Blob Storage. La función se desencadena mediante una solicitud HTTP y usa el SDK de Azure Storage para generar y devolver la dirección URL segura.

1. Una vez creada la aplicación de función, búsquela en Azure Portal mediante el cuadro de búsqueda y haga clic en ella para abrirla.

    ![Apertura de la aplicación de función](media/functions-first-serverless-web-app/2-search-function-app.png)

1. En el panel de navegación izquierdo de la ventana de aplicación de función, mantenga el mouse sobre **Funciones** y haga clic en **+** para comenzar a crear una función sin servidor.

    ![Creación de una función](media/functions-first-serverless-web-app/2-new-function.png)

1. Haga clic en **Función personalizada** para ver una lista de plantillas de función.

1. Busque la plantilla **HttpTrigger** y haga clic en el lenguaje que usará (C# o JavaScript).

1. Use estos valores para crear una función que genere una dirección URL de carga de blobs.

    | Configuración      |  Valor sugerido   | DESCRIPCIÓN                                        |
    | --- | --- | ---|
    | **Lenguaje** | C# o JavaScript | Seleccione el lenguaje que quiera usar. |
    | **Asigne un nombre a la función** | GetUploadUrl | Escriba este nombre tal y como se muestra para que la aplicación pueda detectar la función. |
    | **Nivel de autorización** | Anónimas | Permita que la función sea accesible públicamente. |

    ![Especificación de la configuración de una nueva función desencadenada mediante HTTP](media/functions-first-serverless-web-app/2-new-function-httptrigger.png)

1. Haga clic en **Crear** para crear la función.

1. **C#** 

    1. Cuando aparezca el código fuente de la función, reemplace **run.csx** por el contenido de [**csharp/GetUploadUrl/run.csx**](https://raw.githubusercontent.com/Azure-Samples/functions-first-serverless-web-application/master/csharp/GetUploadUrl/run.csx).

1. **JavaScript** 

    1. (JavaScript) Esta función requiere el paquete `azure-storage` de npm para generar el token de firma de acceso compartido (SAS) necesario para crear la dirección URL segura. Para instalar el paquete de npm, haga clic en el nombre de la aplicación de función en el panel de navegación izquierdo y haga clic en **Características de la plataforma**.

    1. (JavaScript) Haga clic en **Consola** para mostrar una ventana de consola.

        ![Apertura de una ventana de consola](media/functions-first-serverless-web-app/2-open-console.jpg)

    1. (JavaScript) Asegúrese de que el directorio actual sea **d:\home\site\wwwroot** mediante la ejecución del comando `cd d:\home\site\wwwroot`.

    1. (JavaScript) Ejecute el comando `npm init -y` para crear un archivo **package.json** vacío.

    1. (JavaScript) Ejecute el comando `npm install --save azure-storage` en la consola para instalar el paquete y guárdelo en **package.json**. La operación puede tardar un minuto o dos en completarse.

    1. (JavaScript) Haga clic en el nombre de función (**GetUploadUrl**) en el panel izquierdo para mostrar la función, reemplace **index.js** por el contenido de [**javascript/GetUploadUrl/index.js**](https://raw.githubusercontent.com/Azure-Samples/functions-first-serverless-web-application/master/javascript/GetUploadUrl/index.js).
    
        ![index.js después de la actualización](media/functions-first-serverless-web-app/2-paste-js.jpg)

1. Haga clic en **Registros** debajo de la ventana de código para expandir el panel de registros.

1. Haga clic en **Save**(Guardar). Compruebe el panel de registros para asegurarse de que la función se compila correctamente.

La función genera lo que se conoce como una dirección URL de firma de acceso compartido (SAS) que se usa para cargar un archivo en Blob Storage. La dirección URL de SAS es válida durante un corto período de tiempo y solo permite que se cargue un archivo. Consulte la documentación de Blob Storage para más información sobre el [uso de firmas de acceso compartido](https://docs.microsoft.com/azure/storage/common/storage-dotnet-shared-access-signature-part-1).


## <a name="add-an-environment-variable-for-the-storage-connection-string"></a>Incorporación de una variable de entorno para la cadena de conexión de almacenamiento

La función que acaba de crear requiere una cadena de conexión para la cuenta de Storage para que pueda generar la dirección URL de SAS. En vez de codificar de forma rígida la cadena de conexión en el cuerpo de la función, se puede almacenar como una configuración de la aplicación. La configuración de la aplicación es accesible como variables de entorno por todas las funciones de la aplicación de función.

1. En Cloud Shell, consulte la cadena de conexión de la cuenta de almacenamiento y guárdela en una variable de Bash llamada **STORAGE_CONNECTION_STRING**.

    ```azurecli
    export STORAGE_CONNECTION_STRING=$(az storage account show-connection-string -n <storage account name> -g first-serverless-app --query "connectionString" --output tsv)
    ```

    Confirme que la variable está establecida correctamente.

    ```azurecli
    echo $STORAGE_CONNECTION_STRING
    ```

1. Cree una nueva configuración de aplicación llamada **AZURE_STORAGE_CONNECTION_STRING** con el valor guardado en el paso anterior.

    ```azurecli
    az functionapp config appsettings set -n <function app name> -g first-serverless-app --settings AZURE_STORAGE_CONNECTION_STRING=$STORAGE_CONNECTION_STRING -o table
    ```

    Confirme que la salida del comando contiene la nueva configuración de la aplicación con el valor correcto.


## <a name="test-the-serverless-function"></a>Prueba de la función sin servidor

Además de crear y editar funciones, en Azure Portal también se proporciona una herramienta integrada para probar funciones.

1. Para probar la función sin servidor HTTP, haga clic en la pestaña **Probar** a la derecha de la ventana de código para expandir el panel de prueba.

1. Cambie el **método HTTP** a **GET**.

1. En **Consulta**, haga clic en *Agregar parámetro** y agregue el siguiente parámetro:

    | Nombre      |  value   | 
    | --- | --- |
    | filename | image1.jpg |

1. Haga clic en **Ejecutar** en el panel de prueba para enviar una solicitud HTTP a la función.

1. La función devuelve una dirección URL de carga en la salida. La ejecución de la función aparece en el panel de registros.

    ![Registros que muestran que la función se ha ejecutado correctamente](media/functions-first-serverless-web-app/2-test-function.png)


## <a name="configure-cors-in-the-function-app"></a>Configuración de CORS en la aplicación de función

Como el front-end de la aplicación se hospeda en Blob Storage, tiene un nombre de dominio diferente al de la aplicación de función de Azure. Para que JavaScript en el lado cliente llame correctamente a la función que acaba de crear, la aplicación de función se debe configurar para el uso compartido de recursos entre orígenes (CORS).

1. En la barra de navegación izquierda de la ventana de aplicación de función, haga clic en el nombre de la aplicación de función.

1. Haga clic en **Características de la plataforma** para ver una lista de características avanzadas.

1. En **API**, haga clic en **CORS**.

    ![Selección de CORS](media/functions-first-serverless-web-app/2-open-cors.jpg)

1. Agregue un origen de permiso para la dirección URL de la aplicación desde el módulo anterior, y omita la terminación **/** (por ejemplo: `https://firstserverlessweb.z4.web.core.windows.net`).

    ![Configuración de CORS que muestra la dirección URL de aplicación web sin servidor agregada](media/functions-first-serverless-web-app/2-add-cors.png)

1. Haga clic en **Save**(Guardar).

1. C#

   1. (C#) Navegue de vuelta a la función `GetUploadUrl` y, luego, seleccione la pestaña **Integrar**.

   1. (C#) En los métodos HTTP seleccionados, seleccione **OPTIONS**.

      Se deben seleccionar todos: **GET**, **POST** y **OPTIONS**. CORS usa el método OPTIONS, que no se selecciona de forma predeterminada para las funciones de C#.  

   1. (C#) Haga clic en **Guardar**.

1. Todavía en el portal, navegue a la aplicación de función, seleccione la pestaña **Información general** y, luego, haga clic en **Reiniciar** para asegurarse de que los cambios de CORS surten efecto.

## <a name="configure-cors-in-the-storage-account"></a>Configuración de CORS en la cuenta de Storage

Dado que la aplicación también realiza llamadas JavaScript en el lado cliente a Blob Storage para cargar archivos, también es necesario configurar la cuenta de almacenamiento para CORS.

1. Ejecute el siguiente comando para permitir que todos los orígenes carguen archivos en la cuenta de almacenamiento.

    ```azurecli
    az storage cors add --methods OPTIONS PUT --origins '*' --exposed-headers '*' --allowed-headers '*' --services b --account-name <storage account name>
    ```


## <a name="modify-the-web-app-to-upload-images"></a>Modificación de la aplicación web para cargar imágenes

La aplicación web recupera la configuración de un archivo llamado **settings.js**. En los pasos siguientes, creará el archivo mediante Cloud Shell y luego establecerá `window.apiBaseUrl` en la dirección URL de la aplicación de función y `window.blobBaseUrl` en la dirección URL del punto de conexión de Azure Blob Storage.

1. En Cloud Shell, asegúrese de que el directorio actual sea la carpeta **www/dist**.

    ```azurecli
    cd ~/functions-first-serverless-web-application/www/dist
    ```

1. Consulte la dirección URL de la aplicación de función y almacénela en una variable de Bash llamada **FUNCTION_APP_URL**.

    ```azurecli
    export FUNCTION_APP_URL="https://"$(az functionapp show -n <function app name> -g first-serverless-app --query "defaultHostName" --output tsv)
    ```

    Confirme que la variable está correctamente establecida.

    ```azurecli
    echo $FUNCTION_APP_URL
    ```

1. Para establecer el URI base de las llamadas API a la aplicación de función, cree **settings.js** y agregue la dirección URL de la aplicación de función de la manera siguiente.

    `window.apiBaseUrl = 'https://fnapp@lab.GlobalLabInstanceId.azurewebsites.net'`

    Para realizar el cambio, puede ejecutar el siguiente comando o usar un editor de la línea de comandos como VIM.

    ```azurecli
    echo "window.apiBaseUrl = '$FUNCTION_APP_URL'" > settings.js
    ```

    Confirme que el archivo se ha escrito correctamente.

    ```azurecli
    cat settings.js
    ```

1. Consulte la dirección URL base de Blob Storage y almacénela en una variable de Bash llamada **BLOB_BASE_URL**.

    ```azurecli
    export BLOB_BASE_URL=$(az storage account show -n <storage account name> -g first-serverless-app --query primaryEndpoints.blob -o tsv | sed 's/\/$//')
    ```

    Confirme que la variable está correctamente establecida.

    ```azurecli
    echo $BLOB_BASE_URL
    ```

1. Para establecer el URI base de las llamadas API a la aplicación de función, anexe la dirección URL de almacenamiento como la siguiente línea de código a **settings.js**.

    `window.blobBaseUrl = 'https://mystorage.blob.core.windows.net'`

    Para realizar el cambio, puede ejecutar el siguiente comando o usar un editor de la línea de comandos como VIM.

    ```azurecli
    echo "window.blobBaseUrl = '$BLOB_BASE_URL'" >> settings.js
    ```

    Confirme que el archivo se ha escrito correctamente y que ahora contiene 2 líneas.

    ```azurecli
    cat settings.js
    ```

1. Cargue el archivo en Blob Storage.

    ```azurecli
    az storage blob upload -c \$web --account-name <storage account name> -f settings.js -n settings.js
    ```


## <a name="test-the-web-application"></a>Prueba de la aplicación web

En este momento, la aplicación de la galería puede cargar una imagen en Blob Storage, pero aún no puede mostrar imágenes. Esta aplicación intentará llamar a una función `GetImages` que no existe aún porque se crea en un módulo posterior. Esa llamada producirá error y la página web parecerá bloquearse en "Analizando...", pero la imagen que seleccione se cargará correctamente.

Para comprobar que una imagen se ha cargado correctamente, puede comprobar el contenido del contenedor **images** en Azure Portal.

1. En una ventana del explorador, vaya a la aplicación. Seleccione un archivo de imagen y cárguelo. La carga finaliza, pero como aún no se ha agregado la capacidad para mostrar imágenes, la aplicación no muestra la foto cargada. (La página web parece estar bloqueada en "Analizando imágenes...;" más adelante corregirá este error.)

1. En Cloud Shell, confirme que se cargó la imagen en el contenedor **images**.

    ```azurecli
    az storage blob list --account-name <storage account name> -c images -o table
    ```

1. Antes de pasar al siguiente tutorial, elimine todos los archivos del contenedor **images**.

    ```azurecli
    az storage blob delete-batch -s images --account-name <storage account name>
    ```


## <a name="summary"></a>Resumen

En este módulo, creó una aplicación de función de Azure y aprendió a usar una función sin servidor para permitir que una aplicación web cargara imágenes en Blob Storage. A continuación, aprenderá a crear miniaturas para las imágenes cargadas mediante una función sin servidor desencadenada por blobs.

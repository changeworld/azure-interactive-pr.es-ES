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
ms.openlocfilehash: 56cfb4c2893977086309660f4f6941fd0d648913
ms.sourcegitcommit: e721422a57e6deb95245135fd9f4f5677c344d93
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/26/2018
ms.locfileid: "40079694"
---
<span data-ttu-id="c451d-103">La aplicación que va a crear es una galería de fotos.</span><span class="sxs-lookup"><span data-stu-id="c451d-103">The application that you're building is a photo gallery.</span></span> <span data-ttu-id="c451d-104">Se usará JavaScript del lado cliente para llamar a API para cargar y mostrar imágenes.</span><span class="sxs-lookup"><span data-stu-id="c451d-104">It uses client-side JavaScript to call APIs to upload and display images.</span></span> <span data-ttu-id="c451d-105">En este módulo, creará una API mediante una función sin servidor que genera una dirección URL de tiempo limitado para cargar una imagen.</span><span class="sxs-lookup"><span data-stu-id="c451d-105">In this module, you create an API using a serverless function that generates a time-limited URL to upload an image.</span></span> <span data-ttu-id="c451d-106">La aplicación web usa la dirección URL generada para cargar una imagen en Blob Storage mediante la [API REST de Blob Storage](https://docs.microsoft.com/rest/api/storageservices/blob-service-rest-api).</span><span class="sxs-lookup"><span data-stu-id="c451d-106">The web application uses the generated URL to upload an image to Blob storage using the [Blob storage REST API](https://docs.microsoft.com/rest/api/storageservices/blob-service-rest-api).</span></span>

## <a name="create-a-blob-storage-container-for-images"></a><span data-ttu-id="c451d-107">Creación de un contenedor de almacenamiento de blobs para imágenes</span><span class="sxs-lookup"><span data-stu-id="c451d-107">Create a blob storage container for images</span></span>

<span data-ttu-id="c451d-108">La aplicación requiere un contenedor de almacenamiento independiente para cargar y hospedar imágenes.</span><span class="sxs-lookup"><span data-stu-id="c451d-108">The application requires a separate storage container to upload and host images.</span></span>

1. <span data-ttu-id="c451d-109">Asegúrese de que sigue conectado a Cloud Shell (Bash).</span><span class="sxs-lookup"><span data-stu-id="c451d-109">Ensure you are still signed in to the Cloud Shell (bash).</span></span> <span data-ttu-id="c451d-110">En caso contrario, seleccione **Enter focus mode** (Entrar en modo de enfoque) para abrir una ventana de Cloud Shell.</span><span class="sxs-lookup"><span data-stu-id="c451d-110">If not, select **Enter focus mode** to open a Cloud Shell window.</span></span>

1.  <span data-ttu-id="c451d-111">Cree un contenedor llamado **images** en su cuenta de almacenamiento con acceso público a todos los blobs.</span><span class="sxs-lookup"><span data-stu-id="c451d-111">Create a new container named **images** in your storage account with public access to all blobs.</span></span>

    ```azurecli
    az storage container create -n images --account-name <storage account name> --public-access blob
    ```

## <a name="create-an-azure-function-app"></a><span data-ttu-id="c451d-112">Creación de una Function App de Azure</span><span class="sxs-lookup"><span data-stu-id="c451d-112">Create an Azure Function app</span></span>

<span data-ttu-id="c451d-113">Azure Functions es un servicio para ejecutar funciones sin servidor.</span><span class="sxs-lookup"><span data-stu-id="c451d-113">Azure Functions is a service for running serverless functions.</span></span> <span data-ttu-id="c451d-114">Una función sin servidor se puede desencadenar (llamar) por medio de eventos como una solicitud HTTP o cuando se crea un blob en un contenedor de almacenamiento.</span><span class="sxs-lookup"><span data-stu-id="c451d-114">A serverless function can be triggered (called) by events such as an HTTP request or when a blob is created in a storage container.</span></span>

<span data-ttu-id="c451d-115">Una aplicación de función de Azure es un contenedor de una o más funciones sin servidor.</span><span class="sxs-lookup"><span data-stu-id="c451d-115">An Azure Function app is a container for one or more serverless functions.</span></span>

1. <span data-ttu-id="c451d-116">Cree una aplicación de función de Azure con un nombre único en el grupo de recursos que creó anteriormente llamado **first-serverless-app**.</span><span class="sxs-lookup"><span data-stu-id="c451d-116">Create a new Azure Function app with a unique name in the resource group you created earlier named **first-serverless-app**.</span></span> <span data-ttu-id="c451d-117">Las aplicaciones de función requieren una cuenta de Storage; en este tutorial, usará la cuenta de almacenamiento existente.</span><span class="sxs-lookup"><span data-stu-id="c451d-117">Function apps require a Storage account; in this tutorial, you use the existing storage account.</span></span>

    ```azurecli
    az functionapp create -n <function app name> -g first-serverless-app -s <storage account name> -c westcentralus
    ```


## <a name="create-an-http-triggered-serverless-function"></a><span data-ttu-id="c451d-118">Creación de una función sin servidor desencadenada mediante HTTP</span><span class="sxs-lookup"><span data-stu-id="c451d-118">Create an HTTP-triggered serverless function</span></span>

<span data-ttu-id="c451d-119">La aplicación web de galería de fotos realiza una solicitud HTTP a la función sin servidor para generar una dirección URL de tiempo limitado para cargar una imagen de forma segura en Blob Storage.</span><span class="sxs-lookup"><span data-stu-id="c451d-119">The photo gallery web application makes an HTTP request to serverless function to generate a time-limited URL to securely upload an image to Blob storage.</span></span> <span data-ttu-id="c451d-120">La función se desencadena mediante una solicitud HTTP y usa el SDK de Azure Storage para generar y devolver la dirección URL segura.</span><span class="sxs-lookup"><span data-stu-id="c451d-120">The function is triggered by an HTTP request and uses the Azure Storage SDK to generate and return the secure URL.</span></span>

1. <span data-ttu-id="c451d-121">Una vez creada la aplicación de función, búsquela en Azure Portal mediante el cuadro de búsqueda y haga clic en ella para abrirla.</span><span class="sxs-lookup"><span data-stu-id="c451d-121">After the Function app is created, search for it in the Azure Portal using the Search box and click to open it.</span></span>

    ![Apertura de la aplicación de función](media/functions-first-serverless-web-app/2-search-function-app.png)

1. <span data-ttu-id="c451d-123">En el panel de navegación izquierdo de la ventana de aplicación de función, mantenga el mouse sobre **Funciones** y haga clic en **+** para comenzar a crear una función sin servidor.</span><span class="sxs-lookup"><span data-stu-id="c451d-123">In the function app window's left hand navigation, hover over **Functions** and click **+** to start creating a new serverless function.</span></span>

    ![Creación de una función](media/functions-first-serverless-web-app/2-new-function.png)

1. <span data-ttu-id="c451d-125">Haga clic en **Función personalizada** para ver una lista de plantillas de función.</span><span class="sxs-lookup"><span data-stu-id="c451d-125">Click **Custom function** to see a list of function templates.</span></span>

1. <span data-ttu-id="c451d-126">Busque la plantilla **HttpTrigger** y haga clic en el lenguaje que usará (C# o JavaScript).</span><span class="sxs-lookup"><span data-stu-id="c451d-126">Find the **HttpTrigger** template and click the language to use (C# or JavaScript).</span></span>

1. <span data-ttu-id="c451d-127">Use estos valores para crear una función que genere una dirección URL de carga de blobs.</span><span class="sxs-lookup"><span data-stu-id="c451d-127">Use these values to create a function that generates a blob upload URL.</span></span>

    | <span data-ttu-id="c451d-128">Configuración</span><span class="sxs-lookup"><span data-stu-id="c451d-128">Setting</span></span>      |  <span data-ttu-id="c451d-129">Valor sugerido</span><span class="sxs-lookup"><span data-stu-id="c451d-129">Suggested value</span></span>   | <span data-ttu-id="c451d-130">DESCRIPCIÓN</span><span class="sxs-lookup"><span data-stu-id="c451d-130">Description</span></span>                                        |
    | --- | --- | ---|
    | <span data-ttu-id="c451d-131">**Lenguaje**</span><span class="sxs-lookup"><span data-stu-id="c451d-131">**Language**</span></span> | <span data-ttu-id="c451d-132">C# o JavaScript</span><span class="sxs-lookup"><span data-stu-id="c451d-132">C# or JavaScript</span></span> | <span data-ttu-id="c451d-133">Seleccione el lenguaje que quiera usar.</span><span class="sxs-lookup"><span data-stu-id="c451d-133">Select the language you want to use.</span></span> |
    | <span data-ttu-id="c451d-134">**Asigne un nombre a la función**</span><span class="sxs-lookup"><span data-stu-id="c451d-134">**Name your function**</span></span> | <span data-ttu-id="c451d-135">GetUploadUrl</span><span class="sxs-lookup"><span data-stu-id="c451d-135">GetUploadUrl</span></span> | <span data-ttu-id="c451d-136">Escriba este nombre tal y como se muestra para que la aplicación pueda detectar la función.</span><span class="sxs-lookup"><span data-stu-id="c451d-136">Type this name exactly as shown so the application can discover the function.</span></span> |
    | <span data-ttu-id="c451d-137">**Nivel de autorización**</span><span class="sxs-lookup"><span data-stu-id="c451d-137">**Authorization level**</span></span> | <span data-ttu-id="c451d-138">Anónimas</span><span class="sxs-lookup"><span data-stu-id="c451d-138">Anonymous</span></span> | <span data-ttu-id="c451d-139">Permita que la función sea accesible públicamente.</span><span class="sxs-lookup"><span data-stu-id="c451d-139">Allow the function to be accessed publicly.</span></span> |

    ![Especificación de la configuración de una nueva función desencadenada mediante HTTP](media/functions-first-serverless-web-app/2-new-function-httptrigger.png)

1. <span data-ttu-id="c451d-141">Haga clic en **Crear** para crear la función.</span><span class="sxs-lookup"><span data-stu-id="c451d-141">Click **Create** to create the function.</span></span>

1. <span data-ttu-id="c451d-142">**C#**</span><span class="sxs-lookup"><span data-stu-id="c451d-142">**C#**</span></span> 

    1. <span data-ttu-id="c451d-143">Cuando aparezca el código fuente de la función, reemplace **run.csx** por el contenido de [**csharp/GetUploadUrl/run.csx**](https://raw.githubusercontent.com/Azure-Samples/functions-first-serverless-web-application/master/csharp/GetUploadUrl/run.csx).</span><span class="sxs-lookup"><span data-stu-id="c451d-143">When the function's source code appears, replace all of **run.csx** with the content of [**csharp/GetUploadUrl/run.csx**](https://raw.githubusercontent.com/Azure-Samples/functions-first-serverless-web-application/master/csharp/GetUploadUrl/run.csx).</span></span>

1. <span data-ttu-id="c451d-144">**JavaScript**</span><span class="sxs-lookup"><span data-stu-id="c451d-144">**JavaScript**</span></span> 

    1. <span data-ttu-id="c451d-145">(JavaScript) Esta función requiere el paquete `azure-storage` de npm para generar el token de firma de acceso compartido (SAS) necesario para crear la dirección URL segura.</span><span class="sxs-lookup"><span data-stu-id="c451d-145">(JavaScript) This function requires the `azure-storage` package from npm to generate the shared access signature (SAS) token required to build the secure URL.</span></span> <span data-ttu-id="c451d-146">Para instalar el paquete de npm, haga clic en el nombre de la aplicación de función en el panel de navegación izquierdo y haga clic en **Características de la plataforma**.</span><span class="sxs-lookup"><span data-stu-id="c451d-146">To install the npm package, click on the Function App's name on the left navigation and click **Platform features**.</span></span>

    1. <span data-ttu-id="c451d-147">(JavaScript) Haga clic en **Consola** para mostrar una ventana de consola.</span><span class="sxs-lookup"><span data-stu-id="c451d-147">(JavaScript) Click **Console** to reveal a console window.</span></span>

        ![Apertura de una ventana de consola](media/functions-first-serverless-web-app/2-open-console.jpg)

    1. <span data-ttu-id="c451d-149">(JavaScript) Asegúrese de que el directorio actual sea **d:\home\site\wwwroot** mediante la ejecución del comando `cd d:\home\site\wwwroot`.</span><span class="sxs-lookup"><span data-stu-id="c451d-149">(JavaScript) Ensure the current directory is **d:\home\site\wwwroot** by running the command `cd d:\home\site\wwwroot`.</span></span>

    1. <span data-ttu-id="c451d-150">(JavaScript) Ejecute el comando `npm init -y` para crear un archivo **package.json** vacío.</span><span class="sxs-lookup"><span data-stu-id="c451d-150">(JavaScript) Run the command `npm init -y` to create an empty **package.json** file.</span></span>

    1. <span data-ttu-id="c451d-151">(JavaScript) Ejecute el comando `npm install --save azure-storage` en la consola para instalar el paquete y guárdelo en **package.json**.</span><span class="sxs-lookup"><span data-stu-id="c451d-151">(JavaScript) Run the command `npm install --save azure-storage` in the console to install the package and save it in **package.json**.</span></span> <span data-ttu-id="c451d-152">La operación puede tardar un minuto o dos en completarse.</span><span class="sxs-lookup"><span data-stu-id="c451d-152">It may take a minute or two to complete the operation.</span></span>

    1. <span data-ttu-id="c451d-153">(JavaScript) Haga clic en el nombre de función (**GetUploadUrl**) en el panel izquierdo para mostrar la función, reemplace **index.js** por el contenido de [**javascript/GetUploadUrl/index.js**](https://raw.githubusercontent.com/Azure-Samples/functions-first-serverless-web-application/master/javascript/GetUploadUrl/index.js).</span><span class="sxs-lookup"><span data-stu-id="c451d-153">(JavaScript) Click on the function name (**GetUploadUrl**) in the left navigation to reveal the function, replace all of **index.js** with the content of [**javascript/GetUploadUrl/index.js**](https://raw.githubusercontent.com/Azure-Samples/functions-first-serverless-web-application/master/javascript/GetUploadUrl/index.js).</span></span>
    
        ![index.js después de la actualización](media/functions-first-serverless-web-app/2-paste-js.jpg)

1. <span data-ttu-id="c451d-155">Haga clic en **Registros** debajo de la ventana de código para expandir el panel de registros.</span><span class="sxs-lookup"><span data-stu-id="c451d-155">Click **Logs** below the code window to expand the logs panel.</span></span>

1. <span data-ttu-id="c451d-156">Haga clic en **Save**(Guardar).</span><span class="sxs-lookup"><span data-stu-id="c451d-156">Click **Save**.</span></span> <span data-ttu-id="c451d-157">Compruebe el panel de registros para asegurarse de que la función se compila correctamente.</span><span class="sxs-lookup"><span data-stu-id="c451d-157">Check the logs panel to ensure the function is successfully compiled.</span></span>

<span data-ttu-id="c451d-158">La función genera lo que se conoce como una dirección URL de firma de acceso compartido (SAS) que se usa para cargar un archivo en Blob Storage.</span><span class="sxs-lookup"><span data-stu-id="c451d-158">The function generates what is called a shared access signature (SAS) URL that is used to upload a file to Blob storage.</span></span> <span data-ttu-id="c451d-159">La dirección URL de SAS es válida durante un corto período de tiempo y solo permite que se cargue un archivo.</span><span class="sxs-lookup"><span data-stu-id="c451d-159">The SAS URL is valid for a short period of time and only allows a single file to be uploaded.</span></span> <span data-ttu-id="c451d-160">Consulte la documentación de Blob Storage para más información sobre el [uso de firmas de acceso compartido](https://docs.microsoft.com/azure/storage/common/storage-dotnet-shared-access-signature-part-1).</span><span class="sxs-lookup"><span data-stu-id="c451d-160">Consult the Blob storage documentation for more information on [using shared access signatures](https://docs.microsoft.com/azure/storage/common/storage-dotnet-shared-access-signature-part-1).</span></span>


## <a name="add-an-environment-variable-for-the-storage-connection-string"></a><span data-ttu-id="c451d-161">Incorporación de una variable de entorno para la cadena de conexión de almacenamiento</span><span class="sxs-lookup"><span data-stu-id="c451d-161">Add an environment variable for the storage connection string</span></span>

<span data-ttu-id="c451d-162">La función que acaba de crear requiere una cadena de conexión para la cuenta de Storage para que pueda generar la dirección URL de SAS.</span><span class="sxs-lookup"><span data-stu-id="c451d-162">The function you just created requires a connection string for the Storage account so that it can generate the SAS URL.</span></span> <span data-ttu-id="c451d-163">En vez de codificar de forma rígida la cadena de conexión en el cuerpo de la función, se puede almacenar como una configuración de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="c451d-163">Instead of hardcoding the connection string in the function's body, it can be stored as an application setting.</span></span> <span data-ttu-id="c451d-164">La configuración de la aplicación es accesible como variables de entorno por todas las funciones de la aplicación de función.</span><span class="sxs-lookup"><span data-stu-id="c451d-164">Application settings are accessible as environment variables by all functions in the Function app.</span></span>

1. <span data-ttu-id="c451d-165">En Cloud Shell, consulte la cadena de conexión de la cuenta de almacenamiento y guárdela en una variable de Bash llamada **STORAGE_CONNECTION_STRING**.</span><span class="sxs-lookup"><span data-stu-id="c451d-165">In the Cloud Shell, query the Storage account connection string and save it to a bash variable named **STORAGE_CONNECTION_STRING**.</span></span>

    ```azurecli
    export STORAGE_CONNECTION_STRING=$(az storage account show-connection-string -n <storage account name> -g first-serverless-app --query "connectionString" --output tsv)
    ```

    <span data-ttu-id="c451d-166">Confirme que la variable está establecida correctamente.</span><span class="sxs-lookup"><span data-stu-id="c451d-166">Confirm the variable is set successfully.</span></span>

    ```azurecli
    echo $STORAGE_CONNECTION_STRING
    ```

1. <span data-ttu-id="c451d-167">Cree una nueva configuración de aplicación llamada **AZURE_STORAGE_CONNECTION_STRING** con el valor guardado en el paso anterior.</span><span class="sxs-lookup"><span data-stu-id="c451d-167">Create a new application setting named **AZURE_STORAGE_CONNECTION_STRING** using the value saved from the previous step.</span></span>

    ```azurecli
    az functionapp config appsettings set -n <function app name> -g first-serverless-app --settings AZURE_STORAGE_CONNECTION_STRING=$STORAGE_CONNECTION_STRING -o table
    ```

    <span data-ttu-id="c451d-168">Confirme que la salida del comando contiene la nueva configuración de la aplicación con el valor correcto.</span><span class="sxs-lookup"><span data-stu-id="c451d-168">Confirm that the command's output contains the new application setting with the correct value.</span></span>


## <a name="test-the-serverless-function"></a><span data-ttu-id="c451d-169">Prueba de la función sin servidor</span><span class="sxs-lookup"><span data-stu-id="c451d-169">Test the serverless function</span></span>

<span data-ttu-id="c451d-170">Además de crear y editar funciones, en Azure Portal también se proporciona una herramienta integrada para probar funciones.</span><span class="sxs-lookup"><span data-stu-id="c451d-170">In addition to creating and editing functions, the Azure portal also provides an built-in tool for testing functions.</span></span>

1. <span data-ttu-id="c451d-171">Para probar la función sin servidor HTTP, haga clic en la pestaña **Probar** a la derecha de la ventana de código para expandir el panel de prueba.</span><span class="sxs-lookup"><span data-stu-id="c451d-171">To test the HTTP serverless function, click on the **Test** tab on the right of the code window to expand the test panel.</span></span>

1. <span data-ttu-id="c451d-172">Cambie el **método HTTP** a **GET**.</span><span class="sxs-lookup"><span data-stu-id="c451d-172">Change the **Http method** to **GET**.</span></span>

1. <span data-ttu-id="c451d-173">En **Consulta**, haga clic en *Agregar parámetro*\* y agregue el siguiente parámetro:</span><span class="sxs-lookup"><span data-stu-id="c451d-173">Under **Query**, click *Add parameter*\* and add the following parameter:</span></span>

    | <span data-ttu-id="c451d-174">Nombre</span><span class="sxs-lookup"><span data-stu-id="c451d-174">name</span></span>      |  <span data-ttu-id="c451d-175">value</span><span class="sxs-lookup"><span data-stu-id="c451d-175">value</span></span>   | 
    | --- | --- |
    | <span data-ttu-id="c451d-176">filename</span><span class="sxs-lookup"><span data-stu-id="c451d-176">filename</span></span> | <span data-ttu-id="c451d-177">image1.jpg</span><span class="sxs-lookup"><span data-stu-id="c451d-177">image1.jpg</span></span> |

1. <span data-ttu-id="c451d-178">Haga clic en **Ejecutar** en el panel de prueba para enviar una solicitud HTTP a la función.</span><span class="sxs-lookup"><span data-stu-id="c451d-178">Click **Run** in the test panel to send an HTTP request to the function.</span></span>

1. <span data-ttu-id="c451d-179">La función devuelve una dirección URL de carga en la salida.</span><span class="sxs-lookup"><span data-stu-id="c451d-179">The function returns an upload URL in the output.</span></span> <span data-ttu-id="c451d-180">La ejecución de la función aparece en el panel de registros.</span><span class="sxs-lookup"><span data-stu-id="c451d-180">The function execution appears in the logs panel.</span></span>

    ![Registros que muestran que la función se ha ejecutado correctamente](media/functions-first-serverless-web-app/2-test-function.png)


## <a name="configure-cors-in-the-function-app"></a><span data-ttu-id="c451d-182">Configuración de CORS en la aplicación de función</span><span class="sxs-lookup"><span data-stu-id="c451d-182">Configure CORS in the function app</span></span>

<span data-ttu-id="c451d-183">Como el front-end de la aplicación se hospeda en Blob Storage, tiene un nombre de dominio diferente al de la aplicación de función de Azure.</span><span class="sxs-lookup"><span data-stu-id="c451d-183">Because the app's frontend is hosted in Blob storage, it has a different domain name than the Azure Function app.</span></span> <span data-ttu-id="c451d-184">Para que JavaScript en el lado cliente llame correctamente a la función que acaba de crear, la aplicación de función se debe configurar para el uso compartido de recursos entre orígenes (CORS).</span><span class="sxs-lookup"><span data-stu-id="c451d-184">For the client-side JavaScript to successfully call the function you just created, the function app needs to be configured for cross-origin resource sharing (CORS).</span></span>

1. <span data-ttu-id="c451d-185">En la barra de navegación izquierda de la ventana de aplicación de función, haga clic en el nombre de la aplicación de función.</span><span class="sxs-lookup"><span data-stu-id="c451d-185">In the left navigation bar of the function app window, click on the name of your function app.</span></span>

1. <span data-ttu-id="c451d-186">Haga clic en **Características de la plataforma** para ver una lista de características avanzadas.</span><span class="sxs-lookup"><span data-stu-id="c451d-186">Click on **Platform features** to view a list of advanced features.</span></span>

1. <span data-ttu-id="c451d-187">En **API**, haga clic en **CORS**.</span><span class="sxs-lookup"><span data-stu-id="c451d-187">Under **API**, click **CORS**.</span></span>

    ![Selección de CORS](media/functions-first-serverless-web-app/2-open-cors.jpg)

1. <span data-ttu-id="c451d-189">Agregue un origen de permiso para la dirección URL de la aplicación desde el módulo anterior, y omita la terminación **/** (por ejemplo: `https://firstserverlessweb.z4.web.core.windows.net`).</span><span class="sxs-lookup"><span data-stu-id="c451d-189">Add an allow origin for the application URL from the previous module, omitting the trailing **/** (for example: `https://firstserverlessweb.z4.web.core.windows.net`).</span></span>

    ![Configuración de CORS que muestra la dirección URL de aplicación web sin servidor agregada](media/functions-first-serverless-web-app/2-add-cors.png)

1. <span data-ttu-id="c451d-191">Haga clic en **Save**(Guardar).</span><span class="sxs-lookup"><span data-stu-id="c451d-191">Click **Save**.</span></span>

1. <span data-ttu-id="c451d-192">C#</span><span class="sxs-lookup"><span data-stu-id="c451d-192">C#</span></span>

   1. <span data-ttu-id="c451d-193">(C#) Navegue de vuelta a la función `GetUploadUrl` y, luego, seleccione la pestaña **Integrar**.</span><span class="sxs-lookup"><span data-stu-id="c451d-193">(C#) Navigate back to the `GetUploadUrl` function, and then select the **Integrate** tab.</span></span>

   1. <span data-ttu-id="c451d-194">(C#) En los métodos HTTP seleccionados, seleccione **OPTIONS**.</span><span class="sxs-lookup"><span data-stu-id="c451d-194">(C#) Under Selected HTTP methods, select **OPTIONS**.</span></span>

      <span data-ttu-id="c451d-195">Se deben seleccionar todos: **GET**, **POST** y **OPTIONS**.</span><span class="sxs-lookup"><span data-stu-id="c451d-195">**GET**, **POST**, and **OPTIONS** should all be selected.</span></span> <span data-ttu-id="c451d-196">CORS usa el método OPTIONS, que no se selecciona de forma predeterminada para las funciones de C#.</span><span class="sxs-lookup"><span data-stu-id="c451d-196">CORS uses the OPTIONS method, which is not selected by default for C# functions.</span></span>  

   1. <span data-ttu-id="c451d-197">(C#) Haga clic en **Guardar**.</span><span class="sxs-lookup"><span data-stu-id="c451d-197">(C#) Click **Save**.</span></span>

1. <span data-ttu-id="c451d-198">Todavía en el portal, navegue a la aplicación de función, seleccione la pestaña **Información general** y, luego, haga clic en **Reiniciar** para asegurarse de que los cambios de CORS surten efecto.</span><span class="sxs-lookup"><span data-stu-id="c451d-198">Still in the portal, navigate to the function app, select the **Overview** tab, and then click **Restart** to make sure that the changes for CORS take effect.</span></span>

## <a name="configure-cors-in-the-storage-account"></a><span data-ttu-id="c451d-199">Configuración de CORS en la cuenta de Storage</span><span class="sxs-lookup"><span data-stu-id="c451d-199">Configure CORS in the Storage account</span></span>

<span data-ttu-id="c451d-200">Dado que la aplicación también realiza llamadas JavaScript en el lado cliente a Blob Storage para cargar archivos, también es necesario configurar la cuenta de almacenamiento para CORS.</span><span class="sxs-lookup"><span data-stu-id="c451d-200">Because the app also makes client-side JavaScript calls to Blob Storage to upload files, you also have to configure the Storage account for CORS.</span></span>

1. <span data-ttu-id="c451d-201">Ejecute el siguiente comando para permitir que todos los orígenes carguen archivos en la cuenta de almacenamiento.</span><span class="sxs-lookup"><span data-stu-id="c451d-201">Run the following command to allow all origins to upload files to the Storage account.</span></span>

    ```azurecli
    az storage cors add --methods OPTIONS PUT --origins '*' --exposed-headers '*' --allowed-headers '*' --services b --account-name <storage account name>
    ```


## <a name="modify-the-web-app-to-upload-images"></a><span data-ttu-id="c451d-202">Modificación de la aplicación web para cargar imágenes</span><span class="sxs-lookup"><span data-stu-id="c451d-202">Modify the web app to upload images</span></span>

<span data-ttu-id="c451d-203">La aplicación web recupera la configuración de un archivo llamado **settings.js**.</span><span class="sxs-lookup"><span data-stu-id="c451d-203">The web app retrieves settings from a file named **settings.js**.</span></span> <span data-ttu-id="c451d-204">En los pasos siguientes, creará el archivo mediante Cloud Shell y luego establecerá `window.apiBaseUrl` en la dirección URL de la aplicación de función y `window.blobBaseUrl` en la dirección URL del punto de conexión de Azure Blob Storage.</span><span class="sxs-lookup"><span data-stu-id="c451d-204">In the following steps, you create the file using Cloud Shell, then set `window.apiBaseUrl` to the URL of the Function app and `window.blobBaseUrl` to the URL of the Azure Blob Storage endpoint.</span></span>

1. <span data-ttu-id="c451d-205">En Cloud Shell, asegúrese de que el directorio actual sea la carpeta **www/dist**.</span><span class="sxs-lookup"><span data-stu-id="c451d-205">In the Cloud Shell, ensure that the current directory is the **www/dist** folder.</span></span>

    ```azurecli
    cd ~/functions-first-serverless-web-application/www/dist
    ```

1. <span data-ttu-id="c451d-206">Consulte la dirección URL de la aplicación de función y almacénela en una variable de Bash llamada **FUNCTION_APP_URL**.</span><span class="sxs-lookup"><span data-stu-id="c451d-206">Query the function app's URL and store it in a bash variable named **FUNCTION_APP_URL**.</span></span>

    ```azurecli
    export FUNCTION_APP_URL="https://"$(az functionapp show -n <function app name> -g first-serverless-app --query "defaultHostName" --output tsv)
    ```

    <span data-ttu-id="c451d-207">Confirme que la variable está correctamente establecida.</span><span class="sxs-lookup"><span data-stu-id="c451d-207">Confirm the variable is correctly set.</span></span>

    ```azurecli
    echo $FUNCTION_APP_URL
    ```

1. <span data-ttu-id="c451d-208">Para establecer el URI base de las llamadas API a la aplicación de función, cree **settings.js** y agregue la dirección URL de la aplicación de función de la manera siguiente.</span><span class="sxs-lookup"><span data-stu-id="c451d-208">To set the base URI of API calls to your function app, create **settings.js** and add the function app URL like the following.</span></span>

    `window.apiBaseUrl = 'https://fnapp@lab.GlobalLabInstanceId.azurewebsites.net'`

    <span data-ttu-id="c451d-209">Para realizar el cambio, puede ejecutar el siguiente comando o usar un editor de la línea de comandos como VIM.</span><span class="sxs-lookup"><span data-stu-id="c451d-209">You can make the change by running the following command or by using a command-line editor like VIM.</span></span>

    ```azurecli
    echo "window.apiBaseUrl = '$FUNCTION_APP_URL'" > settings.js
    ```

    <span data-ttu-id="c451d-210">Confirme que el archivo se ha escrito correctamente.</span><span class="sxs-lookup"><span data-stu-id="c451d-210">Confirm the file was successfully written.</span></span>

    ```azurecli
    cat settings.js
    ```

1. <span data-ttu-id="c451d-211">Consulte la dirección URL base de Blob Storage y almacénela en una variable de Bash llamada **BLOB_BASE_URL**.</span><span class="sxs-lookup"><span data-stu-id="c451d-211">Query the Blob Storage base URL and store it in a bash variable named **BLOB_BASE_URL**.</span></span>

    ```azurecli
    export BLOB_BASE_URL=$(az storage account show -n <storage account name> -g first-serverless-app --query primaryEndpoints.blob -o tsv | sed 's/\/$//')
    ```

    <span data-ttu-id="c451d-212">Confirme que la variable está correctamente establecida.</span><span class="sxs-lookup"><span data-stu-id="c451d-212">Confirm the variable is correctly set.</span></span>

    ```azurecli
    echo $BLOB_BASE_URL
    ```

1. <span data-ttu-id="c451d-213">Para establecer el URI base de las llamadas API a la aplicación de función, anexe la dirección URL de almacenamiento como la siguiente línea de código a **settings.js**.</span><span class="sxs-lookup"><span data-stu-id="c451d-213">To set the base URI of API calls to your function app, append the storage URL like the following line of code to **settings.js**.</span></span>

    `window.blobBaseUrl = 'https://mystorage.blob.core.windows.net'`

    <span data-ttu-id="c451d-214">Para realizar el cambio, puede ejecutar el siguiente comando o usar un editor de la línea de comandos como VIM.</span><span class="sxs-lookup"><span data-stu-id="c451d-214">You can make the change by running the following command or by using a command-line editor like VIM.</span></span>

    ```azurecli
    echo "window.blobBaseUrl = '$BLOB_BASE_URL'" >> settings.js
    ```

    <span data-ttu-id="c451d-215">Confirme que el archivo se ha escrito correctamente y que ahora contiene 2 líneas.</span><span class="sxs-lookup"><span data-stu-id="c451d-215">Confirm the file was successfully written and it now contains 2 lines.</span></span>

    ```azurecli
    cat settings.js
    ```

1. <span data-ttu-id="c451d-216">Cargue el archivo en Blob Storage.</span><span class="sxs-lookup"><span data-stu-id="c451d-216">Upload the file to Blob storage.</span></span>

    ```azurecli
    az storage blob upload -c \$web --account-name <storage account name> -f settings.js -n settings.js
    ```


## <a name="test-the-web-application"></a><span data-ttu-id="c451d-217">Prueba de la aplicación web</span><span class="sxs-lookup"><span data-stu-id="c451d-217">Test the web application</span></span>

<span data-ttu-id="c451d-218">En este momento, la aplicación de la galería puede cargar una imagen en Blob Storage, pero aún no puede mostrar imágenes.</span><span class="sxs-lookup"><span data-stu-id="c451d-218">At this point, the gallery application is able to upload an image to Blob storage, but it can't display images yet.</span></span> <span data-ttu-id="c451d-219">Esta aplicación intentará llamar a una función `GetImages` que no existe aún porque se crea en un módulo posterior.</span><span class="sxs-lookup"><span data-stu-id="c451d-219">It will try to call a `GetImages` function that doesn't exist yet because you create it in a later module.</span></span> <span data-ttu-id="c451d-220">Esa llamada producirá error y la página web parecerá bloquearse en "Analizando...", pero la imagen que seleccione se cargará correctamente.</span><span class="sxs-lookup"><span data-stu-id="c451d-220">That call will fail, and the web page will appear to be stuck on "Analyzing...", but the image you select will be successfully uploaded.</span></span>

<span data-ttu-id="c451d-221">Para comprobar que una imagen se ha cargado correctamente, puede comprobar el contenido del contenedor **images** en Azure Portal.</span><span class="sxs-lookup"><span data-stu-id="c451d-221">You can verify an image is successfully uploaded by checking the contents of the **images** container in the Azure portal.</span></span>

1. <span data-ttu-id="c451d-222">En una ventana del explorador, vaya a la aplicación.</span><span class="sxs-lookup"><span data-stu-id="c451d-222">In a browser window, browse to the application.</span></span> <span data-ttu-id="c451d-223">Seleccione un archivo de imagen y cárguelo.</span><span class="sxs-lookup"><span data-stu-id="c451d-223">Select an image file and upload it.</span></span> <span data-ttu-id="c451d-224">La carga finaliza, pero como aún no se ha agregado la capacidad para mostrar imágenes, la aplicación no muestra la foto cargada.</span><span class="sxs-lookup"><span data-stu-id="c451d-224">The upload completes, but because we haven't added the ability to display images yet, the app doesn't show the uploaded photo.</span></span> <span data-ttu-id="c451d-225">(La página web parece estar bloqueada en "Analizando imágenes...;" más adelante corregirá este error.)</span><span class="sxs-lookup"><span data-stu-id="c451d-225">(The web page appears to be stuck on "Analyzing image..."; you'll fix that later.)</span></span>

1. <span data-ttu-id="c451d-226">En Cloud Shell, confirme que se cargó la imagen en el contenedor **images**.</span><span class="sxs-lookup"><span data-stu-id="c451d-226">In the Cloud Shell, confirm the image was uploaded to the **images** container.</span></span>

    ```azurecli
    az storage blob list --account-name <storage account name> -c images -o table
    ```

1. <span data-ttu-id="c451d-227">Antes de pasar al siguiente tutorial, elimine todos los archivos del contenedor **images**.</span><span class="sxs-lookup"><span data-stu-id="c451d-227">Before moving on to the next tutorial, delete all files in the **images** container.</span></span>

    ```azurecli
    az storage blob delete-batch -s images --account-name <storage account name>
    ```


## <a name="summary"></a><span data-ttu-id="c451d-228">Resumen</span><span class="sxs-lookup"><span data-stu-id="c451d-228">Summary</span></span>

<span data-ttu-id="c451d-229">En este módulo, creó una aplicación de función de Azure y aprendió a usar una función sin servidor para permitir que una aplicación web cargara imágenes en Blob Storage.</span><span class="sxs-lookup"><span data-stu-id="c451d-229">In this module, you created an Azure Function app and learned how to use a serverless function to allow a web application to upload images to Blob storage.</span></span> <span data-ttu-id="c451d-230">A continuación, aprenderá a crear miniaturas para las imágenes cargadas mediante una función sin servidor desencadenada por blobs.</span><span class="sxs-lookup"><span data-stu-id="c451d-230">Next, you learn how to create thumbnails for the uploaded images using a Blob triggered serverless function.</span></span>

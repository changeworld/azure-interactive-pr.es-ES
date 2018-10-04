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
<span data-ttu-id="6359b-103">En el módulo anterior, vimos cómo una función sin servidor puede facilitar la carga segura de imágenes en Blob Storage desde una aplicación web.</span><span class="sxs-lookup"><span data-stu-id="6359b-103">In the previous module, you saw how a serverless function can facilitate the secure uploading of images to Blob storage from a web application.</span></span> <span data-ttu-id="6359b-104">En este módulo, creará otra función sin servidor para inspeccionar las imágenes cargadas y crear miniaturas a partir de ellas.</span><span class="sxs-lookup"><span data-stu-id="6359b-104">In this module, you create another serverless function to watch for uploaded images and create thumbnails from them.</span></span>

## <a name="create-a-blob-storage-container-for-thumbnails"></a><span data-ttu-id="6359b-105">Creación de un contenedor de almacenamiento de blobs para miniaturas</span><span class="sxs-lookup"><span data-stu-id="6359b-105">Create a blob storage container for thumbnails</span></span>

<span data-ttu-id="6359b-106">Las imágenes de tamaño completo se almacenan en un contenedor llamado **images**.</span><span class="sxs-lookup"><span data-stu-id="6359b-106">The full size images are stored in a container named **images**.</span></span> <span data-ttu-id="6359b-107">Necesitará otro contenedor para almacenar las miniaturas de esas imágenes.</span><span class="sxs-lookup"><span data-stu-id="6359b-107">You need another container to store thumbnails of those images.</span></span>

1. <span data-ttu-id="6359b-108">Asegúrese de que sigue conectado a Cloud Shell (Bash).</span><span class="sxs-lookup"><span data-stu-id="6359b-108">Ensure you are still signed in to the Cloud Shell (bash).</span></span>  <span data-ttu-id="6359b-109">En caso contrario, seleccione **Enter focus mode** (Entrar en modo de enfoque) para abrir una ventana de Cloud Shell.</span><span class="sxs-lookup"><span data-stu-id="6359b-109">If not, select **Enter focus mode** to open a Cloud Shell window.</span></span> 

1. <span data-ttu-id="6359b-110">Cree un contenedor llamado **thumbnails** en su cuenta de almacenamiento con acceso público a todos los blobs.</span><span class="sxs-lookup"><span data-stu-id="6359b-110">Create a new container named **thumbnails** in your storage account with public access to all blobs.</span></span>

    ```azurecli
    az storage container create -n thumbnails --account-name <storage account name> --public-access blob
    ```


## <a name="create-a-blob-triggered-serverless-function"></a><span data-ttu-id="6359b-111">Creación de una función sin servidor desencadenada mediante blobs</span><span class="sxs-lookup"><span data-stu-id="6359b-111">Create a blob-triggered serverless function</span></span>

<span data-ttu-id="6359b-112">Un desencadenador define cómo se invoca una función.</span><span class="sxs-lookup"><span data-stu-id="6359b-112">A trigger defines how a function is invoked.</span></span> <span data-ttu-id="6359b-113">La función que se crea a continuación usa un desencadenador de blobs: la función se invoca automáticamente cuando se carga un blob (archivo de imagen) en el contenedor **images**.</span><span class="sxs-lookup"><span data-stu-id="6359b-113">The function you create next uses a blob trigger: the function is automatically invoked when a blob (image file) is uploaded to the **images** container.</span></span> <span data-ttu-id="6359b-114">Una función debe tener un desencadenador.</span><span class="sxs-lookup"><span data-stu-id="6359b-114">A function must have one trigger.</span></span> <span data-ttu-id="6359b-115">Los desencadenadores tienen datos asociados, que suelen ser la carga que desencadenó la función.</span><span class="sxs-lookup"><span data-stu-id="6359b-115">Triggers have associated data, which is usually the payload that triggered the function.</span></span>

<span data-ttu-id="6359b-116">Los enlaces definen cómo una función lee o escribe datos en servicios de Azure o de terceros.</span><span class="sxs-lookup"><span data-stu-id="6359b-116">Bindings define how a function reads or writes data in Azure or third-party services.</span></span> <span data-ttu-id="6359b-117">Esta función crea una versión en miniatura de la imagen que la desencadena y guarda la miniatura en un contenedor *thumbnails*.</span><span class="sxs-lookup"><span data-stu-id="6359b-117">This function creates a thumbnail version of the image that triggers it and saves the thumbnail in a *thumbnails* container.</span></span>

1. <span data-ttu-id="6359b-118">Abra la aplicación de función en Azure Portal.</span><span class="sxs-lookup"><span data-stu-id="6359b-118">Open your function app in the Azure Portal.</span></span>

1. <span data-ttu-id="6359b-119">En el panel de navegación izquierdo de la ventana de aplicación de función, mantenga el mouse sobre **Funciones** y haga clic en **+** para comenzar a crear una función sin servidor.</span><span class="sxs-lookup"><span data-stu-id="6359b-119">In the function app window's left hand navigation, hover over **Functions** and click **+** to start creating a new serverless function.</span></span> <span data-ttu-id="6359b-120">Si aparece una página de inicio rápido, haga clic en **Función personalizada** para ver una lista de plantillas de función.</span><span class="sxs-lookup"><span data-stu-id="6359b-120">If a quickstart page appears, click **Custom function** to see a list of function templates.</span></span>

1. <span data-ttu-id="6359b-121">Busque la plantilla **BlobTrigger** y selecciónela.</span><span class="sxs-lookup"><span data-stu-id="6359b-121">Find the **BlobTrigger** template and select it.</span></span>

1. <span data-ttu-id="6359b-122">Use estos valores para crear una función que crea miniaturas a medida que se cargan las imágenes.</span><span class="sxs-lookup"><span data-stu-id="6359b-122">Use these values to create a function that creates thumbnails as images are uploaded.</span></span>

    | <span data-ttu-id="6359b-123">Configuración</span><span class="sxs-lookup"><span data-stu-id="6359b-123">Setting</span></span>      |  <span data-ttu-id="6359b-124">Valor sugerido</span><span class="sxs-lookup"><span data-stu-id="6359b-124">Suggested value</span></span>   | <span data-ttu-id="6359b-125">DESCRIPCIÓN</span><span class="sxs-lookup"><span data-stu-id="6359b-125">Description</span></span>                                        |
    | --- | --- | ---|
    | <span data-ttu-id="6359b-126">**Lenguaje**</span><span class="sxs-lookup"><span data-stu-id="6359b-126">**Language**</span></span> | <span data-ttu-id="6359b-127">C# o JavaScript</span><span class="sxs-lookup"><span data-stu-id="6359b-127">C# or JavaScript</span></span> | <span data-ttu-id="6359b-128">Elija su lenguaje preferido.</span><span class="sxs-lookup"><span data-stu-id="6359b-128">Choose your preferred language.</span></span> |
    | <span data-ttu-id="6359b-129">**Asigne un nombre a la función**</span><span class="sxs-lookup"><span data-stu-id="6359b-129">**Name your function**</span></span> | <span data-ttu-id="6359b-130">ResizeImage</span><span class="sxs-lookup"><span data-stu-id="6359b-130">ResizeImage</span></span> | <span data-ttu-id="6359b-131">Escriba este nombre tal y como se muestra para que la aplicación pueda detectar la función.</span><span class="sxs-lookup"><span data-stu-id="6359b-131">Type this name exactly as shown so the application can discover the function.</span></span> |
    | <span data-ttu-id="6359b-132">**Path**</span><span class="sxs-lookup"><span data-stu-id="6359b-132">**Path**</span></span> | <span data-ttu-id="6359b-133">images/{name}</span><span class="sxs-lookup"><span data-stu-id="6359b-133">images/{name}</span></span> | <span data-ttu-id="6359b-134">Ejecute la función cuando aparezca un archivo en el contenedor **images**.</span><span class="sxs-lookup"><span data-stu-id="6359b-134">Execute the function when a file appears in the **images** container.</span></span> |
    | <span data-ttu-id="6359b-135">**Información de la cuenta de almacenamiento**</span><span class="sxs-lookup"><span data-stu-id="6359b-135">**Storage account information**</span></span> | <span data-ttu-id="6359b-136">AZURE_STORAGE_CONNECTION_STRING</span><span class="sxs-lookup"><span data-stu-id="6359b-136">AZURE_STORAGE_CONNECTION_STRING</span></span> | <span data-ttu-id="6359b-137">Use la variable de entorno creada anteriormente con la cadena de conexión.</span><span class="sxs-lookup"><span data-stu-id="6359b-137">Use the environment variable previously created with the connection string.</span></span> |

    ![Especificación de la configuración de la nueva función](media/functions-first-serverless-web-app/3-new-function.png)

1. <span data-ttu-id="6359b-139">Haga clic en **Crear** para crear la función.</span><span class="sxs-lookup"><span data-stu-id="6359b-139">Click **Create** to create the function.</span></span>

1. <span data-ttu-id="6359b-140">Cuando se cree la función, haga clic en **Integrar** para ver sus enlaces de desencadenador, entrada y salida.</span><span class="sxs-lookup"><span data-stu-id="6359b-140">When the function is created, click **Integrate** to view its trigger, input, and output bindings.</span></span>

1. <span data-ttu-id="6359b-141">Haga clic en **Nueva salida** para crear un nuevo enlace de salida.</span><span class="sxs-lookup"><span data-stu-id="6359b-141">Click **New output** to create a new output binding.</span></span>

    ![Selección de Nueva salida en la pestaña Integrar](media/functions-first-serverless-web-app/3-new-output.jpg)

1. <span data-ttu-id="6359b-143">Seleccione **Azure Blob Storage** y haga clic en **Seleccionar**.</span><span class="sxs-lookup"><span data-stu-id="6359b-143">Select **Azure Blob Storage** and click **Select**.</span></span> <span data-ttu-id="6359b-144">Es posible que deba desplazarse para ver el botón **Seleccionar**.</span><span class="sxs-lookup"><span data-stu-id="6359b-144">You may have to scroll down to see the **Select** button.</span></span>

    ![Seleccionar Azure Blob Storage](media/functions-first-serverless-web-app/3-storage-output.jpg)

1. <span data-ttu-id="6359b-146">Escriba los siguientes valores:</span><span class="sxs-lookup"><span data-stu-id="6359b-146">Enter the following values.</span></span>

    | <span data-ttu-id="6359b-147">Configuración</span><span class="sxs-lookup"><span data-stu-id="6359b-147">Setting</span></span>      |  <span data-ttu-id="6359b-148">Valor sugerido</span><span class="sxs-lookup"><span data-stu-id="6359b-148">Suggested value</span></span>   | <span data-ttu-id="6359b-149">DESCRIPCIÓN</span><span class="sxs-lookup"><span data-stu-id="6359b-149">Description</span></span>                                        |
    | --- | --- | ---|
    | <span data-ttu-id="6359b-150">**Nombre de parámetro de blob**</span><span class="sxs-lookup"><span data-stu-id="6359b-150">**Blob parameter name**</span></span> | <span data-ttu-id="6359b-151">thumbnail</span><span class="sxs-lookup"><span data-stu-id="6359b-151">thumbnail</span></span> | <span data-ttu-id="6359b-152">La función usa el parámetro con este nombre para escribir la miniatura.</span><span class="sxs-lookup"><span data-stu-id="6359b-152">The function uses the parameter with this name to write the thumbnail.</span></span> |
    | <span data-ttu-id="6359b-153">**Usar el valor devuelto de la función**</span><span class="sxs-lookup"><span data-stu-id="6359b-153">**Use function return value**</span></span> | <span data-ttu-id="6359b-154">Sin </span><span class="sxs-lookup"><span data-stu-id="6359b-154">No</span></span> |  |
    | <span data-ttu-id="6359b-155">**Path**</span><span class="sxs-lookup"><span data-stu-id="6359b-155">**Path**</span></span> | <span data-ttu-id="6359b-156">thumbnails/{name}</span><span class="sxs-lookup"><span data-stu-id="6359b-156">thumbnails/{name}</span></span> | <span data-ttu-id="6359b-157">Las miniaturas se escriben en un contenedor llamado **thumbnails**.</span><span class="sxs-lookup"><span data-stu-id="6359b-157">The thumbnails are written to a container named **thumbnails**.</span></span> |
    | <span data-ttu-id="6359b-158">**Conexión de la cuenta de almacenamiento**</span><span class="sxs-lookup"><span data-stu-id="6359b-158">**Storage account connection**</span></span> | <span data-ttu-id="6359b-159">AZURE_STORAGE_CONNECTION_STRING</span><span class="sxs-lookup"><span data-stu-id="6359b-159">AZURE_STORAGE_CONNECTION_STRING</span></span> | <span data-ttu-id="6359b-160">Use la variable de entorno creada anteriormente con la cadena de conexión.</span><span class="sxs-lookup"><span data-stu-id="6359b-160">Use the environment variable previously created with the connection string.</span></span> |

    ![Escribir la configuración del enlace de salida de blob](media/functions-first-serverless-web-app/3-blob-output.png)

1. <span data-ttu-id="6359b-162">**JavaScript**</span><span class="sxs-lookup"><span data-stu-id="6359b-162">**JavaScript**</span></span>

    1. <span data-ttu-id="6359b-163">(JavaScript) Haga clic en **Editor avanzado** en la esquina superior derecha de la ventana para ver el código JSON que representa los enlaces.</span><span class="sxs-lookup"><span data-stu-id="6359b-163">(JavaScript) Click on **Advanced editor** in the top right corner of the window to reveal the JSON representing the bindings.</span></span>

    1. <span data-ttu-id="6359b-164">(JavaScript) En el enlace `blobTrigger`, agregue una propiedad llamada `dataType` con un valor de `binary`.</span><span class="sxs-lookup"><span data-stu-id="6359b-164">(JavaScript) In the `blobTrigger` binding, add a property named `dataType` with a value of `binary`.</span></span> <span data-ttu-id="6359b-165">De esta forma, se configura el enlace para pasar el contenido del blob a la función como datos binarios.</span><span class="sxs-lookup"><span data-stu-id="6359b-165">This configures the binding to pass the blob contents to the function as binary data.</span></span>

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

1. <span data-ttu-id="6359b-166">Haga clic en **Guardar** para crear el enlace.</span><span class="sxs-lookup"><span data-stu-id="6359b-166">Click **Save** to create the new binding.</span></span>

1. <span data-ttu-id="6359b-167">**C#**</span><span class="sxs-lookup"><span data-stu-id="6359b-167">**C#**</span></span>

    1. <span data-ttu-id="6359b-168">(C#) Seleccione el nombre de función **ResizeImage** en el panel de navegación izquierdo para abrir el código fuente de la función.</span><span class="sxs-lookup"><span data-stu-id="6359b-168">(C#) Select the **ResizeImage** function name on the left navigation to open the function's source code.</span></span>

    1. <span data-ttu-id="6359b-169">(C#) La función requiere un paquete NuGet llamado **ImageResizer** para generar las miniaturas.</span><span class="sxs-lookup"><span data-stu-id="6359b-169">(C#) The function requires a NuGet package called **ImageResizer** to generate the thumbnails.</span></span> <span data-ttu-id="6359b-170">Los paquetes NuGet se agregan a las funciones de C# mediante un archivo **project.json**.</span><span class="sxs-lookup"><span data-stu-id="6359b-170">NuGet packages are added to C# functions using a **project.json** file.</span></span> <span data-ttu-id="6359b-171">Para crear el archivo, haga clic en **Ver archivos** a la derecha para mostrar los archivos que constituyen la función.</span><span class="sxs-lookup"><span data-stu-id="6359b-171">To create the file, click **View Files** on the right to reveal the files that make up the function.</span></span>
    
    1. <span data-ttu-id="6359b-172">(C#) Haga clic en **Agregar** para agregar un nuevo archivo llamado **project.json**.</span><span class="sxs-lookup"><span data-stu-id="6359b-172">(C#) Click **Add** to add a new file named **project.json**.</span></span>
    
    1. <span data-ttu-id="6359b-173">(C#) Copie el contenido de [**/csharp/ResizeImage/project.json**](https://raw.githubusercontent.com/Azure-Samples/functions-first-serverless-web-application/master/csharp/ResizeImage/project.json) en el archivo recién creado.</span><span class="sxs-lookup"><span data-stu-id="6359b-173">(C#) Copy the contents of [**/csharp/ResizeImage/project.json**](https://raw.githubusercontent.com/Azure-Samples/functions-first-serverless-web-application/master/csharp/ResizeImage/project.json) into the newly created file.</span></span> <span data-ttu-id="6359b-174">Guarde el archivo.</span><span class="sxs-lookup"><span data-stu-id="6359b-174">Save the file.</span></span> <span data-ttu-id="6359b-175">Los paquetes se restauran automáticamente cuando se actualiza el archivo.</span><span class="sxs-lookup"><span data-stu-id="6359b-175">Packages are automatically restored when the file is updated.</span></span>
    
        ![archivo project.json con ImageResizer](media/functions-first-serverless-web-app/3-project-json.png)
    
    1. <span data-ttu-id="6359b-177">(C#) Seleccione **run.csx** en **Ver archivos** y reemplace su contenido por el contenido de [**/csharp/ResizeImage/run.csx**](https://raw.githubusercontent.com/Azure-Samples/functions-first-serverless-web-application/master/csharp/ResizeImage/run.csx).</span><span class="sxs-lookup"><span data-stu-id="6359b-177">(C#) Select **run.csx** under **View Files** and replace its content with the content in [**/csharp/ResizeImage/run.csx**](https://raw.githubusercontent.com/Azure-Samples/functions-first-serverless-web-application/master/csharp/ResizeImage/run.csx).</span></span>

1. <span data-ttu-id="6359b-178">**JavaScript**</span><span class="sxs-lookup"><span data-stu-id="6359b-178">**JavaScript**</span></span> 

    1. <span data-ttu-id="6359b-179">(JavaScript) Esta función requiere el paquete `jimp` de npm para cambiar el tamaño de la foto.</span><span class="sxs-lookup"><span data-stu-id="6359b-179">(JavaScript) This function requires the `jimp` package from npm to resize the photo.</span></span> <span data-ttu-id="6359b-180">Para instalar el paquete de npm, haga clic en el nombre de la aplicación de función en el panel de navegación izquierdo y haga clic en **Características de la plataforma**.</span><span class="sxs-lookup"><span data-stu-id="6359b-180">To install the npm package, click on the Function App's name on the left navigation and click **Platform features**.</span></span>

    1. <span data-ttu-id="6359b-181">(JavaScript) Haga clic en **Consola** para mostrar una ventana de consola.</span><span class="sxs-lookup"><span data-stu-id="6359b-181">(JavaScript) Click **Console** to reveal a console window.</span></span>

    1. <span data-ttu-id="6359b-182">(JavaScript) Ejecute el comando `npm install jimp` en la consola.</span><span class="sxs-lookup"><span data-stu-id="6359b-182">(JavaScript) Run the command `npm install jimp` in the console.</span></span> <span data-ttu-id="6359b-183">La operación puede tardar un minuto o dos en completarse.</span><span class="sxs-lookup"><span data-stu-id="6359b-183">It may take a minute or two to complete the operation.</span></span>

    1. <span data-ttu-id="6359b-184">(JavaScript) Haga clic en el nombre de la función **ResizeImage** en el panel de navegación izquierdo para mostrar la función y remplace **index.js** por el contenido de [**/javascript/ResizeImage/index.js**](https://raw.githubusercontent.com/Azure-Samples/functions-first-serverless-web-application/master/javascript/ResizeImage/index.js).</span><span class="sxs-lookup"><span data-stu-id="6359b-184">(JavaScript) Click on the **ResizeImage** function name in the left navigation to reveal the function, replace all of **index.js** with the content of [**/javascript/ResizeImage/index.js**](https://raw.githubusercontent.com/Azure-Samples/functions-first-serverless-web-application/master/javascript/ResizeImage/index.js).</span></span>

1. <span data-ttu-id="6359b-185">Haga clic en **Registros** debajo de la ventana de código para expandir el panel de registros.</span><span class="sxs-lookup"><span data-stu-id="6359b-185">Click **Logs** below the code window to expand the logs panel.</span></span>

1. <span data-ttu-id="6359b-186">Haga clic en **Save**(Guardar).</span><span class="sxs-lookup"><span data-stu-id="6359b-186">Click **Save**.</span></span> <span data-ttu-id="6359b-187">Compruebe el panel de registros para asegurarse de que la función se guarda correctamente y no hay errores.</span><span class="sxs-lookup"><span data-stu-id="6359b-187">Check the logs panel to ensure the function is successfully saved and there are no errors.</span></span>


## <a name="test-the-serverless-function"></a><span data-ttu-id="6359b-188">Prueba de la función sin servidor</span><span class="sxs-lookup"><span data-stu-id="6359b-188">Test the serverless function</span></span>

1. <span data-ttu-id="6359b-189">Abra la aplicación en un explorador.</span><span class="sxs-lookup"><span data-stu-id="6359b-189">Open the application in a browser.</span></span> <span data-ttu-id="6359b-190">Seleccione un archivo de imagen y cárguelo.</span><span class="sxs-lookup"><span data-stu-id="6359b-190">Select an image file and upload it.</span></span> <span data-ttu-id="6359b-191">La carga finaliza, pero como aún no se ha agregado la capacidad para mostrar imágenes, la aplicación no muestra la foto cargada.</span><span class="sxs-lookup"><span data-stu-id="6359b-191">The upload completes, but because we haven't added the ability to display images yet, the app doesn't show the uploaded photo.</span></span>

1. <span data-ttu-id="6359b-192">En Cloud Shell, confirme que se cargó la imagen en el contenedor **images**.</span><span class="sxs-lookup"><span data-stu-id="6359b-192">In the Cloud Shell, confirm the image was uploaded to the **images** container.</span></span>

    ```azurecli
    az storage blob list --account-name <storage account name> -c images -o table
    ```

1. <span data-ttu-id="6359b-193">Confirme que se ha creado la miniatura en un contenedor llamado **thumbnails**.</span><span class="sxs-lookup"><span data-stu-id="6359b-193">Confirm the thumbnail was created in a container named **thumbnails**.</span></span>

    ```azurecli
    az storage blob list --account-name <storage account name> -c thumbnails -o table
    ```

1. <span data-ttu-id="6359b-194">Obtenga la dirección URL de la miniatura.</span><span class="sxs-lookup"><span data-stu-id="6359b-194">Obtain the thumbnail's URL.</span></span>

    ```azurecli
    az storage blob url --account-name <storage account name> -c thumbnails -n <filename> --output tsv
    ```

    <span data-ttu-id="6359b-195">Abra la dirección URL en un explorador y confirme que la miniatura se creó correctamente.</span><span class="sxs-lookup"><span data-stu-id="6359b-195">Open the URL in a browser and confirm the thumbnail was properly created.</span></span>

1. <span data-ttu-id="6359b-196">Antes de pasar al siguiente tutorial, elimine todos los archivos de los contenedores **images** y **thumbnails**.</span><span class="sxs-lookup"><span data-stu-id="6359b-196">Before moving on to the next tutorial, delete all files in the **images** and **thumbnails** containers.</span></span>

    ```azurecli
    az storage blob delete-batch -s images --account-name <storage account name>
    ```
    ```azurecli
    az storage blob delete-batch -s thumbnails --account-name <storage account name>
    ```


## <a name="summary"></a><span data-ttu-id="6359b-197">Resumen</span><span class="sxs-lookup"><span data-stu-id="6359b-197">Summary</span></span>

<span data-ttu-id="6359b-198">En este módulo, creó una función sin servidor para crear una miniatura cada vez que se carga una imagen en un contenedor de almacenamiento de blobs.</span><span class="sxs-lookup"><span data-stu-id="6359b-198">In this module, you created a serverless function to create a thumbnail whenever an image is uploaded to a Blob storage container.</span></span> <span data-ttu-id="6359b-199">A continuación, aprenderá a usar Azure Cosmos DB para almacenar y mostrar metadatos de imagen.</span><span class="sxs-lookup"><span data-stu-id="6359b-199">Next, you learn how to use Azure Cosmos DB to store and list image metadata.</span></span>

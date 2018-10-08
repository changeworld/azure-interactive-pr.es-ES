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
ms.openlocfilehash: 194a25dbf9abda80379aa5aab408ac4ffe9ab7f5
ms.sourcegitcommit: 81587470a181e314242c7a97cd0f91c82d4fe232
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/01/2018
ms.locfileid: "47460065"
---
<span data-ttu-id="fa01f-103">Azure Cosmos DB es la base de datos multimodelo, globalmente distribuida y sin servidor de Microsoft.</span><span class="sxs-lookup"><span data-stu-id="fa01f-103">Azure Cosmos DB is Microsoft's serverless, globally distributed, multi-model database.</span></span> <span data-ttu-id="fa01f-104">En este módulo, aprenderá a usar Azure Functions para almacenar y recuperar metadatos de imagen como documentos JSON en Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="fa01f-104">In this module, you learn how to use Azure Functions to store and retrieve image metadata as JSON documents in Cosmos DB.</span></span>

## <a name="create-a-cosmos-db-account-database-and-collection"></a><span data-ttu-id="fa01f-105">Creación de una cuenta, una base de datos y una colección de Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="fa01f-105">Create a Cosmos DB account, database, and collection</span></span>

<span data-ttu-id="fa01f-106">Una cuenta de Cosmos DB es un recurso de Azure que contiene bases de datos de Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="fa01f-106">A Cosmos DB account is an Azure resource that contains Cosmos DB databases.</span></span>

1. <span data-ttu-id="fa01f-107">Asegúrese de que aún tiene la sesión iniciada en Cloud Shell.</span><span class="sxs-lookup"><span data-stu-id="fa01f-107">Ensure you are still signed into the Cloud Shell.</span></span>  <span data-ttu-id="fa01f-108">En caso contrario, seleccione **Enter focus mode** (Entrar en modo de enfoque) para abrir una ventana de Cloud Shell.</span><span class="sxs-lookup"><span data-stu-id="fa01f-108">If not, select **Enter focus mode** to open a Cloud Shell window.</span></span> 

1. <span data-ttu-id="fa01f-109">Cree una cuenta de Cosmos DB con un nombre único en el mismo grupo de recursos que los otros recursos de este tutorial.</span><span class="sxs-lookup"><span data-stu-id="fa01f-109">Create a Cosmos DB account with a unique name in the same resource group as the other resources in this tutorial.</span></span>

    ```azurecli
    az cosmosdb create -g first-serverless-app -n <cosmos db account name>
    ```

1. <span data-ttu-id="fa01f-110">Después de crear la cuenta de Cosmos DB, cree una base de datos llamada **imagesdb** en la cuenta.</span><span class="sxs-lookup"><span data-stu-id="fa01f-110">After the Cosmos DB account is created, create a new database named **imagesdb** in the account.</span></span>

    ```azurecli
    az cosmosdb database create -g first-serverless-app -n <cosmos db account name> --db-name imagesdb
    ```

1. <span data-ttu-id="fa01f-111">Una vez creada la base de datos, cree una colección llamada **images** en la base de datos con un rendimiento de 400 unidades de solicitud (RU).</span><span class="sxs-lookup"><span data-stu-id="fa01f-111">After the database is created, create a new collection named **images** in the database with a throughput of 400 request units (RUs).</span></span>

    ```azurecli
    az cosmosdb collection create -g first-serverless-app -n <cosmos db account name> --db-name imagesdb --collection-name images --throughput 400
    ```


## <a name="save-a-document-to-cosmos-db-when-a-thumbnail-is-created"></a><span data-ttu-id="fa01f-112">Guardar un documento en Cosmos DB cuando se crea una miniatura</span><span class="sxs-lookup"><span data-stu-id="fa01f-112">Save a document to Cosmos DB when a thumbnail is created</span></span>

<span data-ttu-id="fa01f-113">El enlace de salida de Cosmos DB le permite crear documentos en una colección de Cosmos DB desde Azure Functions.</span><span class="sxs-lookup"><span data-stu-id="fa01f-113">The Cosmos DB output binding lets you create documents in a Cosmos DB collection from Azure Functions.</span></span> <span data-ttu-id="fa01f-114">En los pasos siguientes, configurará un enlace de salida de Cosmos DB en la función **ResizeImage** y modificará la función para devolver un documento (objeto) para guardar.</span><span class="sxs-lookup"><span data-stu-id="fa01f-114">In the following steps, you configure a Cosmos DB output binding in the **ResizeImage** function and modify the function to return a document (object) to be saved.</span></span>

1. <span data-ttu-id="fa01f-115">Abra la aplicación de función en Azure Portal.</span><span class="sxs-lookup"><span data-stu-id="fa01f-115">Open the function app in the Azure Portal.</span></span>

1. <span data-ttu-id="fa01f-116">En el panel de navegación izquierdo, expanda la función **ResizeImage** y, a continuación, seleccione **Integrar**.</span><span class="sxs-lookup"><span data-stu-id="fa01f-116">In the left hand navigation, expand the **ResizeImage** function, and then select **Integrate**.</span></span>

1. <span data-ttu-id="fa01f-117">En **Salida**, haga clic en **Nueva salida**.</span><span class="sxs-lookup"><span data-stu-id="fa01f-117">Under **Outputs**, click **New Output**.</span></span>

1. <span data-ttu-id="fa01f-118">Busque el elemento **Azure Cosmos DB** y selecciónelo.</span><span class="sxs-lookup"><span data-stu-id="fa01f-118">Find the **Azure Cosmos DB** item and select it.</span></span> <span data-ttu-id="fa01f-119">Después, haga clic en **Seleccionar**.</span><span class="sxs-lookup"><span data-stu-id="fa01f-119">Then click **Select**.</span></span>

    ![Selección de la nueva salida](media/functions-first-serverless-web-app/4-new-output.jpg)

1. <span data-ttu-id="fa01f-121">Rellene los campos de **Azure Cosmos DB output** (Salida de Azure Cosmos DB) con los valores siguientes.</span><span class="sxs-lookup"><span data-stu-id="fa01f-121">Fill out the fields under **Azure Cosmos DB output** with the following values.</span></span>

    | <span data-ttu-id="fa01f-122">Configuración</span><span class="sxs-lookup"><span data-stu-id="fa01f-122">Setting</span></span>      |  <span data-ttu-id="fa01f-123">Valor sugerido</span><span class="sxs-lookup"><span data-stu-id="fa01f-123">Suggested value</span></span>   | <span data-ttu-id="fa01f-124">DESCRIPCIÓN</span><span class="sxs-lookup"><span data-stu-id="fa01f-124">Description</span></span>                                        |
    | --- | --- | ---|
    | <span data-ttu-id="fa01f-125">**Nombre del parámetro de documento**</span><span class="sxs-lookup"><span data-stu-id="fa01f-125">**Document parameter name**</span></span> | <span data-ttu-id="fa01f-126">Seleccione **Use function return value** (Usar valor de devolución de función).</span><span class="sxs-lookup"><span data-stu-id="fa01f-126">Select **Use function return value**</span></span> | <span data-ttu-id="fa01f-127">El valor del cuadro de texto se establece automáticamente en **$return**.</span><span class="sxs-lookup"><span data-stu-id="fa01f-127">The value of the textbox is automatically set to **$return**.</span></span> |
    | <span data-ttu-id="fa01f-128">**Nombre de la base de datos**</span><span class="sxs-lookup"><span data-stu-id="fa01f-128">**Database name**</span></span> | <span data-ttu-id="fa01f-129">imagesdb</span><span class="sxs-lookup"><span data-stu-id="fa01f-129">imagesdb</span></span> | <span data-ttu-id="fa01f-130">Use el nombre de la base de datos que ha creado.</span><span class="sxs-lookup"><span data-stu-id="fa01f-130">Use the name of the database that you created.</span></span> |
    | <span data-ttu-id="fa01f-131">**Nombre de colección**</span><span class="sxs-lookup"><span data-stu-id="fa01f-131">**Collection name**</span></span> | <span data-ttu-id="fa01f-132">images</span><span class="sxs-lookup"><span data-stu-id="fa01f-132">images</span></span> | <span data-ttu-id="fa01f-133">Use el nombre de la colección que ha creado.</span><span class="sxs-lookup"><span data-stu-id="fa01f-133">Use the name of the collection that you created.</span></span> |

1. <span data-ttu-id="fa01f-134">Junto a **Conexión de la cuenta de Azure Cosmos DB**, haga clic en **new** (nuevo).</span><span class="sxs-lookup"><span data-stu-id="fa01f-134">Next to **Azure Cosmos DB account connection**, click **new**.</span></span> <span data-ttu-id="fa01f-135">Seleccione la cuenta de Cosmos DB que creó anteriormente.</span><span class="sxs-lookup"><span data-stu-id="fa01f-135">Select the Cosmos DB account you previously created.</span></span>

    ![Especificación de la configuración del enlace de salida de Azure Cosmos DB](media/functions-first-serverless-web-app/4-cosmos-db-output.png)

1. <span data-ttu-id="fa01f-137">Haga clic en **Guardar** para crear el enlace de salida de Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="fa01f-137">Click **Save** to create the Cosmos DB output binding.</span></span>

1. <span data-ttu-id="fa01f-138">Haga clic en el nombre de función **ResizeImage** a la izquierda para abrir la función.</span><span class="sxs-lookup"><span data-stu-id="fa01f-138">Click on the **ResizeImage** function name on the left to open the function.</span></span>

1. <span data-ttu-id="fa01f-139">**C#**</span><span class="sxs-lookup"><span data-stu-id="fa01f-139">**C#**</span></span>

    1. <span data-ttu-id="fa01f-140">(C#) Cambie el tipo de valor devuelto de la función de **void** a **object**.</span><span class="sxs-lookup"><span data-stu-id="fa01f-140">(C#) Change the return type of the function from **void** to **object**.</span></span>

    1. <span data-ttu-id="fa01f-141">(C#) Al final de la función, agregue el siguiente bloque de código para devolver el documento que se va a guardar:</span><span class="sxs-lookup"><span data-stu-id="fa01f-141">(C#) At the end of the function, add the following code block to return the document to be saved:</span></span>
    
        ```csharp
        return new {
            id = name,
            imgPath = "/images/" + name,
            thumbnailPath = "/thumbnails/" + name
        };
        ```
    
        ![run.csx para la función ResizeImages tras las modificaciones](media/functions-first-serverless-web-app/4-update-function.png)

1. <span data-ttu-id="fa01f-143">**JavaScript**</span><span class="sxs-lookup"><span data-stu-id="fa01f-143">**JavaScript**</span></span>

    1. <span data-ttu-id="fa01f-144">(JavaScript) Cambie la instrucción `context.done()` de la cláusula `else` para devolver el documento que se va a guardar a Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="fa01f-144">(JavaScript) Change the `context.done()` statement in the `else` clause to return the document to be saved to Cosmos DB.</span></span>

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

1. <span data-ttu-id="fa01f-145">Haga clic en **Registros** debajo de la ventana de código para expandir el panel de registros.</span><span class="sxs-lookup"><span data-stu-id="fa01f-145">Click **Logs** below the code window to expand the logs panel.</span></span>

1. <span data-ttu-id="fa01f-146">Haga clic en **Save**(Guardar).</span><span class="sxs-lookup"><span data-stu-id="fa01f-146">Click **Save**.</span></span> <span data-ttu-id="fa01f-147">Compruebe el panel de registros para asegurarse de que la función se guarda correctamente y no hay errores.</span><span class="sxs-lookup"><span data-stu-id="fa01f-147">Check the logs panel to ensure the function is successfully saved and there are no errors.</span></span>


## <a name="create-a-function-to-list-images-from-cosmos-db"></a><span data-ttu-id="fa01f-148">Creación de una función para mostrar imágenes de Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="fa01f-148">Create a function to list images from Cosmos DB</span></span>

<span data-ttu-id="fa01f-149">La aplicación web requiere una API para recuperar metadatos de imagen de Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="fa01f-149">The web application requires an API to retrieve image metadata from Cosmos DB.</span></span> <span data-ttu-id="fa01f-150">En los siguientes pasos,</span><span class="sxs-lookup"><span data-stu-id="fa01f-150">In the following steps.</span></span> <span data-ttu-id="fa01f-151">creará una función desencadenada mediante HTTP que usa un enlace de entrada de Cosmos DB para consultar la colección de base de datos.</span><span class="sxs-lookup"><span data-stu-id="fa01f-151">you create an HTTP triggered function that uses a Cosmos DB input binding to query the database collection.</span></span>

1. <span data-ttu-id="fa01f-152">En la aplicación de función, mantenga el mouse sobre **Funciones** a la izquierda y haga clic en **+** para crear una función.</span><span class="sxs-lookup"><span data-stu-id="fa01f-152">In your function app, hover over **Functions** on the left and click **+** to create a new function.</span></span>

1. <span data-ttu-id="fa01f-153">Busque la plantilla **HttpTrigger** y selecciónela.</span><span class="sxs-lookup"><span data-stu-id="fa01f-153">Find the **HttpTrigger** template and select it.</span></span>

1. <span data-ttu-id="fa01f-154">Use estos valores para crear una función que genere una dirección URL para obtener imágenes.</span><span class="sxs-lookup"><span data-stu-id="fa01f-154">Use these values to create a function that generates a get images URL.</span></span>

    | <span data-ttu-id="fa01f-155">Configuración</span><span class="sxs-lookup"><span data-stu-id="fa01f-155">Setting</span></span>      |  <span data-ttu-id="fa01f-156">Valor sugerido</span><span class="sxs-lookup"><span data-stu-id="fa01f-156">Suggested value</span></span>   | <span data-ttu-id="fa01f-157">DESCRIPCIÓN</span><span class="sxs-lookup"><span data-stu-id="fa01f-157">Description</span></span>                                        |
    | --- | --- | ---|
    | <span data-ttu-id="fa01f-158">**Asigne un nombre a la función**</span><span class="sxs-lookup"><span data-stu-id="fa01f-158">**Name your function**</span></span> | <span data-ttu-id="fa01f-159">GetImages</span><span class="sxs-lookup"><span data-stu-id="fa01f-159">GetImages</span></span> | <span data-ttu-id="fa01f-160">Escriba este nombre tal y como se muestra para que la aplicación pueda detectar la función.</span><span class="sxs-lookup"><span data-stu-id="fa01f-160">Type this name exactly as shown so the application can discover the function.</span></span> |
    | <span data-ttu-id="fa01f-161">**Nivel de autorización**</span><span class="sxs-lookup"><span data-stu-id="fa01f-161">**Authorization level**</span></span> | <span data-ttu-id="fa01f-162">Anónimas</span><span class="sxs-lookup"><span data-stu-id="fa01f-162">Anonymous</span></span> | <span data-ttu-id="fa01f-163">Permita que la función sea accesible públicamente.</span><span class="sxs-lookup"><span data-stu-id="fa01f-163">Allow the function to be accessed publicly.</span></span> |

1. <span data-ttu-id="fa01f-164">Haga clic en **Create**(Crear).</span><span class="sxs-lookup"><span data-stu-id="fa01f-164">Click **Create**.</span></span>

1. <span data-ttu-id="fa01f-165">Cuando se cree la nueva función, haga clic en **Integrar** en el nombre de la función en el panel de navegación izquierdo.</span><span class="sxs-lookup"><span data-stu-id="fa01f-165">When the new function is created, click **Integrate** under the function's name on the left navigation.</span></span>

1. <span data-ttu-id="fa01f-166">Haga clic en **Nueva entrada** y seleccione **Azure Cosmos DB**.</span><span class="sxs-lookup"><span data-stu-id="fa01f-166">Click **New Input** and select **Azure Cosmos DB**.</span></span> 

    ![Selección de la nueva entrada](media/functions-first-serverless-web-app/4-new-input.jpg)

1. <span data-ttu-id="fa01f-168">Haga clic en **Seleccionar**.</span><span class="sxs-lookup"><span data-stu-id="fa01f-168">Click **Select**.</span></span>

1. <span data-ttu-id="fa01f-169">Rellene los siguientes valores:</span><span class="sxs-lookup"><span data-stu-id="fa01f-169">Fill out the following values:</span></span>

    | <span data-ttu-id="fa01f-170">Configuración</span><span class="sxs-lookup"><span data-stu-id="fa01f-170">Setting</span></span>      |  <span data-ttu-id="fa01f-171">Valor sugerido</span><span class="sxs-lookup"><span data-stu-id="fa01f-171">Suggested value</span></span>   | <span data-ttu-id="fa01f-172">DESCRIPCIÓN</span><span class="sxs-lookup"><span data-stu-id="fa01f-172">Description</span></span>                                        |
    | --- | --- | ---|
    | <span data-ttu-id="fa01f-173">**Nombre del parámetro de documento**</span><span class="sxs-lookup"><span data-stu-id="fa01f-173">**Document parameter name**</span></span> | <span data-ttu-id="fa01f-174">documents</span><span class="sxs-lookup"><span data-stu-id="fa01f-174">documents</span></span> | <span data-ttu-id="fa01f-175">Busca la coincidencia del nombre del parámetro en la función.</span><span class="sxs-lookup"><span data-stu-id="fa01f-175">Matches parameter name in the function.</span></span> |
    | <span data-ttu-id="fa01f-176">**Nombre de la base de datos**</span><span class="sxs-lookup"><span data-stu-id="fa01f-176">**Database name**</span></span> | <span data-ttu-id="fa01f-177">imagesdb</span><span class="sxs-lookup"><span data-stu-id="fa01f-177">imagesdb</span></span> |  |
    | <span data-ttu-id="fa01f-178">**Nombre de colección**</span><span class="sxs-lookup"><span data-stu-id="fa01f-178">**Collection name**</span></span> | <span data-ttu-id="fa01f-179">images</span><span class="sxs-lookup"><span data-stu-id="fa01f-179">images</span></span> |  |
    | <span data-ttu-id="fa01f-180">**SQL query**</span><span class="sxs-lookup"><span data-stu-id="fa01f-180">**SQL query**</span></span> | <span data-ttu-id="fa01f-181">select \* from c order by c._ts desc</span><span class="sxs-lookup"><span data-stu-id="fa01f-181">select \* from c order by c._ts desc</span></span> | <span data-ttu-id="fa01f-182">Obtiene documentos, los más recientes primero.</span><span class="sxs-lookup"><span data-stu-id="fa01f-182">Get documents, latest documents first.</span></span> |
    | <span data-ttu-id="fa01f-183">**Conexión de la cuenta de Azure Cosmos DB**</span><span class="sxs-lookup"><span data-stu-id="fa01f-183">**Azure Cosmos DB account connection**</span></span> | <span data-ttu-id="fa01f-184">Seleccionar cadena de conexión existente</span><span class="sxs-lookup"><span data-stu-id="fa01f-184">Select existing connection string</span></span> |  |

1. <span data-ttu-id="fa01f-185">Haga clic en **Guardar** para crear el enlace de entrada.</span><span class="sxs-lookup"><span data-stu-id="fa01f-185">Click **Save** to create the input binding.</span></span>

1. <span data-ttu-id="fa01f-186">**C#**</span><span class="sxs-lookup"><span data-stu-id="fa01f-186">**C#**</span></span>

    1. <span data-ttu-id="fa01f-187">Haga clic en el nombre de la función para abrir la ventana de código y, a continuación, reemplace **run.csx** por el contenido de [**/csharp/GetImages/run.csx**](https://raw.githubusercontent.com/Azure-Samples/functions-first-serverless-web-application/master/csharp/GetImages/run.csx).</span><span class="sxs-lookup"><span data-stu-id="fa01f-187">Click the function's name to open the code window, and then replace all of **run.csx** with the content in [**/csharp/GetImages/run.csx**](https://raw.githubusercontent.com/Azure-Samples/functions-first-serverless-web-application/master/csharp/GetImages/run.csx).</span></span>

1. <span data-ttu-id="fa01f-188">**JavaScript**</span><span class="sxs-lookup"><span data-stu-id="fa01f-188">**JavaScript**</span></span>

    1. <span data-ttu-id="fa01f-189">Haga clic en el nombre de la función para abrir la ventana de código y, a continuación, reemplace **index.js** por el contenido de [**/javascript/GetImages/index.js**](https://raw.githubusercontent.com/Azure-Samples/functions-first-serverless-web-application/master/javascript/GetImages/index.js).</span><span class="sxs-lookup"><span data-stu-id="fa01f-189">Click the function's name to open the code window, and then replace all of **index.js** with the content in [**/javascript/GetImages/index.js**](https://raw.githubusercontent.com/Azure-Samples/functions-first-serverless-web-application/master/javascript/GetImages/index.js).</span></span>

1. <span data-ttu-id="fa01f-190">Haga clic en **Registros** debajo de la ventana de código para expandir el panel de registros.</span><span class="sxs-lookup"><span data-stu-id="fa01f-190">Click **Logs** below the code window to expand the logs panel.</span></span>

1. <span data-ttu-id="fa01f-191">Haga clic en **Save**(Guardar).</span><span class="sxs-lookup"><span data-stu-id="fa01f-191">Click **Save**.</span></span> <span data-ttu-id="fa01f-192">Compruebe el panel de registros para asegurarse de que la función se guarda correctamente y no hay errores.</span><span class="sxs-lookup"><span data-stu-id="fa01f-192">Check the logs panel to ensure the function is successfully saved and there are no errors.</span></span>


## <a name="test-the-application"></a><span data-ttu-id="fa01f-193">Prueba de la aplicación</span><span class="sxs-lookup"><span data-stu-id="fa01f-193">Test the application</span></span>

1. <span data-ttu-id="fa01f-194">Abra la aplicación en un explorador.</span><span class="sxs-lookup"><span data-stu-id="fa01f-194">Open the application in a browser.</span></span> <span data-ttu-id="fa01f-195">Seleccione un archivo de imagen y cárguelo.</span><span class="sxs-lookup"><span data-stu-id="fa01f-195">Select an image file and upload it.</span></span>

1. <span data-ttu-id="fa01f-196">Al cabo de unos segundos, la miniatura de la nueva imagen aparece en la página.</span><span class="sxs-lookup"><span data-stu-id="fa01f-196">After a few seconds, the thumbnail of the new image appears on the page.</span></span>

1. <span data-ttu-id="fa01f-197">En Azure Portal, use el cuadro de búsqueda para buscar la cuenta de Cosmos DB por su nombre.</span><span class="sxs-lookup"><span data-stu-id="fa01f-197">In the Azure portal, use the Search box to search for your Cosmos DB account by name.</span></span> <span data-ttu-id="fa01f-198">Haga clic en ella para abrirla.</span><span class="sxs-lookup"><span data-stu-id="fa01f-198">Click it to open it.</span></span>

1. <span data-ttu-id="fa01f-199">Haga clic en **Explorador de datos** a la izquierda para examinar las colecciones y los documentos.</span><span class="sxs-lookup"><span data-stu-id="fa01f-199">Click **Data Explorer** on the left to browse collections and documents.</span></span>

1. <span data-ttu-id="fa01f-200">En la base de datos **imagesdb**, seleccione la colección **images**.</span><span class="sxs-lookup"><span data-stu-id="fa01f-200">Under the **imagesdb** database, select the **images** collection.</span></span>

1. <span data-ttu-id="fa01f-201">Confirme que se creó un documento para la imagen cargada.</span><span class="sxs-lookup"><span data-stu-id="fa01f-201">Confirm that a document was created for the uploaded image.</span></span>

    ![Explorador de datos que muestra un documento para una imagen cargada](media/functions-first-serverless-web-app/4-data-explorer.png)



## <a name="summary"></a><span data-ttu-id="fa01f-203">Resumen</span><span class="sxs-lookup"><span data-stu-id="fa01f-203">Summary</span></span>

<span data-ttu-id="fa01f-204">En este módulo, aprendió a crear una cuenta, una base de datos y una colección de Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="fa01f-204">In this module, you learned how to create a Cosmos DB account, database, and collection.</span></span> <span data-ttu-id="fa01f-205">También aprendido a usar los enlaces de Cosmos DB para guardar y recuperar metadatos de imagen en la colección de Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="fa01f-205">You also learned how to use the Cosmos DB bindings to save and retrieve image metadata in the Cosmos DB collection.</span></span> <span data-ttu-id="fa01f-206">A continuación, aprenderá a generar automáticamente un título para cada imagen cargada con Microsoft Cognitive Services.</span><span class="sxs-lookup"><span data-stu-id="fa01f-206">Next, you learn how to automatically generate a caption for each uploaded image using Microsoft Cognitive Services.</span></span>

<span data-ttu-id="dd216-101">Azure Blob Storage es un servicio rentable y muy escalable que puede usarse para hospedar archivos estáticos.</span><span class="sxs-lookup"><span data-stu-id="dd216-101">Azure Blob storage is a low-cost and massively scalable service that can be used to host static files.</span></span> <span data-ttu-id="dd216-102">En este tutorial, lo usará para servir contenido estático (por ejemplo, HTML, JavaScript, CSS) para la aplicación web que ha creado.</span><span class="sxs-lookup"><span data-stu-id="dd216-102">For this tutorial, you use it to serve static content (for example, HTML, JavaScript, CSS) for the web app that you build.</span></span>

## <a name="create-a-storage-account"></a><span data-ttu-id="dd216-103">Creación de una cuenta de Storage</span><span class="sxs-lookup"><span data-stu-id="dd216-103">Create a Storage account</span></span>

<span data-ttu-id="dd216-104">Una cuenta de almacenamiento es un recurso de Azure que le permite almacenar tablas, colas, archivos, blobs (objetos) y discos de máquina virtual.</span><span class="sxs-lookup"><span data-stu-id="dd216-104">A Storage account is an Azure resource that allows you to store tables, queues, files, blobs (objects), and virtual machine disks.</span></span>

1. <span data-ttu-id="dd216-105">Inicie sesión en Cloud Shell (Bash) con el botón **Enter focus mode** (Entrar en modo de enfoque).</span><span class="sxs-lookup"><span data-stu-id="dd216-105">Log in to the Cloud Shell (Bash), by selecting the **Enter focus mode** button.</span></span> <span data-ttu-id="dd216-106">Este botón se encuentra en la parte superior derecha o en la parte inferior de la página, según cómo de ancha sea la ventana del explorador.</span><span class="sxs-lookup"><span data-stu-id="dd216-106">This button is at the top right or the bottom of the page, depending on how wide your browser window is.</span></span> <span data-ttu-id="dd216-107">El modo de enfoque acopla una ventana de Cloud Shell en el lado derecho de la ventana del explorador, por lo que puede ejecutar fácilmente los comandos que se muestran en el tutorial.</span><span class="sxs-lookup"><span data-stu-id="dd216-107">Focus mode docks a Cloud Shell window on the right side of your browser window, so you can easily execute commands that are shown in the tutorial.</span></span>

1. <span data-ttu-id="dd216-108">En Azure, un grupo de recursos es un contenedor de recursos de Azure relacionados para facilitar la administración.</span><span class="sxs-lookup"><span data-stu-id="dd216-108">In Azure, a Resource Group is a container that holds related Azure resources for ease of management.</span></span> <span data-ttu-id="dd216-109">Cree un nuevo grupo de recursos llamado **first-serverless-app**.</span><span class="sxs-lookup"><span data-stu-id="dd216-109">Create a new resource group named **first-serverless-app**.</span></span>

    ```azurecli
    az group create -n first-serverless-app -l westcentralus
    ```

1. <span data-ttu-id="dd216-110">El contenido estático (archivos HTML, CSS y JavaScript) de este tutorial se hospeda en Blob Storage.</span><span class="sxs-lookup"><span data-stu-id="dd216-110">The static content (HTML, CSS, and JavaScript files) for this tutorial is hosted in Blob Storage.</span></span> <span data-ttu-id="dd216-111">Blob Storage necesita una cuenta de almacenamiento.</span><span class="sxs-lookup"><span data-stu-id="dd216-111">Blob Storage requires a Storage account.</span></span> <span data-ttu-id="dd216-112">Cree una cuenta de almacenamiento (de uso general V2) en el grupo de recursos.</span><span class="sxs-lookup"><span data-stu-id="dd216-112">Create a Storage account (general purpose V2) in the resource group.</span></span> <span data-ttu-id="dd216-113">Reemplace `<storage account name>` por un nombre único.</span><span class="sxs-lookup"><span data-stu-id="dd216-113">Replace `<storage account name>` with a unique name.</span></span>

    ```azurecli
    az storage account create -n <storage account name> -g first-serverless-app --kind StorageV2 -l westcentralus --https-only true --sku Standard_LRS
    ```

1. <span data-ttu-id="dd216-114">Mediante la barra de búsqueda de la parte superior de [Azure Portal](https://portal.azure.com), busque la cuenta de almacenamiento que acaba de crear y ábrala.</span><span class="sxs-lookup"><span data-stu-id="dd216-114">Using the Search bar at the top of the [Azure portal](https://portal.azure.com), find the storage account you just created and open it.</span></span>

1. <span data-ttu-id="dd216-115">En el panel de navegación izquierdo, seleccione **Sitio web estático (versión preliminar)** para configurar un contenedor para el hospedaje de sitios web estáticos.</span><span class="sxs-lookup"><span data-stu-id="dd216-115">On the left navigation, select **Static website (preview)** to configure a container for static website hosting.</span></span>
    - <span data-ttu-id="dd216-116">Seleccione **Habilitado** para habilitar el sitio web estático.</span><span class="sxs-lookup"><span data-stu-id="dd216-116">Select **Enabled** to enable static website.</span></span>
    - <span data-ttu-id="dd216-117">Escriba *index.html* como nombre del documento de índice.</span><span class="sxs-lookup"><span data-stu-id="dd216-117">Enter *index.html* as the index document name.</span></span> <span data-ttu-id="dd216-118">El campo ya contiene *index.html* con una fuente gris, pero este texto es solo de ejemplo; deberá especificar ese valor en el campo.</span><span class="sxs-lookup"><span data-stu-id="dd216-118">The field already has *index.html* in a gray font but this is only example text; you still have to enter that value in the field.</span></span>
    - <span data-ttu-id="dd216-119">Haga clic en **Save**(Guardar).</span><span class="sxs-lookup"><span data-stu-id="dd216-119">Click **Save**.</span></span>
    
    ![Especificación de la configuración del sitio web estático](media/functions-first-serverless-web-app/1-storage-static-website.png)

1. <span data-ttu-id="dd216-121">Guarde el valor de **Punto de conexión principal** en alguna parte desde la que pueda copiarlo mientras trabaja con el tutorial.</span><span class="sxs-lookup"><span data-stu-id="dd216-121">Save the **Primary Endpoint** somewhere you can conveniently copy it from while working through the tutorial.</span></span> <span data-ttu-id="dd216-122">Esta es la dirección URL de la aplicación web.</span><span class="sxs-lookup"><span data-stu-id="dd216-122">This is the URL of your web application.</span></span>

## <a name="upload-the-web-application"></a><span data-ttu-id="dd216-123">Carga de la aplicación web</span><span class="sxs-lookup"><span data-stu-id="dd216-123">Upload the web application</span></span>

1. <span data-ttu-id="dd216-124">Los archivos de origen para la aplicación que se crea en este tutorial se encuentran en un [repositorio de GitHub](https://github.com/Azure-Samples/functions-first-serverless-web-application).</span><span class="sxs-lookup"><span data-stu-id="dd216-124">The source files for the application that you build in this tutorial are located in a [GitHub repository](https://github.com/Azure-Samples/functions-first-serverless-web-application).</span></span> <span data-ttu-id="dd216-125">Asegúrese de que se encuentra en su directorio particular en Cloud Shell y clone este repositorio.</span><span class="sxs-lookup"><span data-stu-id="dd216-125">Make sure that you are in your home directory in Cloud Shell and clone this repository.</span></span>

    ```azurecli
    cd ~
    git clone https://github.com/Azure-Samples/functions-first-serverless-web-application
    ```

    <span data-ttu-id="dd216-126">El repositorio se clona en `/home/<username>/functions-first-serverless-web-application`.</span><span class="sxs-lookup"><span data-stu-id="dd216-126">The repository is cloned to `/home/<username>/functions-first-serverless-web-application`.</span></span>

1. <span data-ttu-id="dd216-127">La aplicación web del lado cliente se encuentra en la carpeta **www** y se compila mediante el marco Vue.js de JavaScript.</span><span class="sxs-lookup"><span data-stu-id="dd216-127">The client-side web application is located in the **www** folder and is built using the Vue.js JavaScript framework.</span></span> <span data-ttu-id="dd216-128">Cambie a la carpeta y ejecute los comandos de npm para instalar las dependencias de la aplicación y compilar la aplicación.</span><span class="sxs-lookup"><span data-stu-id="dd216-128">Change into the folder and run npm commands to install the application's dependencies and build the application.</span></span> <span data-ttu-id="dd216-129">El último de estos comandos puede tardar varios minutos en completarse.</span><span class="sxs-lookup"><span data-stu-id="dd216-129">The last of these commands might take several minutes to complete.</span></span>

    ```azurecli
    cd ~/functions-first-serverless-web-application/www
    npm install
    npm run generate
    ```

    <span data-ttu-id="dd216-130">La aplicación se genera en la carpeta **dist**.</span><span class="sxs-lookup"><span data-stu-id="dd216-130">The application is generated in the **dist** folder.</span></span>

1. <span data-ttu-id="dd216-131">Cambie el directorio actual a **dist** y cargue la aplicación en el contenedor de blobs **$web**.</span><span class="sxs-lookup"><span data-stu-id="dd216-131">Change the current directory to **dist** and upload the application to the **$web** Blob container.</span></span>

    ```azurecli
    cd dist
    az storage blob upload-batch -s . -d \$web --account-name <storage account name>
    ```

1. <span data-ttu-id="dd216-132">Abra la dirección URL del punto de conexión principal de sitios web estáticos de Storage en un explorador web para ver la aplicación.</span><span class="sxs-lookup"><span data-stu-id="dd216-132">Open the Storage static websites primary endpoint URL in a web browser to view the application.</span></span>

    ![Primera página principal de la aplicación web sin servidor](media/functions-first-serverless-web-app/1-app-screenshot-new.png)


## <a name="summary"></a><span data-ttu-id="dd216-134">Resumen</span><span class="sxs-lookup"><span data-stu-id="dd216-134">Summary</span></span>

<span data-ttu-id="dd216-135">En este módulo, creó un grupo de recursos llamado **first-serverless-app** que contiene una cuenta de Storage.</span><span class="sxs-lookup"><span data-stu-id="dd216-135">In this module, you created a resource group named **first-serverless-app** containing a Storage account.</span></span> <span data-ttu-id="dd216-136">Un contenedor de blobs llamado **$web** en la cuenta de Storage almacena el contenido estático de la aplicación web y permite que el contenido esté disponible públicamente.</span><span class="sxs-lookup"><span data-stu-id="dd216-136">A blob container named **$web** in the Storage account stores the static content for your web application and makes the content available publicly.</span></span> <span data-ttu-id="dd216-137">A continuación, aprenda a usar una función sin servidor para cargar imágenes en Blob Storage desde esta aplicación web.</span><span class="sxs-lookup"><span data-stu-id="dd216-137">Next, you learn how to use a serverless function to upload images to Blob storage from this web application.</span></span>

Azure Blob Storage es un servicio rentable y muy escalable que puede usarse para hospedar archivos estáticos. En este tutorial, lo usará para servir contenido estático (por ejemplo, HTML, JavaScript, CSS) para la aplicación web que ha creado.

## <a name="create-a-storage-account"></a>Creación de una cuenta de Storage

Una cuenta de almacenamiento es un recurso de Azure que le permite almacenar tablas, colas, archivos, blobs (objetos) y discos de máquina virtual.

1. Inicie sesión en Cloud Shell (Bash) con el botón **Enter focus mode** (Entrar en modo de enfoque). Este botón se encuentra en la parte superior derecha o en la parte inferior de la página, según cómo de ancha sea la ventana del explorador. El modo de enfoque acopla una ventana de Cloud Shell en el lado derecho de la ventana del explorador, por lo que puede ejecutar fácilmente los comandos que se muestran en el tutorial.

1. En Azure, un grupo de recursos es un contenedor de recursos de Azure relacionados para facilitar la administración. Cree un nuevo grupo de recursos llamado **first-serverless-app**.

    ```azurecli
    az group create -n first-serverless-app -l westcentralus
    ```

1. El contenido estático (archivos HTML, CSS y JavaScript) de este tutorial se hospeda en Blob Storage. Blob Storage necesita una cuenta de almacenamiento. Cree una cuenta de almacenamiento (de uso general V2) en el grupo de recursos. Reemplace `<storage account name>` por un nombre único.

    ```azurecli
    az storage account create -n <storage account name> -g first-serverless-app --kind StorageV2 -l westcentralus --https-only true --sku Standard_LRS
    ```

1. Mediante la barra de búsqueda de la parte superior de [Azure Portal](https://portal.azure.com), busque la cuenta de almacenamiento que acaba de crear y ábrala.

1. En el panel de navegación izquierdo, seleccione **Sitio web estático (versión preliminar)** para configurar un contenedor para el hospedaje de sitios web estáticos.
    - Seleccione **Habilitado** para habilitar el sitio web estático.
    - Escriba *index.html* como nombre del documento de índice. El campo ya contiene *index.html* con una fuente gris, pero este texto es solo de ejemplo; deberá especificar ese valor en el campo.
    - Haga clic en **Save**(Guardar).
    
    ![Especificación de la configuración del sitio web estático](media/functions-first-serverless-web-app/1-storage-static-website.png)

1. Guarde el valor de **Punto de conexión principal** en alguna parte desde la que pueda copiarlo mientras trabaja con el tutorial. Esta es la dirección URL de la aplicación web.

## <a name="upload-the-web-application"></a>Carga de la aplicación web

1. Los archivos de origen para la aplicación que se crea en este tutorial se encuentran en un [repositorio de GitHub](https://github.com/Azure-Samples/functions-first-serverless-web-application). Asegúrese de que se encuentra en su directorio particular en Cloud Shell y clone este repositorio.

    ```azurecli
    cd ~
    git clone https://github.com/Azure-Samples/functions-first-serverless-web-application
    ```

    El repositorio se clona en `/home/<username>/functions-first-serverless-web-application`.

1. La aplicación web del lado cliente se encuentra en la carpeta **www** y se compila mediante el marco Vue.js de JavaScript. Cambie a la carpeta y ejecute los comandos de npm para instalar las dependencias de la aplicación y compilar la aplicación. El último de estos comandos puede tardar varios minutos en completarse.

    ```azurecli
    cd ~/functions-first-serverless-web-application/www
    npm install
    npm run generate
    ```

    La aplicación se genera en la carpeta **dist**.

1. Cambie el directorio actual a **dist** y cargue la aplicación en el contenedor de blobs **$web**.

    ```azurecli
    cd dist
    az storage blob upload-batch -s . -d \$web --account-name <storage account name>
    ```

1. Abra la dirección URL del punto de conexión principal de sitios web estáticos de Storage en un explorador web para ver la aplicación.

    ![Primera página principal de la aplicación web sin servidor](media/functions-first-serverless-web-app/1-app-screenshot-new.png)


## <a name="summary"></a>Resumen

En este módulo, creó un grupo de recursos llamado **first-serverless-app** que contiene una cuenta de Storage. Un contenedor de blobs llamado **$web** en la cuenta de Storage almacena el contenido estático de la aplicación web y permite que el contenido esté disponible públicamente. A continuación, aprenda a usar una función sin servidor para cargar imágenes en Blob Storage desde esta aplicación web.

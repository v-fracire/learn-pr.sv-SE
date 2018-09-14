<span data-ttu-id="5bf98-101">I den här övningen ska du läsa in anslutningssträngen för Azure Storage-kontot från konfigurationen och använda den för att ansluta till Azure Storage-kontot.</span><span class="sxs-lookup"><span data-stu-id="5bf98-101">In this unit, you'll load the Azure storage account connection string from configuration and use it to connect to the Azure storage account.</span></span>

## <a name="retrieve-the-connection-string"></a><span data-ttu-id="5bf98-102">Hämta anslutningssträngen</span><span class="sxs-lookup"><span data-stu-id="5bf98-102">Retrieve the connection string</span></span>

1. <span data-ttu-id="5bf98-103">Se till så att konsolprogrammet från den tidigare enheten är inläst i Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="5bf98-103">Ensure the console application from the previous unit is loaded into Visual Studio.</span></span>

1. <span data-ttu-id="5bf98-104">Lägg till ett *using*-uttryck överst i filen **Program.cs** för att referera till biblioteket **Microsoft.WindowsAzure.Storage**:</span><span class="sxs-lookup"><span data-stu-id="5bf98-104">In the **Program.cs** file, add a *using* statement at the top of the file to reference the **Microsoft.WindowsAzure.Storage** library:</span></span>

    ```csharp
    using Microsoft.WindowsAzure.Storage;
    ```

1. <span data-ttu-id="5bf98-105">Längst ned i **Huvud**metoden lägger du till följande rad för att hämta anslutningssträngen för Azure Storage-kontot från konfigurationsfilen:</span><span class="sxs-lookup"><span data-stu-id="5bf98-105">At the bottom of the **Main** method, add the following line to retrieve the Azure storage account connection string from the configuration file:</span></span>

    ```csharp
    var connectionString = configuration["StorageAccountConnectionString"];
    ```

## <a name="create-a-blob-client"></a><span data-ttu-id="5bf98-106">Skapa en blobbklient</span><span class="sxs-lookup"><span data-stu-id="5bf98-106">Create a blob client</span></span>

1. <span data-ttu-id="5bf98-107">Infoga följande kod längst ned i **huvudmetoden** för att parsa anslutningssträngen och skapa en blobklient:</span><span class="sxs-lookup"><span data-stu-id="5bf98-107">Insert the following code at the bottom of the **Main** method to parse the connection string and create a blob client:</span></span>

    ```csharp
    CloudStorageAccount storageAccount;
    if (!CloudStorageAccount.TryParse(connectionString, out storageAccount))
    {
        Console.WriteLine("Unable to parse connection string");
        return;
    }
    var blobClient = storageAccount.CreateCloudBlobClient();
    ```

1. <span data-ttu-id="5bf98-108">Infoga följande kod för att skapa en blobklient och hämta en referens till en container längst ned i **huvudmetoden**. Om inte containern finns kan du skapa den.</span><span class="sxs-lookup"><span data-stu-id="5bf98-108">Insert the following code to create a blob client and retrieve a reference to a container, optionally creating it if it does not exist at the bottom of the **Main** method:</span></span>

    ```csharp
    var containerName = "MyBlobContainer";
    var success = blobClient
        .GetContainerReference(containerName)
        .CreateIfNotExistsAsync()
        .Result;
    ```

    > [!Warning]
    > <span data-ttu-id="5bf98-109">Observera användningen av den **async** metoden **CreateIfNotExistsAsync()** och motsvarande **resultatet** egenskapen.</span><span class="sxs-lookup"><span data-stu-id="5bf98-109">Note The use of the **async** method **CreateIfNotExistsAsync()** and corresponding **Result** property.</span></span> <span data-ttu-id="5bf98-110">I normalfallet använder man nyckelordet **await** i ett program.</span><span class="sxs-lookup"><span data-stu-id="5bf98-110">In a typical application, we would normally use the **await** keyword.</span></span> <span data-ttu-id="5bf98-111">Men det här programmet är ett konsolprogram och är inte asynkrona, så vi behöver för att anropa den **resultatet** egenskapen för att hämta resultatet av async-aktivitet som skapats av den **CreateIfNotExistsAsync()** metod.</span><span class="sxs-lookup"><span data-stu-id="5bf98-111">However, this application is a console application and is not async, so we need to call the **Result** property to get the results of the async task created by the **CreateIfNotExistsAsync()** method.</span></span>

## <a name="print-a-status-message"></a><span data-ttu-id="5bf98-112">Skriv ut ett statusmeddelande</span><span class="sxs-lookup"><span data-stu-id="5bf98-112">Print a status message</span></span>

1. <span data-ttu-id="5bf98-113">Lägg till följande kod längst ned i **huvudmetoden** för att skriva ut ett meddelande om åtgärden lyckades eller misslyckades.</span><span class="sxs-lookup"><span data-stu-id="5bf98-113">Add the following code at the bottom of the **Main** method to print a success or failure message.</span></span>

    ```csharp
    if (!success)
    {
        Console.WriteLine("Error: Could not connect to Azure Storage container");
        return;
    }
    Console.WriteLine("Successfully connected to Azure Storage container");
    ```

1. <span data-ttu-id="5bf98-114">Avsluta med att köra programmet för att kontrollera att en anslutning upprättas. Öppna Azure-portalen och kontrollera så att det har skapats en Azure Storage-container om det inte fanns någon sedan tidigare.</span><span class="sxs-lookup"><span data-stu-id="5bf98-114">Finally, run the application to verify that a successful connection is made, and view the Azure portal to ensure the Azure Storage container is created if it did not exist previously.</span></span>

1. <span data-ttu-id="5bf98-115">**Valfritt**: Om du inte tänker använda containern kan du ta bort resursgruppen där containern finns för att säkerställa att du inte debiteras för resurser som du inte använder.</span><span class="sxs-lookup"><span data-stu-id="5bf98-115">**Optional**: If you don't plan on using the container, delete the resource group where the container is located to ensure you are not charged for resources you are not using.</span></span>

I den här övningen ska du läsa in anslutningssträngen för Azure Storage-kontot från konfigurationen och använda den för att ansluta till Azure Storage-kontot.

## <a name="retrieve-the-connection-string"></a>Hämta anslutningssträngen

1. Se till så att konsolprogrammet från den tidigare enheten är inläst i Visual Studio.
1. Lägg till ett *using*-uttryck överst i filen **Program.cs** för att referera till biblioteket **Microsoft.WindowsAzure.Storage**:
    ```csharp
    using Microsoft.WindowsAzure.Storage;
    ```
1. Längst ned i **huvudmetoden** lägger du till följande rad för att hämta anslutningssträngen för Azure Storage-kontot från konfigurationsfilen:
    ```csharp
    var connectionString = configuration["StorageAccountConnectionString"];
    ```

## <a name="create-a-blob-client"></a>Skapa en blobklient

1. Infoga följande kod längst ned i **huvudmetoden** för att parsa anslutningssträngen och skapa en blobklient:
    ```csharp
    CloudStorageAccount storageAccount;
    if (!CloudStorageAccount.TryParse(connectionString,out storageAccount))
    {
        Console.WriteLine("Unable to parse connection string");
        return;
    }
    var blobClient = storageAccount.CreateCloudBlobClient();
    ```
1. Infoga följande kod för att skapa en blobklient och hämta en referens till en container längst ned i **huvudmetoden**. Om inte containern finns kan du skapa den.
    ```csharp
    var containerName = "MyBlobContainer";
    var success = blobClient
        .GetContainerReference(containerName)
        .CreateIfNotExistsAsync()
        .Result;
    ```

  *Observera hur den **asynkrona** metoden **CreateIfNotExistsAsync()** används och den motsvarande **Result**-egenskapen. I normalfallet använder man nyckelordet **await** i ett program. Det här är dock ett konsolprogram som inte är asynkront, så vi måste anropa egenskapen **Result** för att hämta resultatet av den asynkrona aktivitet som skapats av metoden **CreateIfNotExistsAsync()**.*

## <a name="print-a-status-message"></a>Visa ett statusmeddelande

1. Lägg till följande kod längst ned i **huvudmetoden** för att visa ett meddelande som huruvida åtgärden lyckades eller misslyckades.
    ```csharp
    if (!success)
    {
        Console.WriteLine("Error: Could not connect to Azure Storage container");
        return;
    }
    Console.WriteLine("Successfully connected to Azure Storage container");
    ```
1. Avsluta med att köra programmet för att kontrollera att en anslutning upprättas. Öppna Azure-portalen och kontrollera så att det har skapats en Storage-container (om det inte fanns någon sedan tidigare).
1. **Valfritt**: Om du inte tänker använda containern kan du ta bort resursgruppen där containern finns för att säkerställa att du inte debiteras för resurser som du inte använder.


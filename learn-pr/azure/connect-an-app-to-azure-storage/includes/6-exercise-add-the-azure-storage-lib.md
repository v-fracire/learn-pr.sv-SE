::: zone pivot="csharp"
Nu ska vi integrera Azure Storage-klientbiblioteket i ditt .NET Core-konsolprogram.

Azure Storages klientbibliotek för .NET distribueras med NuGet. Du kan lägga till **WindowsAzure.Storage**-paketet till dina .NET- eller .NET Core-program.

## <a name="add-the-azure-storage-nuget-package"></a>Lägga till NuGet-paketet Azure Storage

1. I terminalen, `cd` till katalogen PhotoSharingApp om du inte redan är där.

1. Lägg till **WindowsAzure.Storage**-paketet i programmet.

    ```bash
    dotnet add package WindowsAzure.Storage
    ```

1. Detta resulterar i viss konsolaktivitet medan klientbiblioteket och alla de nödvändiga beroendena laddas ned. När det är klart går du vidare och skapar och kör appen igen för att kontrollera att allt fungerar.

    ```bash
    dotnet run
    ```

1. Som tidigare bör den visa ”Hello World!”.

::: zone-end

::: zone pivot="javascript"

Nu ska vi integrera **Microsoft Azure Storage-klientbibliotek för Node.js och JavaScript** i ditt program.

Klientbiblioteket för Node.js är tillgängligt via NPM (Node Package Manager). Du kan lägga till paketet **azure-storage** i din **packages.json**-fil.

> [!NOTE]
> **Microsoft Azure Storage-klientbibliotek för Node.js och JavaScript** är avsett att användas i serverprogram. Om du använder JavaScript för klientsidan kan du använda **Azure Storage-klientbibliotek för JavaScript** som fungerar på samma sätt, men som är skräddarsytt för att köras i en webbläsare.

## <a name="add-the-azure-storage-package"></a>Lägga till Azure Storage-paketet

1. I Cloud Shell, `cd` till katalogen PhotoSharingApp om du inte redan är där.

1. Lägg till paketet **azure-storage** i programmet. Se till att ange `--save`-alternativet så att det finns kvar i **packages.json**.

    ```bash
    npm install azure-storage --save
    ```

1. Detta resulterar i viss konsolaktivitet medan klientbiblioteket och alla de nödvändiga beroendena laddas ned. När det är klart går du vidare och skapar och kör appen igen för att kontrollera att allt fungerar.

    ```bash
    node index.js
    ```

1. Som tidigare bör den visa ”Hello, World!”.

::: zone-end

Nu när vi har nödvändiga bibliotek på plats kan vi titta på vanliga åtgärder som du kommer att utföra i din kod när du arbetar med Azure Storage.

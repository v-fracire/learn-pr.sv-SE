::: zon pivot = ”csharp” vi integrerar Azure Storage-klientbiblioteket i ditt .NET Core-konsolprogram.

Azure storage-klientbiblioteket för .NET distribueras med NuGet. Du kan lägga till den **WindowsAzure.Storage** paketet till dina .NET- eller .NET Core-program.

## <a name="add-the-azure-storage-nuget-package"></a>Lägga till NuGet-paketet Azure Storage

1. I Cloud Shell `cd` till katalogen PhotoSharingApp om du inte redan det.

1. Lägg till den **WindowsAzure.Storage** paketet till programmet.

    ```bash
    dotnet add package WindowsAzure.Storage
    ```

1. Detta resulterar i vissa konsolen aktiviteten medan klientbiblioteket och alla de nödvändiga beroendena laddas ned. När det är klart, gå vidare och skapa och köra appen igen för att se till att allt är klart.

    ```bash
    dotnet run
    ```

1. Som tidigare bör den utdata ”Hello World”!.

::: zone-end

::: zone pivot="javascript"

Nu ska vi integrerar den **Microsoft Azure Storage-klientbibliotek för Node.js och JavaScript** i ditt program.

Klientbibliotek för Node.js är tillgänglig via Node Package manager (NPM). Du kan lägga till den **azure-storage** paket till din **packages.json** fil.

> [!NOTE]
> Den **Microsoft Azure Storage-klientbibliotek för Node.js och JavaScript** är avsedd för serverprogram. Om du genomför JavaScript för klientsidan, kommer du vill använda den **Azure Storage-klientbibliotek för JavaScript**, som fungerar på samma sätt, men som skräddarsys för körs i en webbläsare.

## <a name="add-the-azure-storage-package"></a>Lägg till Azure Storage-paketet

1. I Cloud Shell `cd` till katalogen PhotoSharingApp om du inte redan det.

1. Lägg till den **azure-storage** paketet till programmet. Se till att ange den `--save` alternativet så att den finns kvar att **packages.json**.

    ```bash
    npm install azure-storage --save
    ```

1. Detta resulterar i vissa konsolen aktiviteten medan klientbiblioteket och alla de nödvändiga beroendena laddas ned. När det är klart, gå vidare och skapa och köra appen igen för att se till att allt är klart.

    ```bash
    node index.js
    ```

1. Som tidigare bör det utdata ”Hello, World”!

::: zone-end

Nu när vi har de nödvändiga bibliotek på plats kan vi titta på vanliga uppgifter i din kod att arbeta med Azure storage.

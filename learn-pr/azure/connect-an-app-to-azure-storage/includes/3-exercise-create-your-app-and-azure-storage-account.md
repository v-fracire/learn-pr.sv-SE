I den här övningen skapar du en .NET Core-konsolapp och ett Azure Storage-konto.

## <a name="create-a-net-core-console-application"></a>Skapa en .NET Core-konsolapp

Vi kommer att skapa appen med Visual Studio 2017.

1. Öppna Visual Studio 2017 och välj menyalternativet **Arkiv** > **Nytt** > **Projekt**.

1. I kategorin **Visual C# - .NET Core** väljer du **Konsolapp (.NET Core)**, anger ett namn för appen och väljer **OK**.

  ![Skärmbild av Visual Studio 2017 som visar dialogrutan för nytt projekt med .NET Core, Konsolapp och namnfält markerat.](..\media-draft\3-new-console-app.png)

## <a name="create-an-azure-storage-account"></a>Skapa ett Azure-lagringskonto

För att ansluta vår app till en Azure storage-resurs, måste vi först skapa ett lagringskonto.

1. Längst upp till vänster på Azure portal, Välj **skapa en resurs**.

1. Välj i rutan val av **Storage** > **lagringskonto – blob, fil, tabell, kö**.

  ![Skärmbild av Azure-portalen som visar skapa ett Resursblad med Storage kategori och Lagringskonto visas markerad.](..\media-draft\3-portal-storage-select.png)

1. En resursgrupp är en logisk behållare för att organisera relaterade resurser. Det val av resursgrupp, Välj **Skapa nytt** och ange ett namn för din nya resursgrupp.

1. Ange ett globalt unikt namn för din **lagringskontonamn**.

1. Lämna övriga alternativ till sina standardvärden och klicka på **granska + skapa**.

  ![Skärmbild av Azure-portalen som visar bladet skapa storage-konto med exempelinformation i de obligatoriska fälten; beskrivs resursen fält, namnfältet för Storage-konto och granska + skapa knappen är markerade.](..\media-draft\3-portal-storage-details.png)

1. Kontrollera informationen på granskningssidan det förväntade och på **skapa**.

Din resursgrupp etableras. Sedan skapas ditt lagringskonto i resursgruppen.
Dessutom har du skapat en app som nu kan konfigureras och anslutas till ditt lagringskonto.

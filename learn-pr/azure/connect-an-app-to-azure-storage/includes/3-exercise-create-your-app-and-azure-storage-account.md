I den här övningen skapar du en .NET Core-konsolapp och ett Azure Storage-konto.

## <a name="create-a-net-core-console-application"></a>Skapa en .NET Core-konsolapp

Vi kommer att skapa appen med Visual Studio 2017.

1. Öppna Visual Studio 2017 och välj menyalternativet **Arkiv** > **Nytt** > **Projekt**.

1. I kategorin **Visual C# - .NET Core** väljer du **Konsolapp (.NET Core)**, anger ett namn för appen och väljer **OK**.
  ![Ny App](..\media-draft\3-new-console-app.png)

## <a name="create-an-azure-storage-account"></a>Skapa ett Azure-lagringskonto

Innan vi kan ansluta till ett Azure Storage-konto måste vi först skapa lagringskontot.

1. Välj Skapa en resurs uppe till vänster i Azure-portalen.

1. Välj ”Storage” i urvalsfönstret som visas.

1. Välj ”Lagringskonto – blob, fil, tabell, kö” till höger i fönstret.
  ![val av portal](..\media-draft\3-portal-storage-select.png)

1. Du måste ange information om lagringskontot. Ange ett namn för lagringskontot.

1. En resursgrupp är en logisk container för resurser. Välj **Skapa ny** och ange ett namn på din resursgrupp.

1. Lämna standardvärdena för övriga alternativ och klicka på **Skapa**.
  ![Portalinformation](..\media-draft\3-portal-storage-details.png).

Din resursgrupp etableras. Sedan skapas ditt lagringskonto i resursgruppen.
Dessutom har du skapat en app som nu kan konfigureras och anslutas till ditt lagringskonto.

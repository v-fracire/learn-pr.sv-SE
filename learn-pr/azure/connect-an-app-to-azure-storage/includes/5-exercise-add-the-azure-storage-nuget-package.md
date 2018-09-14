I den här enheten integrerar du Azure Storage-klientbiblioteket i din .NET Core-konsolapp.

## <a name="add-the-azure-storage-nuget-package"></a>Lägga till NuGet-paketet Azure Storage

1. I Visual Studio högerklickar du på projektet i den **Solution Explorer** och välj **hantera NuGet-paket...**

1. En sida visas med de NuGet-paket som är installerade. Klicka på den **Bläddra** och ange `WindowsAzure.Storage` i sökfältet.

1. Välj den **WindowsAzure.Storage** paketet i sökresultaten. Till höger visas och anger den senaste versionen som standard. Klicka på knappen **Installera** för att lägga till en referens till paketet i projektet.

  ![Skärmbild av Visual Studio som visar NuGet-system för våra .NET-konsolapp med knappen Bläddra-fliken, sökfältet och installera knappen är markerad.](..\media-draft\5-find-package.png)

1. Visual Studio öppnar fönstret **Utdata** när paketen återställs. Det här tar några sekunder beroende på hastigheten i nätverket. I dialogrutan **Förhandsgranska ändringar** ser du vilka paket som ska läggas till i projektet. Klicka på **OK** när du har granskat vad kommer att installeras.

1. När den **godkännande av licensen** dialogrutan visas, granska licenser och klicka på **jag accepterar**.

Fler aktiviteter visas i utdatafönstret efter några sekunder och paketet kommer att läggas till projektet.

Du kan kontrollera att paketet har lagts genom att expandera den **beroenden** alternativ i ditt projekt i den **Solution Explorer**. Expandera den **NuGet** alternativet och hitta referenser till **WindowsAzure.Storage** i listan.

![Skärmbild av Visual Studio som visar Solution Explorer med beroenden > NuGet > WindowsAzure.Storage hierarki markerat.](..\media-draft\5-package-check.png)

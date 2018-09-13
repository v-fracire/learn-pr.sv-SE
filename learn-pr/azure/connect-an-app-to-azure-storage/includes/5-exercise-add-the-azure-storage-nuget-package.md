I den här enheten integrerar du Azure Storage-klientbiblioteket i din .NET Core-konsolapp.

## <a name="add-the-azure-storage-nuget-package"></a>Lägga till NuGet-paketet Azure Storage

1. Högerklicka på projektet och välj **Hantera Nuget-paket...**
  ![Manage NuGet packages]/' (..\media-draft\5-manage-nuget-packages.png)

1. En sida visas med de NuGet-paket som är installerade. Klicka på alternativet **Bläddra** och skriv **Storage** i sökfältet. Paketet **WindowsAzure.Storage** visas bland sökresultaten.

1. Välj paketet **WindowsAzure.Storage**. I rutan till höger visas den senaste versionen som standard (när detta skrivs är den senaste versionen 9.3.0). Klicka på knappen **Installera** för att lägga till en referens till paketet i projektet.
  ![Hitta paket](..\media-draft\5-find-package.png)

1. Visual Studio öppnar fönstret **Utdata** när paketen återställs. Det här tar några sekunder beroende på hastigheten i nätverket. I dialogrutan **Förhandsgranska ändringar** ser du vilka paket som ska läggas till i projektet. Klicka på **OK**.
  ![Förhandsgranska ändringar](..\media-draft\5-preview-changes.png)

1. En annan dialogruta visas där du får godkänna licensen. Klicka på **Jag accepterar**.
  ![Licens](..\media-draft\5-licence.png)

Efter några sekunder ser du ytterligare aktivitet i utdatafönstret i Visual Studio, och sedan har paketet har lagts till projektet.
Du kan kontrollera att paketet har lagts till genom att expandera alternativet **Beroenden** i projektet och sedan expandera alternativet **NuGet**. Du kommer att se en referens till **WindowsAzure.Storage**.

![paketkontroll](..\media-draft\5-package-check.png)

Det program du skapar är en plattformsoberoende mobilapp som kommunicerar med en Azure-funktion och delar din plats. I den här utbildningsenheten skapar du en tom mobilapp med Visual Studio och installera ett NuGet-paket med ett API för att hämta användarens plats.

## <a name="create-the-xamarinforms-project"></a>Skapa Xamarin.Forms-projektet

1. Välj *Arkiv -> Nytt -> Projekt* i Visual Studio.

1. Gå till det vänstra trädet och välj *Visual C# -> Plattformsoberoende*. Välj sedan *Mobilapp (Xamarin.Forms)* från panelen i mitten.

1. Ge lösningen namnet ”ImHere”.

1. Välj en lämplig plats för lösningen.

1. Klicka på **OK**.

    ![Dialogrutan Ny lösning](../media/2-new-solution-dialog.png)

1. I dialogrutan **Ny plattformsoberoende app** väljer du mallen *Tom app*.

1. I den här modulen skapar du en UWP-app, så avmarkera iOS och Android och lämna UWP markerat.

1. Som *Strategi för delning av kod* väljer du **.NET Standard**.

1. Klicka på **OK**.

    ![Dialogrutan Konfigurera ny lösning](../media/2-configure-solution-dialog.png)

I Visual Studio skapas två projekt åt dig – en UWP-app med namnet `ImHere.UWP` och .NET-standardbiblioteket `ImHere`. Xamarin.Forms-appar består av två delar – ett eller flera plattformsspecifika approjekt och ett (eller flera) .NET-standardbibliotek. De plattformsspecifika approjekten innehåller plattformsspecifik kod som behövs för att köra en app på motsvarande plattform. De här projekten startar sedan en Xamarin.Forms-app som definierats i ett plattformsoberoende .NET-standardbibliotek. Du skapar appen i plattformsoberoende kod, och när den körs översätts de användargränssnitt du skapar till relevanta plattformsspecifika UI-komponenter.

## <a name="adding-xamarinessentials"></a>Lägga till Xamarin.Essentials

Plattformarna UWP, Android och iOS har ett stort antal liknande funktioner där operativsystem och maskinvara används. Trots dessa likheter är API:erna mycket olika. Om du vill använda de här API:erna från plattformsoberoende kod måste du skriva plattformsspecifik kod i dina approjekt som du exponerar för dina .NET-standardbibliotek. [Xamarin.Essentials](https://docs.microsoft.com/xamarin/essentials/?azure-portal=true) är ett NuGet-paket som innehåller en plattformsoberoende abstraktion över ett antal av dessa API:er så att du inte behöver skriva plattformsspecifik kod. Detta inkluderar geografisk plats-API:er som du använder i din app för att hämta användarens plats.

1. Högerklicka på lösningen `ImHere` (den översta nivålösningen, inte `ImHere` .NET- standardprojektet) i lösningsutforskaren i Visual Studio och välj *Hantera NuGet-paket för Lösningen...*.

1. Välj fliken **Bläddra** och sök efter ”Xamarin.Essentials”. Det här paketet är för närvarande tillgängligt som förhandsversion av NuGet-paketet, så se till att kryssrutan *Inkludera förhandsversioner* är markerad.

    > [!TIP]
    > Kontrollera att *“inklude prelease”* är markerat om du inte ser Xamarin.Essentials NuGet-paketet. 

1. Välj NuGet-paketet **Xamarin.Essentials**.

1. Du kan se alla dina projekt i projektlistan till höger.

1. Klicka på knappen **Installera** för att installera NuGet-paketet. Du måste godkänna licensavtalet för att fortsätta.

    ![Lägga till NuGet-paketet Xamarin.Essentials i alla projekt i lösningen](../media/2-add-essentials-nuget.png)

## <a name="building-and-running-the-app"></a>Skapa och köra appen

1. Högerklicka på projektet `ImHere.UWP` i Lösningsutforskaren och välj *Ange som startprojekt*.

1. Ange versionskonfigurationen som **Felsökning**, plattformen som **x86** och att enheten ska köras **Lokalt**.

    ![Ställ in x86-felsökningskonfigurationen att köras lokalt](../media/2-debug-configuration.png)

1. Börja felsöka appen.

    ![Appen körs](../media/2-debuging-app.png)

## <a name="summary"></a>Sammanfattning

I den här utbildningsenheten har du skapat en ny plattformsoberoende Xamarin.Forms-mobilapp och lagt till NuGet-paketet Xamarin.Essentials. Härnäst får du lära dig att skapa mobilappens gränssnitt och logik.
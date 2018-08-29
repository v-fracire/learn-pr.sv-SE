Det program du skapar är en plattformsoberoende mobilapp som kommunicerar med en Azure-funktion och delar din plats. I den här utbildningsenheten skapar du en tom mobilapp med Visual Studio och installera ett NuGet-paket med ett API för att hämta användarens plats.

## <a name="create-the-xamarinforms-project"></a>Skapa Xamarin.Forms-projektet

1. Välj *Arkiv -> Nytt -> Projekt* i Visual Studio.

2. Gå till det vänstra trädet och välj *Visual C# -> Plattformsoberoende*. Välj sedan *Mobilapp (Xamarin.Forms)* från panelen i mitten.

3. Ge lösningen namnet ”ImHere”.

4. Välj en lämplig plats för lösningen.

    > Om du kör modulen lokalt i Windows och planerar att utveckla för Android ska du se till att sökvägen är så kort som möjligt. Det finns längdbegränsningar för sökvägar i SDK:t för Android, så rotsökvägen bör vara så kort som möjligt.

5. Klicka på **OK**.

    ![Dialogrutan Ny lösning](../media/2-new-solution-dialog.png)

6. I dialogrutan **Ny plattformsoberoende app** väljer du mallen *Tom app*.

7. I den här modulen skapar du en UWP-app, så avmarkera iOS och Android och lämna UWP markerat.

    > Om du kör det här lokalt kan du låta Android vara markerat eftersom SDK:t för Android installeras i samband med arbetsbelastningen *Mobil utveckling med .NET* i Visual Studio. Om du även vill skapa för iOS måste du parkoppla med en macOS-agent. Du kan läsa mer om det här i [iOS-dokumentationen för Xamarin](https://docs.microsoft.com/xamarin/ios/get-started/installation/windows/connecting-to-mac/).

8. Som *Strategi för delning av kod* väljer du **.NET Standard**.

9. Klicka på **OK**.

    ![Dialogrutan Konfigurera ny lösning](../media/2-configure-solution-dialog.png)

I Visual Studio skapas två projekt åt dig – en UWP-app med namnet `ImHere.UWP` och .NET-standardbiblioteket `ImHere`. Xamarin.Forms-appar består av två delar – ett eller flera plattformsspecifika approjekt och ett (eller flera) .NET-standardbibliotek. De plattformsspecifika approjekten innehåller plattformsspecifik kod som behövs för att köra en app på motsvarande plattform. De här projekten startar sedan en Xamarin.Forms-app som definierats i ett plattformsoberoende .NET-standardbibliotek. Du skapar appen i plattformsoberoende kod, och när den körs översätts de användargränssnitt du skapar till relevanta plattformsspecifika UI-komponenter.

## <a name="adding-xamarinessentials"></a>Lägga till Xamarin.Essentials

Plattformarna UWP, Android och iOS har ett stort antal liknande funktioner där operativsystem och maskinvara används. Trots dessa likheter är API:erna mycket olika. Om du vill använda de här API:erna i plattformsoberoende måste du skriva plattformsspecifik kod i dina approjekt som du exponerar för dina .NET-standardbibliotek. [Xamarin.Essentials](https://docs.microsoft.com/xamarin/essentials/) är ett NuGet-paket som ger en plattformsoberoende abstraktion över ett antal sådana API:er, inklusive de API:er för geoplatser som du kommer att använda i din app.

1. Högerklicka på lösningen `ImHere` i lösningsutforskaren i Visual Studio och välj *Hantera NuGet-paket för lösningen*.

2. Välj fliken **Bläddra** och sök efter ”Xamarin.Essentials”. Det här paketet är för närvarande tillgängligt som förhandsversion av NuGet-paketet, så se till att kryssrutan *Inkludera förhandsversioner* är markerad.

3. Välj NuGet-paketet **Xamarin.Essentials**.

4. Du kan se alla dina projekt i projektlistan till höger.

5. Klicka på knappen **Installera** för att installera NuGet-paketet. Du måste godkänna licensavtalet för att fortsätta.

    ![Lägga till NuGet-paketet Xamarin.Essentials i alla projekt i lösningen](../media/2-add-essentials-nuget.png)

    > Om du kör den här modulen lokalt och vill koda för Android måste du göra några ytterligare inställningar. Mer information finns i [Kom igång-dokumentet för Xamarin.Essentials](https://docs.microsoft.com/xamarin/essentials/get-started?context=xamarin%2Fios&tabs=windows%2Candroid).

## <a name="building-and-running-the-app"></a>Skapa och köra appen

1. Högerklicka på projektet `ImHere.UWP` i Lösningsutforskaren och välj *Ange som startprojekt*.

2. Ange versionskonfigurationen som **Felsökning**, plattformen som **x86** och att enheten ska köras **Lokalt**.

    ![Ställ in x86-felsökningskonfigurationen att köras lokalt](../media/2-debug-configuration.png)

3. Börja felsöka appen.

    ![Appen körs](../media/2-debuging-app.png)

## <a name="summary"></a>Sammanfattning

I den här utbildningsenheten har du skapat en ny plattformsoberoende Xamarin.Forms-mobilapp och lagt till NuGet-paketet Xamarin.Essentials. Härnäst får du lära dig att skapa mobilappens gränssnitt och logik.
Webbappen Alpine Ski House är igång och körs, men nu du behöver visa den för chefen. Du behöver uppdatera webbplatsen och publicera uppdateringarna till Azure.

## <a name="update-your-web-app"></a>Uppdatera din webbapp

### <a name="replace-the-boilerplate-code"></a>Ersätta den formaterade exempelkoden

1. I mappen **Pages** (Sidor) öppnar du filen **About.cshtml**
1. Längst ned i koden letar du rätt på `<p> Use this area to provide additional information. </p>`
1. Ersätt den formaterade exempeltexten med `Welcome to the Alpine Ski House!`
1. Spara filen
1. Öppna filen **About.cshtml.cs**
1. Ersätt strängen `Message` så att den blir **Alpine Ski House is the premier ski hill in Northeast.**
1. Spara filen.

### <a name="publish-your-updates"></a>Publicera dina uppdateringar

1. I Solution Explorer högerklickar du på projektet
1. Välj **Publicera**. Du bör ha ett alternativ som innehåller din [website]-web deploy
1. Välj din webbplats så skickar Visual Studio dina ändringar till Azure

### <a name="view-your-changes"></a>Visa dina ändringar

När du har publicerat ändringarna öppnar Visual Studio webbplatsen i webbläsaren. Navigera till sidan **Om** och bekräfta att ändringarna visas.

## <a name="congrats"></a>Grattis!

Du har uppdaterat din webbapp med hjälp av Visual Studio 2017.

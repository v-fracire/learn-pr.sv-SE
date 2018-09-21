I den här enheten skapar en ASP.NET Core-webbapp.

## <a name="create-a-new-web-project"></a>Skapa ett nytt webbprojekt

Navet bland .NET CLI-verktygen är kommandoradsverktyget `dotnet`. Med det här kommandot skapar du ett nytt ASP.NET Core-webbprojekt.

Till höger i Cloud Shell skapar du ett nytt ASP.NET Core MVC-program. Ge det namnet ”BestBikeApp”.

```bash
dotnet new mvc --name BestBikeApp
```

Kommandot skapar en ny mapp som heter ”BestBikeApp”, där projektet ska lagras. `cd` där, skapa sedan och kör programmet för att verifiera att det är klart.

```bash
cd BestBikeApp
dotnet run
```

Du bör se något som liknar följande:

```console
Using launch settings from /home/your-user/Documents/BestBikeApp/Properties/launchSettings.json...
...
Hosting environment: Production
Content root path: /home/your-user/BestBikeApp
Now listening on: http://localhost:5000
Application started.
```

Utdata beskriver situationen efter att du har startat appen: programmet körs och lyssnar på port 5000.

Om appen kördes på vår egen dator skulle vi ha kunnat öppna en webbläsare till http://localhost:5000. Eftersom vi är i Azure Cloud Shell måste vi distribuera appen till en plats med en offentlig slutpunkt. Den App Service-instans som vi skapade tidigare är perfekt för det.
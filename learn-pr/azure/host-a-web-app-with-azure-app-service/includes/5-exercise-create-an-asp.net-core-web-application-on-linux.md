I den här övningen skapar du en ASP.NET Core-webbapp med hjälp av .NET CLI på en Ubuntu-dator.

## <a name="aspnet-core-installation-on-linux-environment"></a>ASP.NET Core-installation i Linux-miljö

Gå till Microsofts [nedladdningssida för .NET](https://www.microsoft.com/net/download) och följ samma steg som på .NET Core SDK-sidan. De finns nedan. För att köra kommandona måste du öppna en ny **terminalinstans**.

### <a name="register-microsoft-key-and-feed"></a>Registrera Microsoft-nyckel och feed

Innan du installerar .NET måste du registrera Microsoft-nyckeln, registrera produktlagringsplatsen och installera nödvändiga beroenden. Detta behöver bara göras en gång per dator.

```console
~$ wget -q https://packages.microsoft.com/config/ubuntu/18.04/packages-microsoft-prod.deb
~$ sudo dpkg -i packages-microsoft-prod.deb
```

## <a name="install-the-net-sdk"></a>Installera .NET SDK

Uppdatera de produkter som är tillgängliga för installation och sedan installera .NET SDK.

Kör följande kommandon i kommandotolken:

```console
sudo apt-get install apt-transport-https
sudo apt-get update
sudo apt-get install dotnet-sdk-2.1
```

Under installationen kan du bli ombedd att ange lösenordet för kontot. Ange lösenordet och tryck på Retur för att fortsätta.

För att verifiera installationen skriver du följande:

```console
dotnet --version
```

Och de utdata som visas är:

```console
2.1.302
```

Nu när du har installerat .NET Core SDK har du även ASP.NET Core installerat. Nu går vi igenom hur du kan använda .NET CLI för att skapa ett nytt ASP.NET Core-projekt med hjälp av de kommandon vi just har lärt oss.

## <a name="open-a-terminal-window"></a>Öppna ett terminalfönster

Först behöver du öppna terminalfönstret. I terminalfönstret kan du köra kommandon, det fungerar ungefär som kommandotolken i Windows.

## <a name="create-a-new-web-project"></a>Skapa ett nytt webbprojekt

Navet bland .NET CLI-verktygen är drivrutinsverktyget *dotnet*. Med det här kommandot skapar du ett nytt ASP.NET Core-webbprojekt.

För att skapa ett nytt ASP.NET Core MVC-program behöver du bara ange följande kommandon:

```console
cd ~/Documents
mkdir BestBikeApp   # Hit Enter
cd BestBikeApp      # Hit Enter
dotnet new mvc      # Hit Enter
```

Med kommandona ovan navigerar du till rotmappen *Documents* och skapar sedan en ny mapp. Sedan kan du öppna den mappen. Kör .NET CLI-kommandot för att generera ett nytt ASP.NET MVC-program med alla de paket återställda:

```console
The template "ASP.NET Core Web App (Model-View-Controller)" was created successfully.
This template contains technologies from parties other than Microsoft, see https://aka.ms/aspnetcore-template-3pn-210 for details.

Processing post-creation actions...
Running 'dotnet restore' on /home/your-user/Documents/BestBikeApp/BestBikeApp.csproj...
...
```

Allt du behöver göra nu är att köra appen genom att ange det här kommandot:

```console
dotnet run
```

I *terminalen* visar *dotnet*-kommandot användbar information för den app som körs:

```console
Using launch settings from /home/your-user/Documents/BestBikeApp/Properties/launchSettings.json...
...
Hosting environment: Development
Content root path: /home/your-user/Documents/BestBikeApp
Now listening on: https://localhost:5001
Now listening on: http://localhost:5000
Application started. Press Ctrl+C to shut down.
```

Utdata beskriver situationen efter att du har startat appen: programmet körs och lyssnar på port 5001 och 5002 (via HTTPS).

För att kunna köra HTTPS lokalt på datorn i utvecklingsläge behöver du skapa och lita på ett **självsignerat certifikat**.

## <a name="create-a-self-signed-certificate"></a>Skapa ett självsignerat certifikat

Kör kommandot nedan för att generera ett självsignerat certifikat för utveckling:

```console
dotnet dev-certs https -v
```

När kommandot körs returneras följande:

```console
The HTTPS developer certificate was generated successfully.
```

## <a name="run-the-application"></a>Köra programmet

I den här demonstrationen använder vi webbläsaren Firefox.

Öppna webbläsaren och ange adressen: `http://localhost:5001` för att kontrollera att programmet körs korrekt.

![Skärmbild som visar en webbläsare med ASP.NET Core MVC-mallen för standardwebbsidan.](../media/5-asp-core-mvc-default-template.PNG)

> [!NOTE]
> Du måste **lägga till ett undantag** för programmets URL eftersom Firefox inte kunde verifiera det självsignerade certifikatet för utveckling.

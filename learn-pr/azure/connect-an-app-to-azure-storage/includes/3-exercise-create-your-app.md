Kom ihåg att vi arbetar på ett bilddelning program som använder Azure Storage för att hantera bilder och andra bits data Vi lagrar för våra användare.

::: zone pivot="csharp"

För att förenkla vårt scenario så att vi kan fokusera på Storage-API: er kan skapar vi ett nytt .NET Core-konsolprogram. Vi förutsätter också det alltid är ansluten till nätverket. Du bör dock alltid förstärka din app för att säkerställa nätverksfel inte påverkar användarupplevelsen eller leda till fel i programmet.

## <a name="create-a-net-core-application"></a>Skapa ett .NET Core-program

.NET core är en plattformsoberoende version av .NET som körs på macOS, Windows och Linux. Du kan installera verktyg lokalt eller använda Cloud Shell på höger sida av fönstret för att köra den nedanstående steg nedan.

1. Logga in på Cloud Shell eller öppna en kommandoradssession och skapa ett nytt .NET Core-konsolprogram med namnet ”PhotoSharingApp”. Du kan lägga till den `-o` eller `--output` flagga för att skapa appen i en viss mapp.

    ```bash
    dotnet new console --name PhotoSharingApp
    ```

1. Kör appen att se till att skapa och kör korrekt. Bör visas ”Hello, World”! i konsolen.

    ```bash
    cd PhotoSharingApp
    
    dotnet run
    ```
::: zone-end

::: zone pivot="javascript"

För att förenkla vårt scenario så att vi kan fokusera på Storage-API: er, skapar vi ett nytt Node.js-program som kan köras från konsolen. Vi förutsätter också det alltid är ansluten till nätverket. Du bör dock alltid förstärka din app för att säkerställa nätverksfel inte påverkar användarupplevelsen, eller leda till fel i programmet.

## <a name="create-a-nodejs-application"></a>Skapa ett Node.js-program

Node.js är ett populära ramverk för att köra JavaScript-appar. Används oftast för web apps, men du kan använda den för att köra logik från från kommandoraden. Om du har de verktyg som installerats lokalt kan köra du följande steg från en kommandorad. Du kan också använda Cloud Shell på höger sida av fönstret för att köra den stegen nedan.

1. Logga in på Cloud Shell eller öppna en kommandoradssession och skapa en ny mapp med namnet ”PhotoSharingApp”.

    ```bash
    mkdir PhotoSharingApp
    ```

1. Ändra till den nya mappen och skapa en **package.json** filen med den Node Package Manager (NPM) som innehåller en beskrivning av vår nya app.
    - Ge den namnet ”PhotoSharingApp”.
    - Du kan dra standardinställningarna för alla övriga frågor.

    ```bash
    cd PhotoSharingApp
    npm init
    ```

1. Skapa en ny källfil **index.js**, vilket är där vår kod hamnar.

    ```bash
    touch index.js
    ```

1. Öppna den **index.js** fil med en textredigerare. Om du använder Cloud Shell kan du skriva `code .` att öppna en textredigerare.

1. Placera följande program i den **index.js** fil.

    ```javascript
    #!/usr/bin/env node
    
    function main() {
        console.log('Hello, World!');
    }
    
    main();
    ```
1. Spara filen&mdash;du kan använda ”...”-menyn i det övre högra hörnet av Cloud Shell-redigeraren.

1. Kör appen att se till att den kan köras korrekt. Bör visas ”Hello, World”! i konsolen.

    ```bash
    node index.js
    ```

::: zone-end
Du kommer kanske ihåg att vi arbetar med ett bilddelningsprogram som använder Azure Storage till att hantera bilder och andra data som vi lagrar åt våra användare.

::: zone pivot="csharp"

För att förenkla vårt scenario så att vi kan fokusera på Storage-API:erna, skapar vi ett nytt .NET Core-konsolprogram. Vi förutsätter också det alltid är anslutet till nätverket. Du bör dock alltid förstärka din app för att säkerställa att nätverksfel inte påverkar användarupplevelsen, eller leder till fel i programmet.

## <a name="create-a-net-core-application"></a>Skapa ett .NET Core-program

.NET Core är en plattformsoberoende version av .NET som körs på macOS, Windows och Linux. Du kan installera verktyg lokalt eller använda Cloud Shell på höger sida i fönstret för att köra stegen nedan. 

1. Logga in på Cloud Shell eller öppna en kommandoradssession och skapa ett nytt .NET Core-konsolprogram med namnet ”PhotoSharingApp”. Du kan lägga till `-o`- eller `--output`-flaggan för att skapa appen i en viss mapp.

    ```bash
    dotnet new console --name PhotoSharingApp
    ```

1. Kör appen för att kontrollera att den skapas och körs korrekt. Den bör visa ”Hello, World!” i konsolen.

    ```bash
    cd PhotoSharingApp
    
    dotnet run
    ```
::: zone-end

::: zone pivot="javascript"

För att förenkla vårt scenario så att vi kan fokusera på Storage-API:erna, skapar vi ett nytt Node.js-program som kan köras från konsolen. Vi förutsätter också det alltid är anslutet till nätverket. Du bör dock alltid förstärka din app för att säkerställa att nätverksfel inte påverkar användarupplevelsen, eller leder till fel i programmet.

## <a name="create-a-nodejs-application"></a>Skapa ett Node.js-program

Node.js är ett populärt ramverk för att köra JavaScript-appar. Det används oftast för webbappar, men du kan använda det till att köra logik från kommandoraden också. Om du har de verktyg som installerats lokalt kan du köra följande steg från en kommandorad. Du kan också använda Cloud Shell på höger sida i fönstret för att köra stegen nedan.

1. Logga in på Cloud Shell eller öppna en kommandoradssession och skapa en ny mapp med namnet ”PhotoSharingApp”.

    ```bash
    mkdir PhotoSharingApp
    ```

1. Ändra till den nya mappen och skapa en **package.json**-fil med den Node Package Manager (NPM) som innehåller en beskrivning av vår nya app.
    - Ge den namnet ”PhotoSharingApp”.
    - Du kan använda standardinställningarna för alla övriga uppmaningar.

    ```bash
    cd PhotoSharingApp
    npm init
    ```

1. Skapa en ny källfil med namnet **index.js** som kommer att vara den plats där vår kod hamnar.

    ```bash
    touch index.js
    ```

1. Öppna filen **index.js** med ett redigeringsprogram. Om du använder Cloud Shell kan du skriva `code .` för att öppna ett redigeringsprogram.

1. Placera följande program i filen **index.js**.

    ```javascript
    #!/usr/bin/env node
    
    function main() {
        console.log('Hello, World!');
    }
    
    main();
    ```
1. Spara filen. Du kan använda ”...”-menyn i det övre högra hörnet av Cloud Shell-redigeringsprogrammet.

1. Kör appen för att kontrollera att den körs korrekt. Den bör visa ”Hello, World!” i konsolen.

    ```bash
    node index.js
    ```

::: zone-end
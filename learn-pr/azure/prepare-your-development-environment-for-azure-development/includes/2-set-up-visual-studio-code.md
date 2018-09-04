Visual Studio Code är ett populärt alternativ för att utveckla program för Azure. IDE är lätt och tar upp bara några megabyte lagringsutrymme jämfört med flera gigabyte för andra IDE:er. VS Code är plattformsoberoende och fungerar på Windows, Linux och macOS. Eftersom det är flexibelt kan du använda Visual Studio Code för att distribuera dina appar med hjälp av Azure CLI eller Azure App Service, antingen med en Docker-containeravbildning eller med Azure Functions med serverlös metod. 

## <a name="visual-studio-code"></a>Visual Studio Code

Visual Studio Code, eller bara VS Code, är ett kraftfullt men enkelt redigeringsprogram. Det stöder de flesta programmeringsspråk – faktiskt hundratals – och det är utformat för att ansluta till molntjänster.

För att kunna erbjuda stöd för alla plattformar (körs på Windows, Linux och MacOS) och ge en bärbar och agil upplevelse, innehåller grundinstallationen av VS Code en redigerare som identifierar en ständigt växande ström av syntaxer för programmeringsspråk. Det finns dock ingen kompilator. Kompilering är tänkt att äga rum i molnet eller via ett tillägg.

Du får perfekt inbyggt stöd för källkodskontroll med en källkodskontrollhanterare i Git (SCM). Det innebär att du måste installera Git-ramverket först.

## <a name="extension-model"></a>Tilläggsmodell

En av de mest kraftfulla funktionerna i VS Code är tilläggsmodellen. Den gör att funktioner från tredje part kan köras som en integrerad del av VS Code IDE och utökar funktionerna i IDE på nästan alla tänkbara sätt.

Ett tillägg är antingen skrivet i TypeScript eller JavaScript och kan även utvecklas i VS Code. Du kan använda Yeoman för att autogenerera tillägg och alla har stöd för IntelliSense, kodnavigering och fullständig felsökning.

Det finns tre allmänna kategorier av tillägg för VS Code: tillägg, språkservrar och felsökare. De två sistnämnda har ytterligare protokoll som gör att de kan erbjuda särskilda funktioner antingen i flera språk i redigeraren eller anslutas till felsökningen.

Tillägg som finns tillgängliga på marknadsplatsen för tillägg: språkstöd för Python, Go, C++ med flera, kodformateringsverktyg, t.ex. linters, verktyg för molnanslutning, som t.ex. Microsoft Azure, nya teman, kodformaterare och kodfragmentbibliotek. Alla dessa tillägg är tillgängliga på [VS Code Marketplace](https://marketplace.visualstudio.com/).

## <a name="azure-extensions"></a>Azure-tillägg

Många av tilläggen riktar sig mot Azure-funktioner och -produkter, och fler läggs till hela tiden. De riktar sig mot områden som Docker-stöd, prenumerationshantering, Azure CLI-verktyg, databasåtkomst, Azure Storage API-integrering, generella Azure-tillägg och mycket mer.

Varje Azure-tillägg lägger till en uppsättning VS Code-funktioner som underlättar och effektiviserar din utveckling med Azure-integreringspunkter.

## <a name="getting-vs-code-and-preparing-for-azure-development"></a>Hämta VS Code och förbered för Azure-utveckling

Det finns tre olika plattformar som har stöd för VS Code: Windows, macOS och Linux. Även om alla installeras från en nedladdningsbar fil, kan de skilja sig åt i installationen.

För att få VS Code att fungera på Windows måste du ladda ned filen för din version av Windows (32-bitars eller 64-bitars) och installera precis som alla andra Windows-program.

För macOS kan du också hämta filen och visa innehållet. Vi rekommenderar att du lägger till VS Code till LaunchPad och Dock.

Linux är lite mer komplext beroende på vilken distribution du valt. Antingen måste du hämta och installera VS Code på Debian och Ubuntu eller använda lagringsplatsen Yum på RHEL, Fedora, CentOS, openSUSE eller SLE. För andra distributioner kan det finnas community-versioner som inte stöds lika bra.

För att förbereda VS Code för Azure-utveckling på valfri plattform använder du tillägget Marketplace för att installera de tillägg som behövs för Azure. Om du arbetar med App Services kan du använda Azure App Service-tillägget, om du arbetar med Node.js behöver du Node Pack för Azure och så vidare.

## <a name="summary"></a>Sammanfattning

VS Code är ett perfekt verktyg för att utveckla och skapa program för Microsoft Azure. VS Code är ett enkelt plattformsoberoende IDE som i kombination med en omfattande uppsättning tillägg för att förbättra både effektivitet och stabilitet för appar, är perfekt för Azure-utveckling.

Här får du lära dig hur du skapar en Azure-webbapp med Azure-portalen.

## <a name="why-use-the-azure-portal"></a>Varför bör jag använda Azure-portalen?

Det första steget i att hantera webbprogrammet är att skapa en **webbapp** i din Azure-prenumeration.

Det finns flera sätt att skapa en **Azure-webbapp**. Du kan använda Azure-portalen, kommandoradsgränssnittet, ett skript eller en IDE.

Här använder vi portalen eftersom det är en grafisk upplevelse, vilket gör det till ett bra inlärningsverktyg. Portalen hjälper dig att upptäcka tillgängliga funktioner, lägga till ytterligare resurser och anpassa befintliga resurser.

## <a name="what-is-an-azure-web-app"></a>Vad är en Azure-webbapp?

Ett Azure-webbapp är en fullständigt hanterad plattform i Azure-miljön som är optimerad för hantering av webbappar, REST-API:er och mobila serverdelar.

Den här plattformen som en tjänst (PaaS), som tillhandahålls av Microsoft Azure, gör att du kan fokusera på att bygga medan Azure tar hand om infrastrukturen för att köra och skala dina program.

## <a name="how-to-create-an-azure-web-app"></a>Så skapar du en Azure-webbapp

När det är dags att hantera din egen app går du till Azure-portalen och skapar en ny **Resurs** av typen Webbapp. Genom att skapa en sådan resurs på Azure-portalen skapar du i själva verket en **container** eller en ritning som du kan använda som värd för alla webbaserade program som stöds av Azure, oavsett om det är ASP.NET Core, Node.js, PHP osv. Bilden nedan visar hur enkelt det är att konfigurera det ramverk/språk som används av appen.

![Inställningar för webbappen](../media-draft/2-web-app-settings.PNG)

Azure-portalen innehåller en mall för att skapa en ny webbapp. Den här mallen kräver följande fält:

- **Appnamn**: namnet på webbappen.
- **Prenumeration**: en giltig och aktiv prenumeration.
- **Resursgrupp**: en giltig resursgrupp. Avsnitten nedan förklarar i detalj vad en resursgrupp är.
- **OS**: operativsystemet. Alternativen är: Windows, Linux och Docker-containrar. I Windows kan du hantera valfri typ av program och från en mängd olika tekniker. Detsamma gäller för Linux-hantering, förutom det faktumet att du bara kan hantera ASP.NET Core-webbappar som använder .NET Core-ramverket via Linux medan de andra ASP.NET-apparna som använder det fullständiga .NET Framework måste hanteras via Windows-operativsystemet. Det sista alternativet är Docker-containrar, där du kan distribuera dina egna lokala Docker-containrar direkt via containrar som hanteras och underhålls av Azure. I princip kan du alltså hantera alla webbappar som använder en teknik med öppen källkod (PHP, ASP.NET Core osv.) via ett Linux-operativsystem.
- **App Service-plan/plats**: en giltig App Service-plan. Avsnitten nedan förklarar i detalj vad en App Service-plan är.
- **Application Insights**: du kan aktivera alternativet Application Insights och dra nytta av de verktyg för övervakning och mått som Azure-portalen tillhandahåller för att hålla koll på prestandan för dina appar.

Med Azure-portalen får du många kraftfulla verktyg för att hantera, övervaka och styra din webbapp.

### <a name="deployment-slots"></a>Distributionsfack

Med hjälp av Azure-portalen kan du enkelt lägga till **distributionsfack** till en webbapp. Du kan till exempel skapa ett distributionsfack för **mellanlagring** som du kan skicka koden till för att testa på Azure. När du är nöjd med koden kan du enkelt **växla** distributionsfacket för mellanlagring med produktionsplatsen. Du gör allt detta med några enkla musklick i Azure-portalen.

![Distributionsfack](../media-draft/2-deployment-slots.PNG)

### <a name="continuous-integrationdeployment-support"></a>Stöd för kontinuerlig integrering/distribution
Azure-portalen tillhandahåller färdig kontinuerlig integrering och distribution med Visual Studio Team Services, GitHub, Bitbucket, Dropbox, OneDrive och en lokal Git-lagringsplats som hanteras fullständigt av Azure. Du ansluter din webbapp med någon av ovanstående källor så gör Azure resten genom att automatiskt synkronisera koden och eventuella framtida ändringar av koden till webbappen. Med Visual Studio Team Services definierar du dessutom din egen bygg- och lanseringsprocess som avslutas med att din källkod kompileras, testerna körs, en version skapas och slutligen att versionen skickas till en webbapp varje gång du checkar in koden. Allt detta händer implicit utan manuella åtgärder.

![Konfigurera kontinuerlig integrering](../media-draft/2-continuous-integration.PNG)

### <a name="integrated-visual-studio-publishing-as-well-as-ftp-publishing"></a>Integrerad Visual Studio-publicering samt FTP-publicering

Utöver att kunna konfigurera kontinuerlig integrering/distribution för webbappen kan du alltid dra nytta av den nära integreringen med Visual Studio för att publicera webbappen till Azure via WebDeploy-teknik. Dessutom har Azure stöd för FTP, men FTP är inte den optimala lösningen för publicering eftersom det saknar vissa funktioner i WebDeploy för att välja endast de filer som har ändrats eller lagts i stället för att bara publicera alltihop i Azure!

### <a name="built-in-auto-scale-support-automatically-scale-updown-based-on-real-world-load"></a>Inbyggt stöd för automatisk skalning (skala automatiskt upp/ned baserat på verklig belastning)

I webbappen ingår möjligheten att skala upp/ned eller skala ut. Beroende på användningen av webbappen kan skala du upp/ned appen genom att öka/minska resurserna för den underliggande datorn som är värd för webbappen. Resurser kan vara ett antal kärnor eller tillgängligt RAM.

Att skala ut är å andra sidan möjligheten att öka antalet datorinstanser som kör webbappen.

## <a name="what-is-a-resource-group"></a>Vad är en resursgrupp?

En resursgrupp är en metod för att gruppera resurser och tjänster som är beroende av varandra, till exempel virtuella datorer, webbappar, databaser och mer för ett visst program och en miljö. Se den som en **mapp**, en plats för att gruppera element i appen.

Med resursgrupper kan du enkelt hantera och ta bort resurser, och du får ett sätt att övervaka, styra åtkomst till, etablera och hantera faktureringen för samlingar av resurser som krävs för att köra ett program eller som används av en klient.

## <a name="what-is-an-app-service-plan"></a>Vad är en App Service-plan?

En App Service-plan är en uppsättning fysiska resurser och tillgänglig kapacitet som du kan distribuera dina App Service-appar till.

Azure-portalen innehåller en mall för att skapa en ny App Service-plan. Den här mallen kräver följande grundläggande information:

- Region (USA, västra, USA, centrala, Europa, norra osv.)
- Skalningsantal (en, två, tre instanser osv.)
- Instansstorlek (liten, medel och stor)
- Lagerhållningsenhet – SKU (Kostnadsfri, Delad, Basic, Standard, Premium och nyligen Premium v2)

Webbappar, mobilappar, API-appar och funktioner i Azure App Service körs alla i en App Service-plan. Du kan distribuera ett obegränsat antal program till en App Service-plan, men det beror starkt på vilka typer av program som distribueras och vilka resurser som krävs för CPU-användning.

Du kan alltid använda bladet App Service-plan i Azure-portalen för att visualisera din CPU- och minnesanvändning så att du kan bestämma ditt behov av att skala eller flytta program till en annan App Service-plan.

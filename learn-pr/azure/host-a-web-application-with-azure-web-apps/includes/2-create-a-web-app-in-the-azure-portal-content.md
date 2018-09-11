Här får du lära dig att skapa en Azure-webbapp i Azure Portal.

## <a name="why-use-the-azure-portal"></a>Varför bör jag använda Azure Portal?

Det första steget när det gäller hantering av webbprogram är att skapa en **webbapp** i din Azure-prenumeration.

Du kan skapa Azure-webbappar på flera olika sätt. Du kan använda Azure Portal, Azure CLI, ett skript eller en IDE.

Här använder vi portalen eftersom det är en grafisk upplevelse, vilket gör den till ett bra inlärningsverktyg. I portalen får du hjälp med tillgängliga funktioner, att lägga till ytterligare resurser och att anpassa befintliga resurser.

## <a name="what-is-web-apps-in-azure"></a>Vad är Web Apps i Azure?

Web Apps är en helt hanterad databehandlingsplattform i Azure-miljön som är optimerad för hantering av webbappar, REST-API:er och mobila serverdelar.

Den här plattformen som en tjänst (PaaS) tillhandahålls av Microsoft Azure, så att du kan fokusera på att skapa apparna medan Azure hanterar infrastrukturen som krävs för att köra och skala om dina program.

## <a name="how-to-create-an-azure-web-app"></a>Så skapar du en Azure-webbapp

När det är dags att köra en egen app går du till Azure Portal och skapar en ny **resurs** av typen **Webbapp**. När du skapar en sådan resurs i Azure Portal skapar du i själva verket en **container** eller en ritning som du kan använda som värd för alla webbaserade program som stöds av Azure, oavsett om de är skrivna i ASP.NET Core, Node.js, PHP eller något liknande. I bilden nedan ser du hur enkelt det är att konfigurera appens ramverk/språk.

![Inställningar för webbappen](../media-draft/2-web-app-settings.png)

Azure Portal har en mall för att skapa nya webbappar. Mallen innehåller följande fält:

- **Appnamn**: namnet på webbappen.
- **Prenumeration**: en giltig och aktiv prenumeration.
- **Resursgrupp**: en giltig resursgrupp. Avsnitten nedan förklarar i detalj vad en resursgrupp är.
- **OS**: operativsystemet. Alternativen är: Windows, Linux och Docker-containrar. I Windows kan du köra valfri typ av program med en mängd olika tekniker. Detsamma gäller Linux-värdar, förutom att du bara kan skapa ASP.NET Core-webbappar som använder .NET Core-ramverket via Linux. Du måste köra andra ASP.NET-appar som använder hela .NET Framework i Windows. Det sista alternativet är Docker-containrar, där du kan distribuera egna lokala Docker-containrar direkt via containrar som hanteras och underhålls av Azure. I princip kan alltså alla webbappar som använder en teknik med öppen källkod (exempelvis PHP eller ASP.NET Core) köras i Linux.
- **App Service-plan/plats**: en giltig App Azure Service-plan. Avsnitten nedan förklarar i detalj vad en App Service-plan är.
- **Application Insights**: du kan aktivera alternativet Azure Application Insights om du vill hålla koll på prestandan för dina appar med övervakningsverktygen och måtten i Azure Portal.

I Azure Portal finns många kraftfulla verktyg för att hantera, övervaka och styra din webbapp.

### <a name="deployment-slots"></a>Distributionsfack

Med Azure Portal kan du enkelt lägga till **distributionsfack** till en webbapp. Du kan till exempel skapa ett distributionsfack för **mellanlagring** dit du kan skicka kod som du vill testa i Azure. När du är nöjd med koden kan du enkelt **växla** distributionsfacket för mellanlagring med produktionsfacket. Det här gör du med några enkla musklick i Azure Portal.

![Distributionsfack](../media-draft/2-deployment-slots.png)

### <a name="continuous-integrationdeployment-support"></a>Stöd för kontinuerlig integrering/distribution

Azure Portal har stöd för kontinuerlig integrering och distribution med Visual Studio Team Services, GitHub, Bitbucket, Dropbox, OneDrive och lokala Git-lagringsplatser som helt hanteras i Azure. Om du ansluter din webbapp till någon av ovanstående källor så gör Azure resten genom att automatiskt synkronisera koden och eventuella ändringar till webbappen. Med Visual Studio Team Services definierar du dessutom din egen kompilerings- och versionsprocess som kompilerar källkoden, kör tester, skapar en version och skickar versionen till en webbapp varje gång du checkar in kod. Allt detta händer implicit utan att du behöver göra någonting.

![Konfigurera kontinuerlig integrering](../media-draft/2-continuous-integration.PNG)

### <a name="integrated-visual-studio-publishing-and-ftp-publishing"></a>Integrerad Visual Studio-publicering och FTP-publicering

Förutom att kunna konfigurera kontinuerlig integrering/distribution för webbappen kan du alltid dra nytta av den nära integreringen med Visual Studio för att publicera webbappen till Azure via WebDeploy-teknik. Dessutom har Azure stöd för FTP, men FTP är inte den optimala lösningen för publicering eftersom det saknar vissa funktioner i WebDeploy för att välja endast de filer som har ändrats eller lagts i stället för att publicera alltihop i Azure!

### <a name="built-in-auto-scale-support-automatically-scale-updown-based-on-real-world-load"></a>Inbyggt stöd för automatisk skalning (skala automatiskt upp/ned baserat på verklig belastning)

I webbappen ingår möjligheten att skala upp/ned eller skala ut. Beroende på hur webbappen används kan du skala upp/ned appen genom att öka/minska resurserna för den underliggande datorn som är värd för webbappen. Resurser kan vara antal kärnor eller mängden tillgängligt RAM-minne.

Att skala ut är å andra sidan möjligheten att öka antalet datorinstanser som kör webbappen.

## <a name="what-is-a-resource-group"></a>Vad är en resursgrupp?

En resursgrupp är en metod för att gruppera resurser och tjänster som är beroende av varandra, till exempel virtuella datorer, webbappar, databaser med mera för ett visst program och en miljö. Se den som en **mapp**, en plats för att gruppera element i appen.

Med resursgrupper kan du enkelt hantera och ta bort resurser. De gör det också enklare att övervaka, kontrollera, komma åt, etablera och hantera fakturering för samlingar av resurser som krävs för att köra ett program eller används av en klient.

## <a name="what-is-an-app-service-plan"></a>Vad är en App Service-plan?

En App Service-plan är en uppsättning fysiska resurser och tillgänglig kapacitet som du kan distribuera dina App Service-appar till.

Azure-portalen innehåller en mall för att skapa en ny App Service-plan. Den här mallen kräver följande grundläggande information:

- Region (USA, västra, USA, centrala, Europa, norra osv.)
- Skalningsantal (en, två, tre instanser osv.)
- Instansstorlek (liten, medel eller stor)
- Lagerhållningsenhet – SKU (Kostnadsfri, Delad, Basic, Standard, Premium och nyligen Premium v2)

Funktionerna Web Apps, Mobile Apps och API Apps i Azure App Service och Azure Functions körs alla i en App Service-plan. Du kan distribuera ett obegränsat antal program till en App Service-plan, men hur många du använder beror mycket på vilka typer av program som distribueras och hur mycket processorkraft de behöver.

Du kan alltid använda din App Service-plan i Azure Portal till att visualisera din CPU- och minnesanvändning, så att du kan avgöra om du behöver skala ut eller flytta program till en annan App Service-plan.

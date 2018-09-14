Här lär du dig att skapa en webbapp i Azure App Service med Azure-portalen.

## <a name="why-use-the-azure-portal"></a>Varför bör jag använda Azure Portal?

Det första steget i som är värd för ditt webbprogram är att skapa en webbapp (en App Service-app) i din Azure-prenumeration.

Det finns flera sätt att skapa en webbapp. Du kan använda Azure Portal, Azure CLI, ett skript eller en IDE.

Här använder vi portalen eftersom det är en grafisk upplevelse, vilket gör den till ett bra inlärningsverktyg. I portalen får du hjälp med tillgängliga funktioner, att lägga till ytterligare resurser och att anpassa befintliga resurser.

## <a name="what-is-azure-app-service"></a>Vad är Azure App Service?

Azure App Service är en fullständigt hanterad plattform i Azure-miljön som är optimerad för hantering av web apps, REST API: er och mobila serverdelar.

Den här plattformen som en tjänst (PaaS) tillhandahålls av Microsoft Azure, så att du kan fokusera på att skapa apparna medan Azure hanterar infrastrukturen som krävs för att köra och skala om dina program.

## <a name="how-to-create-a-web-app"></a>Så här skapar du en webbapp

När det är dags att vara värd för din egen app du gå till Azure-portalen och skapa en **Webbapp**. Genom att skapa en **Webbapp** i Azure-portalen skapar du egentligen en uppsättning värdbaserade resurser i App Service, som du kan använda som värd för alla webbaserade program som stöds av Azure, oavsett om det är ASP.NET Core, Node.js, PHP, osv. I bilden nedan ser du hur enkelt det är att konfigurera appens ramverk/språk.

![Inställningar för webbappen](../media/2-web-app-settings.png)

Azure-portalen innehåller en mall för att skapa en webbapp. Mallen innehåller följande fält:

- **Appnamn**: namnet på webbappen.
- **Prenumeration**: en giltig och aktiv prenumeration.
- **Resursgrupp**: en giltig resursgrupp. Avsnitten nedan förklarar i detalj vad en resursgrupp är.
- **OS**: operativsystemet. Alternativen är: Windows, Linux och Docker-containrar. I Windows kan du köra valfri typ av program med en mängd olika tekniker. Detsamma gäller Linux-värdar, förutom att du bara kan skapa ASP.NET Core-webbappar som använder .NET Core-ramverket via Linux. Du måste köra andra ASP.NET-appar som använder hela .NET Framework i Windows. Det sista alternativet är Docker-containrar, där du kan distribuera egna lokala Docker-containrar direkt via containrar som hanteras och underhålls av Azure. I princip kan alltså alla webbappar som använder en teknik med öppen källkod (exempelvis PHP eller ASP.NET Core) köras i Linux.
- **App Service-plan/plats**: en giltig App Azure Service-plan. Avsnitten nedan förklarar i detalj vad en App Service-plan är.
- **Application Insights**: du kan aktivera alternativet Azure Application Insights om du vill hålla koll på prestandan för dina appar med övervakningsverktygen och måtten i Azure Portal.

I Azure Portal finns många kraftfulla verktyg för att hantera, övervaka och styra din webbapp.

### <a name="deployment-slots"></a>Distributionsfack

Med Azure portal, du kan enkelt lägga till **distributionsfack** till en App Service-webbapp. Du kan till exempel skapa ett distributionsfack för **mellanlagring** dit du kan skicka kod som du vill testa i Azure. När du är nöjd med koden kan du enkelt **växla** distributionsfacket för mellanlagring med produktionsfacket. Det här gör du med några enkla musklick i Azure Portal.

![Distributionsfack](../media/2-deployment-slots.png)

### <a name="continuous-integrationdeployment-support"></a>Stöd för kontinuerlig integrering/distribution

Azure-portalen innehåller out-of the box kontinuerlig integrering och distribution med Visual Studio Team Services, GitHub, Bitbucket, Dropbox, OneDrive eller en lokal Git-lagringsplats på din utvecklingsdator. Du ansluter din webbapp med någon av ovanstående källor och App Service gör resten av automatisk synkronisering kod och eventuella framtida ändringar på koden till webbappen. Med Visual Studio Team Services definierar du dessutom din egen kompilerings- och versionsprocess som kompilerar källkoden, kör tester, skapar en version och skickar versionen till en webbapp varje gång du checkar in kod. Allt detta händer implicit utan att du behöver göra någonting.

![Konfigurera kontinuerlig integrering](../media/2-continuous-integration.PNG)

### <a name="integrated-visual-studio-publishing-and-ftp-publishing"></a>Integrerad Visual Studio-publicering och FTP-publicering

Förutom att kunna konfigurera kontinuerlig integrering/distribution för webbappen kan du alltid dra nytta av den nära integreringen med Visual Studio för att publicera webbappen till Azure via WebDeploy-teknik. Dessutom har Azure stöd för FTP, men FTP är inte den optimala lösningen för publicering eftersom det saknar vissa funktioner i WebDeploy för att välja endast de filer som har ändrats eller lagts i stället för att publicera alltihop i Azure!

### <a name="built-in-auto-scale-support-automatic-scale-out-based-on-real-world-load"></a>Inbyggd automatisk skalning support (automatisk skala ut baserat på verkliga belastningen)

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
- SKU eller prisnivå (kostnadsfri, delad, Basic, Standard, Premium, PremiumV2 och isolerad)

Webbappar, mobilappar och API-appar som finns i Azure App Service, samt Azure Functions, alla kör i en App Service-plan. Du kan distribuera ett obegränsat antal program till en App Service-plan, men hur många du använder beror mycket på vilka typer av program som distribueras och hur mycket processorkraft de behöver.

Du kan alltid använda din App Service-plan i Azure Portal till att visualisera din CPU- och minnesanvändning, så att du kan avgöra om du behöver skala ut eller flytta program till en annan App Service-plan.

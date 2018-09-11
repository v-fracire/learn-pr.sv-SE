Nu undersöker vi hur vi kan mellanlagra vårt innehåll och distribuera med kort eller ingen avbrottstid.

## <a name="what-is-a-deployment-slot"></a>Vad är ett distributionsfack?

Ett distributionsfack är en **oberoende** webbapp med sitt eget innehåll, sin egen konfiguration och även ett unikt värdnamn. Därför fungerar den som andra webbappar.

> Azure debiterar dig inte extra för användning av distributionsfack!

Varje distributionsfack är tillgängligt från dess unika URL. Exempelvis kan vi antar att jag har lagt till en distributionsfack för **mellanlagring** med namnet `BESTBIKE`. URL:erna för programmet och distributionsfacket är:

- https://BESTBIKE.azurewebsites.net/
- https://BESTBIKE-staging.azurewebsites.net/

## <a name="why-are-deployment-slots-useful"></a>Varför är distributionsfack användbara?

När du distribuerar din webbapp på det traditionella sättet, oavsett om det är via FTP, WebDeploy, Git eller något annat sätt, förekommer vissa svagheter:

- När distributionen är klar startar webbappen kanske om, vilket orsakar en **kallstart** för programmet. Den första begäran till programmet blir långsammare.

- Potentiellt distribuerar du en *felaktig* version av webbappen, och du bör testa den (i produktion) innan du lanserar den till din klient.

Det är här som distributionsfack har betydelse. Du kan göra ändringar av din webbapp till ett distributionsfack för **mellanlagring** och testa ändringarna utan att påverka användare som använder distributionsfacket för **produktion**. När du är redo att flytta de nya funktionerna till produktion behöver du bara **växla** fack för mellanlagring och produktion **utan avbrottstid**.

En annan fördel med distributionsfack är att du kan **värma upp** ditt program i en mellanlagringsplats innan du växlar det till produktionsplatsen. Du undviker fördröjningarna från en **kallstart** och den långa initieringskoden.

Slutligen kan du **växla tillbaka** till föregående distributionsfack om du upptäcker att den nya versionen av programmet inte fungerar som väntat.

## <a name="what-is-automated-deployment"></a>Vad är automatiserad distribution?

Automatiserad distribution, eller kontinuerlig integrering, är en process som används för att skicka ut nya funktioner och felkorrigeringar i ett snabbt och upprepat mönster med minimal inverkan på slutanvändarna.

Azure har stöd för automatiserad distribution direkt från flera källor. Följande alternativ är tillgänglig:

- **Visual Studio Team Services (VSTS)**: du kan skicka koden till VSTS, bygga koden i molnet, köra testerna, generera en version utifrån koden och slutligen skicka koden till den Azure-webbapp.

- **GitHub**: Azure har stöd för automatiserad distribution direkt från GitHub. När du ansluter din GitHub-lagringsplats till Azure för automatiserad distribution distribueras eventuella ändringar som du har skickat till din produktionsgren på GitHub automatiskt åt dig.

- **Bitbucket**: tack vare likheterna med GitHub kan du konfigurera en automatiserad distribution med Bitbucket.

- **OneDrive**: du kan ansluta ditt OneDrive-konto till Web Apps så att när du ändrar en fil på OneDrive-konto identifierar Azure den automatiskt och hämtar alla ändringar av webbappfilerna. Det här är ett utmärkt alternativ för statiska webbplatser. Du kommer att få känslan av att arbeta med ett lokalt filsystem som visar alla eventuella ändringar på Azure smidigt och omedelbart.

- **Dropbox**: på ett sätt som liknar OneDrive kan du lagra dina webappfiler på ett Dropbox-konto och göra så att de automatiskt skickas och distribueras via en Azure-webbapp.

- **Extern lagringsplats**: du kan konfigurera automatiserad distribution med valfri extern Git-lagringsplats.

### <a name="non-automated-deployment-to-azure"></a>Icke-automatiserad distribution till Azure

Det finns några alternativ som du kan använda för att manuellt skicka din kod till Azure:

- **FTP/S**: FTP eller FTPS är ett traditionellt sätt att skicka koden till valfri värdmiljö. Även om det inte rekommenderas längre kan du fortfarande använda det.

- **WebDeploy/IDE**: du kan använda Visual Studio IDE för att publicera din webbapp till Azure. Visual Studio-publiceringsmekanismen kan utnyttja WebDeploy-teknik för att skicka din kod till Azure.

- **Git**: Azure har en **lokal Git-lagringsplats** för ditt program. Du kan **checka in** din kod till den direkt.

> I den här modulen utför vi en icke-automatiserad distribution med hjälp av Git.

## <a name="summary"></a>Sammanfattning

Azure tillhandahåller flera olika sätt att ladda upp din kod för att bättre passa in med ditt aktuella arbetsflöde. Du kan även använda distributionsplatser för att förhindra driftstopp för dina användare.

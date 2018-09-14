Nu ska vi se hur du kan mellanlagra innehållet och distribuera det med kort eller ingen avbrottstid.

## <a name="what-is-a-deployment-slot"></a>Vad är ett distributionsfack?

Ett distributionsfack är en **oberoende** app i App Service med dess egna innehåll, konfiguration och även ett unikt värdnamn. Därför kan fungerar den som andra appar i App Service.

Du kommer åt varje distributionsfack via dess unika webbadress. Låt oss anta att jag har lagt till ett distributionsfack för **mellanlagring** med namnet `BESTBIKE`. Programmets och distributionsfackets webbadresser är:

- https://BESTBIKE.azurewebsites.net/
- https://BESTBIKE-staging.azurewebsites.net/

## <a name="why-are-deployment-slots-useful"></a>Varför är distributionsfack användbara?

När du distribuerar en webbapp på det traditionella sättet, oavsett om det är via FTP, WebDeploy, Git eller något annat sätt, medför det vissa problem:

- När distributionen är klar appen kanske måste startas om, orsakar en **kallstart** för programmet. Den första begäran till programmet blir långsammare.

- I praktiken kan du distribuerar en *felaktig* version av din app och du bör testa den (i produktion) innan du publicerar den till din klient.

I de här situationerna kan du använda distributionsfack. Du kan göra ändringar i din app till en **mellanlagring** distribution fack och testa ändringarna utan att påverka användare som kommer åt den **produktion** distributionsplats. När du är redo att flytta de nya funktionerna till produktion behöver du bara **växla** facken för mellanlagring och produktion **utan avbrottstid**.

En annan fördel med distributionsfack är att du kan **värma upp** ditt program i en mellanlagringsplats innan du växlar det till produktionsplatsen. Du undviker fördröjningarna från en **kallstart** och den långa initieringskoden.

Slutligen kan du **växla tillbaka** till föregående distributionsfack om du upptäcker att den nya versionen av programmet inte fungerar som väntat.

## <a name="what-is-automated-deployment"></a>Vad är automatiserad distribution?

Automatiserad distribution, eller kontinuerlig integrering, är en process som används för att skicka ut nya funktioner och felkorrigeringar i ett snabbt och upprepat mönster med minimal inverkan på slutanvändarna.

Azure har stöd för automatiserad distribution direkt från flera källor. Följande alternativ är tillgänglig:

- **Visual Studio Team Services (VSTS)**: du kan skicka koden till VSTS, bygga koden i molnet, köra testerna, generera en version utifrån koden och slutligen skicka koden till den Azure-webbapp.

- **GitHub**: Azure har stöd för automatiserad distribution direkt från GitHub. När du ansluter din GitHub-lagringsplats till Azure för automatiserad distribution distribueras eventuella ändringar du push-överför till produktionsgrenen i GitHub automatiskt åt dig.

- **Bitbucket**: tack vare likheterna med GitHub kan du konfigurera en automatiserad distribution med Bitbucket.

- **OneDrive**: du kan ansluta ditt OneDrive-konto med App Service så att när du ändrar en fil på OneDrive-konto, Azure automatiskt identifiera och hämta alla ändringar på web app-filer. Det här är ett utmärkt alternativ för statiska webbplatser. Du kommer att få känslan av att arbeta med ett lokalt filsystem som visar alla eventuella ändringar på Azure smidigt och omedelbart.

- **Dropbox**: liknar OneDrive, du kan vara värd för din web app-filer i en Dropbox-konto och få dem automatiskt push och distribueras via en webbapp.

- **Extern lagringsplats**: du kan konfigurera automatiserad distribution med valfri extern Git-lagringsplats.

### <a name="non-automated-deployment-to-azure"></a>Icke-automatiserad distribution till Azure

Det finns några alternativ som du kan använda för att manuellt push-överföra din kod till Azure:

- **FTP/S**: FTP eller FTPS är ett traditionellt sätt att skicka koden till valfri värdmiljö. Även om det inte rekommenderas längre kan du fortfarande använda det.

- **WebDeploy/IDE**: du kan använda IDE:n Visual Studio till att publicera din webbapp i Azure. I publiceringsmekanismen i Visual Studio används WebDeploy-teknik till att skicka din kod till Azure.

- **Git**: Azure har en **lokal Git-lagringsplats** för ditt program. Du kan **checka in** din kod till den direkt.

> I den här modulen utför vi en icke-automatiserad distribution med hjälp av Git.

## <a name="summary"></a>Sammanfattning

I Azure finns flera olika sätt att ladda upp din kod så att det passar i ditt arbetsflöde. Du kan även använda distributionsfack så att inte användarna upplever några driftstopp.

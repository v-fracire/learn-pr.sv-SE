Azure-portalen är perfekt när du utför enkla uppgifter och vill få en snabb överblick över tillståndet för dina resurser. Men om du har uppgifter som upprepas varje dag eller till och med per timme, kan du med hjälp av kommandoraden och en uppsättning testade kommandon eller skript få jobbet gjort snabbare och undvika fel. 

Anta att du arbetar på ett företag som utvecklar Azure Web Apps. Det är program som körs i Azure, vilket innebär alla fördelar med automatiskt konfigurerad säkerhet, belastningsutjämning, hantering och så vidare. Du testar för närvarande en webbapp som genererar försäljningsprognoser, baserat på ett antal indata från olika databaser och andra datakällor. Utvecklarna använder Windows-, Linux- och Mac-datorer, samt en GitHub-lagringsplats för dagliga versioner av programmen. 

Som en del av testningen vill du jämföra appens prestanda vid olika datakällor och olika typer av anslutningar. Du har lagt märke till att när utvecklingsteamet använder Azure-portalen till att skapa en ny testinstans för appen använder de inte alltid exakt samma parametrar. Du tänker lösa problemet med hjälp av en uppsättning standardmässiga distributionskommandon för varje apptest, vilket kan automatiseras om det behövs och som fungerar på samma sätt i alla datorer som används av ditt programteam.

I den här modulen visar vi hur du hanterar Azure-resurser med Azure CLI. 

## <a name="learning-objectives"></a>Utbildningsmål

I den här modulen kommer du att göra följande:

- Installera Azure CLI på Linux, macOS och/eller Windows.
- Ansluta till en Azure-prenumeration med hjälp av Azure CLI.
- Skapa Azure-resurser med Azure CLI.

## <a name="prerequisites"></a>Nödvändiga komponenter  

- Erfarenhet av ett kommandoradsgränssnitt, till exempel PowerShell eller Bash
- Kunskaper om grundläggande Azure-begrepp, som exempelvis resursgrupper
- Erfarenhet av att administrera Azure-resurser med hjälp av Azure-portalen
- 
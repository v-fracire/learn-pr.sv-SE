Låt oss lägga vår kunskap om textanalys för att arbeta i en praktisk lösning. Vår lösning fokuserar på Känsloanalys av textdokument. Vi anger kontexten genom att beskriva de problem som vi vill hantera.

## <a name="manage-customer-feedback-more-efficiently"></a>Hantera feedback från kunder mer effektivt

Sociala medier är aktiv med talk av ditt företags produkt. Din feedback e-postalias är aktiv med kunder gärna dela med sig av din produkt.

Så är fallet med alla nya Start live genom ett mantra för att lyssna på kunderna. Dock har kan användas i din produkt gjort att hålla det här samarbetet enklare SA än klar. Detta är ett bra problem men inte samma.

Teamet hänger inte med mängden feedback längre. De behöver hjälp att sortera feedback så att problem som kan hanteras så effektivt som möjligt. Du har blivit ombedd att skapa en lösning för som chefsutvecklare i organisationen.

Låt oss titta på vissa viktiga krav:

|Krav  | Information  |
|---------|---------|
|Kategorisera feedback så att vi kan reagera på den.     |   Inte alla feedback är lika. Några är skärmen vittnesmålen. Annan feedback scathing kritik från en blir frustrerad över kund.  Du kan inte kanske se vad kunden vill ha i andra fall. <br/><br/>Som ett minimum kan med en indikation på åsikter eller tonen hos feedback hjälpa oss att kategorisera den.     |
|Lösningen ska skalas upp eller ned Uppfyll behov.    |   Vi är en start. Fasta kostnader är svåra att motivera och vi inte har kommit fram exakt mönstret för feedback trafik. Vi behöver en lösning som kan hantera ökningar av aktivitet, men kostnaden så lite som möjligt under tyst tider. <br/><br/> En serverlös arkitektur som faktureras i en förbrukningsplan är en bra kandidat i det här fallet.     |
| Skapa en Minimal lönsamma produkt (MVP), men gör att lösningen anpassningsbar.    | Idag vill vi att kategorisera feedback så att vi kan använda vår begränsade resurser för din feedback är viktig. Om en kund är vara frustrerade, vill vi bli underrättad och börja chatta till dem.  I framtiden, kan vi förbättra den här lösningen om du vill göra mer. En idé för en ny funktion är att undersöka nyckelfraser i feedback för att identifiera problemområden innan de når masslagringsenheter med våra kunder.   En annan tanken är att automatisera svar tillbaka till kunder som är positiv eller neutral. Även om de älskar oss vill vi att de ska vet vi fortfarande lyssnar på användarnas feedback. <br/><br/>En lösning som erbjuder en plug-and-play-arkitektur är en bra metod här. Vi kan till exempel använda köer som en form av factory rad. Du utför en uppgift och sedan placera resultatet i en kö för nästa del av systemet att välja den upp och processen.   |
|Leverera snabbt.     |   Vi hört allt det här en innan! Kom ihåg att den här lösningen är en MVP och vi vill testa vårt scenario snabbt. Om du vill leverera i hastighet och med kvalitet innebär skriva mindre kod. <br/><br/> Dra nytta av API för textanalys innebär att vi behöver träna en modell att identifiera sentiment.  Med Azure Functions och binda till köer deklarativt minskar den kod vi måste skriva.  En serverlös lösning innebär också att vi inte behöver bekymra dig om Serverhantering.   |

Vår Föreslagen lösning för alla behov i tabellen ovan erbjuder en glimt i hur du kan mappa krav till lösningar.  Låt oss nu se hur en lösning kan se ut baserat på Azure.

## <a name="a-solution-based-on-azure-functions-azure-queue-storage-and-text-analytics-api"></a>En lösning som baseras på Azure Functions, Azure Queue Storage och API för textanalys

Följande diagram är ett förslag för design till en lösning. Det använder tre kärnkomponenterna i Azure – Azure Queue Storage, Azure Functions och Microsoft Cognitive Services på Azure.

![Konceptuellt diagram av en feedback sortering arkitektur.](../media/proposed-solution.PNG)

Tanken är att dokument som innehåller Användarfeedback placeras i en kö som vi har namnet *ny feedback q* i föregående diagram. Till exempel ankomsten av ett meddelande som innehåller text-dokument i kön ska utlösa eller starta körning av funktion. Funktionen läser meddelanden som innehåller nya dokument från indatakön och skickar dem för analys till API för textanalys. Baserat på resultatet som API: et returnerar placeras ett nytt meddelande som innehåller dokumentet i en kö för utdata för vidare bearbetning.

Resultatet vi gå tillbaka för varje dokument är ett sentimentpoäng. Utgående köer används för att lagra feedback uppdelad i positiv, neutral och negativa. Förhoppningsvis negativt kön kommer alltid att vara tom! :-)   När vi har bucketas en utgående kö baserat på sentiment varje inkommande typ av feedback kan du föreställa dig att lägga till logik för att vidta åtgärder för meddelanden i varje kö.

Låt oss titta på ett flödesschema nästa för att se vad funktionen logiken som behöver göra.

![Flödesdiagram för logiken i Azure-funktion att sortera textdokument efter attityd till utgående köer.](../media/flow.PNG)

Vår logic är som en router. Det tar textinmatning och dirigerar den till en utgående kö baserat på attitydsresultatet av texten. Vi är beroende av API för textanalys. Medan logiken som verkar trivial, tar den här funktionen bort behovet av personer i teamet att analysera feedback manuellt.

## <a name="steps-to-implement-our-solution"></a>Steg för att implementera vår lösning

För att implementera lösningen som beskrivs i den här enheten, måste du utföra följande steg.

1. Skapa en funktionsapp som värd för vår lösning.

1. Titta efter attityd i inkommande meddelanden med hjälp av API för textanalys. Vi använder våra åtkomstnyckel från föregående övning och skriva kod för att skicka begäranden.

1. Lämna feedback för bearbetning av köer baserat på sentiment.

Vi går vidare till att skapa vår funktionsapp.
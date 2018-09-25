Låt oss använda vår kunskap om API för textanalys till att arbeta med en praktisk lösning. Vår lösning fokuserar på attitydanalys av textdokument. Vi anger kontexten genom att beskriva de problem som vi vill hantera.

## <a name="manage-customer-feedback-more-efficiently"></a>Hantera feedback från kunder mer effektivt

Det förekommer många diskussioner om ditt företags produkt på sociala medier. Ditt e-postalias för feedback är också aktivt med kunder som gärna delar med sig av sina uppfattningar om din produkt.

Precis som med alla nylanseringar är du mycket intresserad av att lyssna på kunderna. Dock har produktens framgång inneburit att det är lättare sagt än gjort att göra detta. Det är ett angenämt problem, men ändå ett problem.

Teamet hinner inte med att hantera mängden feedback längre. De behöver hjälp att sortera feedbacken så att eventuella problem kan hanteras så effektivt som möjligt. Eftersom du är chefsutvecklare i organisationen har du blivit ombedd att ta fram en lösning.

Låt oss titta på några viktiga krav:

|Krav  | Information  |
|---------|---------|
|Kategorisera feedbacken så att vi kan reagera på den.     |   Feedback kan se olika ut. Viss feedback höjer produkten till skyarna. Annan feedback kan vara svidande kritik från en frustrerad kund. Och i vissa fall kanske du inte förstår vad kunden vill ha. <br/><br/>Som ett minimum kan en indikation på attityden eller känsla i feedbacken hjälpa oss att kategorisera den.     |
|Lösningen bör kunna skalas uppåt eller nedåt för att anpassas efter efterfrågan.    |   Vi sätter igång. Fasta kostnader är svåra att ändra och vi har inte kommit fram till något exakt mönster för feedbacktrafiken. Vi behöver en lösning som kan hantera aktivitetstoppar, men kosta så lite som möjligt när det är lugnare. <br/><br/> En serverlös arkitektur som faktureras i en förbrukningsplan kan vara ett smart val i det här fallet.     |
| Skapa en MVP (Minimal Viable Product), men gör lösningen anpassningsbar.    | I dag vill vi kategorisera feedbacken så att vi kan använda våra begränsade resurser för den feedback som är viktig. Om en kund är frustrerad vill vi veta detta omedelbart och börja chatta med dem. I framtiden kan vi förbättra den här lösningen för att kunna göra mer. En idé för en ny funktion är att undersöka nyckelfraser i feedbacken och kunna identifiera problemområden innan de når flera kunder. En annan tanke är att automatisera svar till kunder som är positiva eller neutrala. Även om de älskar oss vill vi att de ska veta att vi ändå lyssnar på deras feedback. <br/><br/>En lösning med en Plug and Play-arkitektur är en bra metod här. Vi kan till exempel använda köer som en form av rad. Du utför en uppgift och placerar sedan resultatet i en kö för att nästa del av systemet ska kunna välja den och bearbeta den.   |
|Leverera snabbt.     |   Vi hört allt det här förut! Kom ihåg att den här lösningen är en MVP och vi vill testa vårt scenario snabbt. Om du vill leverera snabbt och med kvalitet måste du skriva mindre kod. <br/><br/> Med hjälp av API för textanalys behöver vi inte träna en modell att identifiera attityder. Med Azure Functions och att binda till köer deklarativt minskar den kod vi måste skriva. En serverlös lösning innebär också att vi inte behöver bekymra oss om serverhantering.   |

Vår föreslagna lösning för behoven i tabellen ovan ger en glimt i hur du kan mappa krav till lösningar. Låt oss nu se hur en lösning som baseras på Azure kan se ut.

## <a name="a-solution-based-on-azure-functions-azure-queue-storage-and-text-analytics-api"></a>En lösning som baseras på Azure Functions, Azure Queue Storage och API för textanalys

Följande diagram är ett förslag på design av en lösning. Det använder tre kärnkomponenterna i Azure: Azure Queue Storage, Azure Functions och Azure Cognitive Services.

![Konceptuellt diagram av en feedbacksorteringsarkitektur.](../media/proposed-solution.PNG)

Tanken är att textdokument som innehåller användarfeedback placeras i en kö med namnet *new-feedback-q* i föregående diagram. När ett meddelande som innehåller textdokumentet kommer till kön kommer det att utlösa, eller starta körningen av funktionen. Funktionen läser meddelanden som innehåller nya dokument från indatakön och skickar dem för analys till API för textanalys. Baserat på resultatet som API:n returnerar, placeras meddelanden som innehåller dokumentet i en utdatakö för vidare bearbetning.

Resultatet som vi får tillbaka för varje dokument är en attitydpoäng. Utdataköer används för att lagra feedback som delats upp i positiv, neutral och negativ. Förhoppningsvis kommer den negativa kön alltid att vara tom! :-) När vi har bucketgrupperat varje inkommande typ av feedback i en utdatakö som baseras på attityder, kan vi lägga till logik för att vidta åtgärder på meddelanden i varje kö.

Låt oss titta på ett flödesschema för att se vad funktionslogiken behöver göra.

![Flödesschemat för logiken i Azure-funktionen sorterar textdokument efter attityd till utdataköer.](../media/flow.PNG)

Vår logik är som en router. Den tar textindatan och dirigerar den till en utdatakö baserat på attitydpoängen för texten. Vi har ett beroende i API för textanalys. Även om logiken verkar trivial, innebär funktionen att personer i teamet inte behöver analysera feedbacken manuellt.

## <a name="steps-to-implement-our-solution"></a>Steg för att implementera vår lösning

Om du ska implementera lösningen som beskrivs i den här kursdelen, måste du utföra följande steg.

1. Skapa en funktionsapp som är värd för vår lösning.

1. Leta efter attityd i inkommande feedbackmeddelanden med hjälp av API för textanalys. Vi använder vår åtkomstnyckel från föregående övning och skriver kod för att skicka våra begäranden.

1. Lämna feedback för bearbetning av köer baserat på attityd.

Vi går vidare till att skapa vår funktionsapp.
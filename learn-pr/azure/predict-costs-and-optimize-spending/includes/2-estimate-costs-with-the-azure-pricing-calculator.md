Anta att du har blivit ombedd att skapa ett system i Azure, och fått i uppgift att uppskatta kostnaderna för att köra systemet de närmsta 12 månaderna. Du vet redan att priserna för Azure är helt transparenta och att du faktureras per månad för de tjänster du använder. Hur skulle du göra en sådan uppskattning utan att faktiskt distribuera och köra tjänsterna eller räkna ihop alla priser manuellt från sidan med prisinformation? 

## <a name="introducing-the-azure-pricing-calculator"></a>Introduktion till priskalkylatorn för Azure

Microsoft ville att kunderna enkelt skulle kunna göra egna uppskattningar, så vi utvecklade **priskalkylatorn för Azure**. Priskalkylatorn för Azure är ett kostnadsfritt webbverktyg där du kan ange olika Azure-tjänster och sedan ändra egenskaper och alternativ för tjänsterna. Du ser kostnaden per tjänst och totalkostnaden för hela uppskattningen.

Öppna en annan webbläsare eller flik och gå till [Azures priskalkylator](https://azure.microsoft.com/pricing/calculator/). Du ser tre flikar i priskalkylatorn:

1. **Produkter.** På den här fliken utförs det mesta av aktiviteten. Här visas alla Azure-tjänster, och du kan lägga till eller ta bort tjänster från din uppskattning.
2. **Uppskattningar.** På den här fliken visas dina sparade uppskattningar. Vi går igenom den här processen om en stund.
3. **Vanliga frågor och svar.** Precis som namnet antyder så hittar du svar på några vanliga frågor här.

Vi börjar med fliken **Produkter**. Här ser du hela listan med tjänstkategorier till vänster. Om du klickar på en kategori visas tjänsterna i kategorin. Det finns också en sökruta där du kan söka efter en viss tjänst bland samtliga tjänster. Om du klickar på tjänsten läggs den till i din uppskattning. Du kan lägga en eller flera tjänster, och du kan även lägga till flera förekomster av samma tjänst (till exempel flera virtuella datorer). 

När du har lagt till tjänsterna är nästa steg att prissätta dem. Om du rullar nedåt på sidan ser du anpassningsbar information om priser för tjänsten. För virtuella datorer kan du till exempel välja detaljer som påverkar priset för den virtuella datorn, som region, operativsystem och instansstorlek. Du ser en delsumma för tjänsten. Om du rullar ännu längre ned ser du en totalsumma för alla tjänster som ingår i uppskattningen. Bredvid totalsumman ser du knappar för att exportera, spara och dela uppskattningen.

## <a name="estimate-a-solution"></a>Uppskatta kostnaden för en lösning

I vårt ursprungliga scenario kan vi anta att systemet ska köras på två virtuella datorer i Azure och ansluta till en instans av Azure SQL Database. Vi vill också ha en layer 7-brandvägg som ger oss följande utökade funktioner för belastningsutjämning:

![Diagram över systemarkitekturen](../media-drafts/2-estimate-costs-architecture.png)

Vi kan använda priskalkylatorn i Azure till att se vad lösningen kommer att kosta och sedan exportera vår uppskattning för att dela den med teamet.

> [!NOTE]
> Se till att du har en ren kalkylator utan några poster i uppskattningen. Om du ser poster i uppskattningen klickar du på papperskorgsikonen för dem så att uppskattningen återställs.

Lägg till följande tjänster på fliken **Produkter** i Azures priskalkylator genom att klicka på dem:

- Virtuella datorer i kategorin Databearbetning
- Azure SQL-databas i kategorin Databaser
- Application Gateway i kategorin Nätverk

Vi kan konfigurera detaljer för varje tjänst på fliken **Beräkningar** så att vi får en tydlig bild över kostnaderna. Använd regionen **USA, västra** för alla resurser.

* **Virtuella datorer.** Det här är en ASP.NET-app, så vi måste använda en virtuell dator med **Windows som operativsystem**. Appen behöver inga stora mängder beräkningskraft, så välj instansstorleken **D2v3**. Vi behöver två virtuella datorer och de kommer att köras kontinuerligt (730 timmar/månad). Vi ka använder SSD-lagring för de virtuella datorerna och behöver bara en disk per virtuell dator med storleken **E10**, vilket totalt ger två diskar. 

* **SQL Database.** Som databas etablerar vi en **enkel databastyp** med **vCore-modellen**. Vi vill ha en Gen 4-databas för generell användning med 4 virtuella kärnor. Vi behöver 32 GB lagringsutrymme och kommer att förvara i genomsnitt 16 GB data. Vår bevarandeprincip blir åtta veckor, 12 månader och fem år. 

* **Application Gateway.** I Application Gateway använder vi WAF-nivån som skydd för miljön. Vi nöjer oss med bara två medelstora instanser eftersom belastningen inte kommer vara så hög. Vi räknar med att bearbeta 1 TB data per månad.

När du går igenom uppskattningen bör du se en summerad kostnad för varje tjänst du lagt till och en totalsumma för hela uppskattningen. I det här fallet bör uppskattningen vara ungefär **1 400,00 USD/månad**. Du kan prova att ändra några av alternativen och se hur uppskattningen ändras.

## <a name="share-and-save-your-estimate"></a>Dela och spara uppskattningen

Nu har vi en uppskattning av kostnaden för vår lösning. Vi kan spara den här uppskattningen så att vi kan återvända till den och göra ändringar senare, vi kan exportera den till Excel för vidare analys och vi kan dela uppskattningen i form av en webbadress. 

Om du vill exportera uppskattningen klickar du på `Export` längst ned. Då laddas uppskattningen ned i Excel-format (**.xlsx**) med alla tjänster du lade till.

Vi kan antingen dela Excel-kalkylbladet eller klicka på knappen `Share` i kalkylatorn. Då får du en webbadress till uppskattningen som du kan dela. Alla som får webbadressen kan se uppskattningen vilket gör det enkelt att dela den i hela teamet.

Om du har loggat in med ditt Azure-konto kan du spara uppskattningen och öppna den igen senare. Klicka på knappen **Spara**. Om du är inloggad bör du se ett meddelande om att uppskattningen har sparats. Om du inte är inloggad ser du ett meddelande om att logga in för att spara uppskattningen. När du har sparat uppskattningen rullar du tillbaka till sidans överkant och väljer fliken **Uppskattningar**. Här ser du din uppskattning. Om du inte längre behöver uppskattningen kan du ta bort den.

## <a name="summary"></a>Sammanfattning

Vi har tagit fram en uppskattning av kostnaden för en uppsättning Azure-tjänster utan att det kostat någonting. Vi behövde inte skapa några resurser, och resultatet är en uppskattning vi kan dela, analysera vidare eller återanvända senare. Du kan både skapa uppskattningar där du känner till precis vilka tjänster du kommer använda och jämföra hur olika tjänster skulle påverka totalkostnaden. Ett exempel är Microsoft SQL Server på en virtuell dator jämfört med Azure SQL Database. Nu ska vi titta närmare på hur vi kan se kostnader för tjänster vi redan har distribuerat.
Den bästa platsen för att starta är ofta i början. Vi ska börja med att ställa den grundläggande frågan ”Vad är Site Reliability Engineering”?
Det finns ett antal svar på den här frågan som cirkulerar, inklusive [det som ofta citeras](https://landing.google.com/sre/book/chapters/introduction.html) från personen som myntade termen (Ben Treynor Sloss på Google), men detta är det mest praktiska svaret vi kan erbjuda:

> Site Reliability Engineering är ett teknikområde som är avsett att hjälpa en organisation att uppnå lämplig nivå av tillförlitlighet i deras system, produkter och tjänster.

Senare kanske vi tar med några andra definitioner i bilden, men vi börjar här. Det finns två viktiga delar i den här definitionen som vi behöver bryta ned, som leder oss till frågan ”Varför spelar det någon roll”? .


## <a name="reliability"></a>Tillförlitlighet

I hjärtat (och i mitten av namnet ”SRE”) finns ordet tillförlitlighet (reliability). Definitionen säger inte ”lämplig nivå av prestanda” eller ”lämplig nivå av effektivitet” eller ”lämplig nivå av stabilitet” eller inte ens ”uppnå lämplig nivå av intäkter”. Det står ”lämplig nivå av tillförlitlighet”. Varför?

Låt oss titta på en snabb demonstration. Här är en skärmbild. Vad tycker du att den visar? Försök inte att gå vidare förrän du har en idé eller ger upp. Obs! Om det är svårt att identifiera mycket information i bilden nedan är det OK. Den återges perfekt i webbläsaren.

   ![Skärmbild 1](../media/02_blank-screenshot.png)

Den här bilden är en skärmbild av hur en PHP-app (utan andra tillagda stöd för felsökning) ser ut när den misslyckas. Du kan se ungefär så här för en Java-app:

   ![Skärmbild 2](../media/02_java-screenshot.png)

Varför tittar vi på de här exemplen? Var och en av dem representerar ett program som potentiellt krävde enorma mängder tid, energi och resurser för ett företag att skapa. Men om programmet inte är igång – om det inte fungerar när en kund behöver få åtkomst till det – om det inte är tillförlitligt – gör det ingen nytta för någon, särskilt inte för företaget. Brist på tillförlitlighet kan i själva verket skada ett företag (ryktet, ekonomin, avtalsenlighet, moral och så vidare kan skadas).

Därför är tillförlitlighet så viktigt, och därför väljer SRE att fokusera på det som en grundläggande egenskap, kanske den mes grundläggande egenskapen för tjänsten, systemet eller produkten. Tillförlitlighet kan omfatta ett antal saker (du lär dig mer om detta lite senare), men vi ska nu gå vidare till den andra viktiga delen av definitionen.

## <a name="appropriate-levels-of-reliability"></a>Lämpliga nivåer av tillförlitlighet

Du kanske inte lägger märke till det när du läser definitionen för första gången, men vi ska betona ett annat viktigt ord:

> Site Reliability Engineering är ett teknikområde som är avsett att hjälpa en organisation att uppnå *lämplig* nivå av tillförlitlighet i deras system, produkter och tjänster.

Varför är det ordet så viktigt?

En viktig observation som gjorts av SRE-världen är att det är mycket få system och tjänster som måste vara tillförlitliga till 100 %. Ett viktigt undantag är situationer där det handlar om liv och död, som när det gäller flygbranschen, medicinska enheter, etc.

Det finns i själva verket mycket få situationer där det ens är önskvärt. Ansträngningen och resurserna (och därmed kostnaden) som krävs för att uppnå större tillförlitlighet skjuter i höjden när större tillförlitlighet begärs. Med andra ord är det slöseri med tid och pengar att jaga efter tillförlitlighet du inte behöver. _Du vill uppnå lämplig nivå av tillförlitlighet i ditt system, dina tjänster och produkter._ 

Nivån måste matcha företagets behov och vara pragmatisk. Om dina kunder till exempel vill ansluta till dig via ett nätverk som inte är 100 % tillförlitligt (vi antar att det är igång 90 % av tiden), är det slöseri med tid och pengar att lägga ansträngning och pengar på att kontrollera att tjänsten är 95 % tillförlitlig. _Du vill uppnå lämplig nivå av tillförlitlighet i ditt system, dina tjänster och produkter._

SRE tar den här pragmatismen ett steg längre. Om vi tänker oss att det finns en önskvärd nivå av tillförlitlighet, finns det något vi bör göra om vi lyckas uppfylla eller överskrida den högsta nivån? Och vad händer om vi inte uppnår det?

Det här är precis sådana frågor som vi ska svarar på efter en avstickare för att ge lite kontext.

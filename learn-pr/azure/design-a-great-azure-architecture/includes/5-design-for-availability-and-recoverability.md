Kliniker i din organisation har låg tolerans för driftavbrott. Program måste kunna hantera fel med minimal påverkan på sina användare. Hur ser de till att deras program fortsätter att fungera, både för lokaliserade incidenter och storskaliga katastrofer? 

Här går vi igenom hur du får med tillgänglighet och återställningsbarhet i din arkitekturella utformning.

## <a name="availability-and-recoverability"></a>Tillgänglighet och återställning

I ett komplext program kan en mängd olika saker går fel oavsett skala. Enskilda servrar och hårddiskar kan misslyckas. Ett distributionsproblem kan oavsiktligt ta bort alla tabeller i en databas. Hela datacenter kan bli onåbara. En incident med en utpressningstrojan kan kryptera alla dina data.

Att designa för *tillgänglighet* fokuserar på att upprätthålla drifttid vid småskaliga incidenter och tillfälliga förhållanden som partiella nätverksavbrott. Du kan kontrollera att ditt program kan hantera lokaliserade fel genom att integrera hög tillgänglighet i varje komponent i ett program och eliminera enskilda felpunkter. En sådan design minskar också påverkan vid infrastrukturunderhåll. Design för hög tillgänglighet har vanligtvis som syfte att lösa problem snabbt och automatiskt.

Att designa för *återställning* fokuserar på återställning från dataförlust och större katastrofer. Återställning från dessa typer av incidenter sker inte automatiskt och kan leda till avbrott eller permanent förlorade data. Haveriberedskap handlar lika mycket om noggrann planering som utförande.

Att inkludera hög tillgänglighet och återställningsfunktioner i utformningen av din arkitektur skyddar din verksamhet från ekonomiska förluster från driftavbrott och dataförlust. De säkerställer att ditt rykte inte påverkas negativt av en förlust av förtroende från dina kunder.

## <a name="architecting-for-availability-and-recoverability"></a>Designa för tillgänglighet och återställning

Att designa för tillgänglighet och återställningsfunktioner garanterar att ditt program kan uppfylla de åtaganden som du gör mot dina kunder.

För tillgänglighet, identifiera de servicenivåavtal (SLA) som du har utlovat. Granska potentiella funktioner för hög tillgänglighet i ditt program i förhållande till dina SLA och identifiera var du har tillräcklig täckning och var du behöver göra förbättringar. Målet är att lägga till redundans till komponenterna i arkitekturen för att minska sannolikheten att uppleva ett avbrott. Exempel på designkomponenter med hög tillgänglighet inkluderar klustring och lastbalansering. Klustring ersätter en enskild virtuell dator med en uppsättning samordnade virtuella datorer. När en virtuell dator misslyckas eller blir oåtkomlig, kan tjänsterna växla över till en annan som kan hantera begäranden. Lastbalansering sprider ut begäranden över många instanser av en tjänst, identifierar misslyckade instanser och förhindrar att begäranden dirigeras till dem.

För återställningsfunktioner, utför du en analys som undersöker möjliga scenarion med dataförluster och större driftsavbrott. Analysen bör innehålla en förklaring av återställningsstrategier och kostnadsfördelen förhållandet för var och en. Den här övningen ger dig viktig insyn i din organisations prioriteringar och hjälper att tydliggöra rollen för ditt program. Resultaten bör innehålla programmets mål för återställningspunkt (RPO) och återställningstid (RTO) eller de högsta nivåerna av dataförlust och driftstopp som anses godtagbart med tanke på den investering som gjorts i haveriberedskap. När du har definierat RPO och RTO så kan du kan utforma säkerhetskopierings- och återställningsfunktioner i din arkitektur för att täcka dataförlust och replikering för att minimera avbrottstid.

Varje molnprovider erbjuder en uppsättning tjänster och funktioner som du kan använda för att förbättra programmets tillgänglighet och återställningsfunktioner. I den mån det går så bör du använda befintliga tjänster och metodtips och undvika att skapa dina egna.

Hårddiskar kan misslyckas, datacenter kan bli onåbara och hackers kan angripa. Det är viktigt att du bibehåller ett gott rykte mot dina kunder med hjälp av tillgänglighet och återställning. Tillgänglighet fokuserar på att bibehålla drifttid tack vare villkor som nätverksavbrott och återställningsfunktioner som fokuserar på hämtning av data efter en katastrof.

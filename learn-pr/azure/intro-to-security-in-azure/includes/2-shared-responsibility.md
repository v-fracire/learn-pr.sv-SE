När beräkningsmiljöer flyttas från kundstyrda datacenter till molndatacenter skiftar även ansvaret för säkerheten. Säkerhet är nu någonting som både molnproviders och kunder måste tänka på. För varje program och lösning är det viktigt att du förstår vad som är ditt eget ansvar och vad som Azure hanterar åt dig. 

## <a name="share-security-responsibility-with-azure"></a>Dela säkerhetsansvaret med Azure

Den första övergången är från lokala datacenter till IaaS (infrastruktur som en tjänst). Med IaaS använder du tjänster på lägsta möjliga nivå, där Azure skapar virtuella datorer och nätverk. På den här nivån är det fortfarande ditt eget ansvar att korrigera och skydda dina operativsystem och program, och att konfigurera nätverket så det är säkert. Contoso Shipping valde IaaS när de gick över till virtuella Azure-datorer från sina lokala fysiska servrar. Förutom driftfördelarna så slipper de även bekymra sig över att skydda nätverkets fysiska delar.

När du flyttar till PaaS (plattform som en tjänst) slipper du en mängd olika säkerhetsbekymmer. På den här nivån tar Azure hand om operativsystemet och de mest grundläggande programmen, som databashanteringssystem. Allt uppdateras med de senaste korrigeringarna och du kan integrera åtkomstkontroll med Active Directory. PaaS har dessutom en mängd driftfördelar. I stället för att skapa hela infrastrukturen och undernäten i din miljö manuellt kan du ”peka och klicka” i Azure Portal, eller köra automatiserade skript som aktiverar och kopplar ned avancerade säkerhetssystem och skalar om dem efter behov. Contoso använder Event Hub till att mata in telemetridata från sina lastbilar. De använder också en webbapp med en CosmosDb-serverdel i sina mobilappar. Alla de här tjänsterna är exempel på PaaS.

Med SaaS (programvara som en tjänst) outsourcar du nästan allt. SaaS är programvara som körs i en internetinfrastruktur. Koden styrs av leverantören men konfigureras för användning av kunden. Precis som många andra företag så använder Contoso Office 365, och det är ett utmärkt exempel på SaaS!

<!--TODO: replace with final media which was submitted for Design-for-security-in-azure -->
![shared_responsibility.png](../media-COPIED-FROM-DESIGNFORSECURITY/shared_responsibilities.png)

## <a name="a-layered-approach-to-security"></a>Säkerhet på flera nivåer

*Skydd på djupet* är en strategi som använder en serie mekanismer till att fördröja en attack som syftar till att få obehörig åtkomst till information. Varje nivå ger skydd, så om intrång sker på en nivå finns det ytterligare nivåer som förhindrar exponering. Microsoft använder säkerhet på flera nivåer både i våra fysiska datacenter och mellan olika Azure-tjänster. Målet med ett djupgående skydd är att skydda och förhindra att information blir stulen av personer som inte har behörighet till den.

Djupgående skydd kan visualiseras som en uppsättning koncentriska ringar, där de data som ska skyddas ligger i mitten. Varje ring är en extra säkerhetsnivå kring datan. Den här metoden innebär att man inte behöver förlita sig på en enda skyddsnivå. Den fördröjer angrepp och aviserar med telemetri som kan åtgärdas, antingen automatiskt eller manuellt. Låt oss ta en titt på var och en av nivåerna.

<!--TODO: replace with final media which was submitted for Design-for-security-in-azure -->
![Skydd på djupet](../media-COPIED-FROM-DESIGNFORSECURITY/defense_in_depth_layers_small.PNG)

### <a name="data"></a>Data

I nästan alla fall är angriparna ute efter data:

- Data som lagras i en databas
- Data som lagras på en disk i virtuella datorer
- Data som lagras i ett SaaS-program, till exempel Office 365
- Data som lagras i molnlagring

Ansvaret för att säkerställa att datan är skyddad ligger hos de som lagrar och styr åtkomsten till den. Det finns ofta regelverk som fastställer vilka kontroller och processer som behövs för att säkerställa sekretess, integritet och tillgänglighet för datan.

### <a name="applications"></a>Program

- Se till att programmen är säkra utan sårbarheter
- Lagra känsliga programhemligheter i ett säkert lagringsmedium
- Göra säkerhet till ett designkrav för all programutveckling

Om säkerhet introduceras i livscykeln för programutveckling kan antalet säkerhetsrisker i koden minskas. Uppmuntra alla utvecklingsteam att säkerställa att programmen är säkra som standard, och gör kraven på säkerhet icke förhandlingsbara.

### <a name="compute"></a>Beräkning

- Säker åtkomst till virtuella datorer
- Implementera slutpunktsskydd och se till att systemen är korrigerade och aktuella

Skadlig kod, okorrigerade system och felaktigt skyddade system gör din miljö sårbar för attacker. Fokus på denna nivå är att se till att dina beräkningsresurser är säkra och att du har rätt kontroller på plats för att minimera eventuella säkerhetsproblem.

### <a name="networking"></a>Nätverk

- Begränsa kommunikationen mellan resurser
- Neka som standard
- Begränsa inkommande Internet-åtkomst och begränsa den utgående åtkomsten när det är lämpligt
- Implementera en säker anslutning till lokala nätverk

På den här nivån ligger fokus på att begränsa nätverksanslutningen mellan alla resurser och endast tillåta det som krävs. Genom att begränsa kommunikationen kan du minska risken för lateral förflyttning i hela nätverket.

### <a name="perimeter"></a>Perimeternätverk

- Använd ett skydd mot distribuerade överbelastningsattacker till att filtrera storskaliga attacker, innan de orsakar en överbelastningsattack hos slutanvändarna
- Använd perimeterbrandväggar till att identifiera och varna vid skadliga attacker mot ditt nätverk

I nätverksperimetern handlar det om att skydda sig mot nätverksbaserade attacker mot dina resurser. Att identifiera dessa attacker, eliminerar deras inverkan och varna för dem är viktigt för att skydda ditt nätverk.

### <a name="policies--access"></a>Principer och åtkomst

- Styra åtkomsten till infrastruktur, ändra styrningen
- Använda enkel inloggning och multifaktorautentisering
- Granska händelser och ändringar

Nivån för princip och åtkomst handlar om att säkerställa att identiteter är skyddade, samt att enbart bevilja åtkomst för det som behövs och logga ändringar.

### <a name="physical-security"></a>Fysisk säkerhet

- Den första försvarslinjen är att fysiskt bygga in säkerhet och styra åtkomsten till maskinvaran i datacentret.

Avsikten är att ge ett fysiskt skydd mot åtkomst till tillgångarna. Det här gör att andra nivåer inte kan kringgås, samt att förluster eller stöld hanteras korrekt.

Vi har sett att Azure kan hjälpa dig med många säkerhetsproblem. Säkerheten är dock fortfarande ett **delat ansvar**. Hur mycket av ansvaret som ligger på oss själva beror på vilken modell vi använder i Azure.

Vi använder ringarna i *skydd på djupet* som riktlinje när vi överväger vilket skydd som är lämpligt för våra data och miljöer.
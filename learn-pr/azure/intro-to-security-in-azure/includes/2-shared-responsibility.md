När beräkningsmiljöer flyttas från kundstyrda datacentra till molndatacentra, övergår också ansvaret för säkerheten. Säkerhet är nu något problem delade både molnleverantörer och kunder. För varje program och en lösning är det viktigt att förstå vad ditt ansvar är och vad du ska hantera med Azure. 

> [!VIDEO https://www.microsoft.com/videoplayer/embed/RE2yEvj]

## <a name="share-security-responsibility-with-azure"></a>Dela security ansvar med Azure

Första övergången är från lokala Datacenter till infrastruktur som en tjänst (IaaS). Du är att använda den lägsta nivån servicen och frågar Azure för att skapa virtuella datorer (VM) och virtuella nätverk med IaaS. På den här nivån är det fortfarande ditt ansvar att korrigera och skydda ditt operativsystem och programvara samt konfigurera nätverket säker. Contoso leverans att utnyttja IaaS när de startar med virtuella Azure-datorer i stället för sina lokala fysiska servrar. Utöver de operativa fördelarna får de security fördelen med utlagd problem över skydda fysiska delar av nätverket.

Flytta till plattform som en tjänst outsources (PaaS) mycket säkerhetsproblem. På den här nivån Azure tar hand av operativsystemet och mest grundläggande programvara som system för databashantering. Allt uppdateras med de senaste uppdateringarna för säkerhet och kan integreras med Azure Active Directory för åtkomstkontroller. PaaS levereras också med en massa operativa fördelar. I stället för att skapa hela infrastrukturer och undernät för dina miljöer för hand ”peka och klicka på” i Azure portal eller kör automatiserade skript för att ta med komplexa, säker system uppåt och nedåt och skala dem efter behov. Leverans av Contoso använder Azure Event Hubs för att föra in telemetridata från sina lastbilar. De kan också använda en webbapp med en Azure Cosmos DB-serverdelen med sina mobila appar. Dessa tjänster är alla exempel på PaaS.

Du betalar nästan allt med programvara som en tjänst (SaaS). SaaS är programvara som körs med en Internetinfrastruktur. Koden är styrs av leverantören, men konfigurerats för användning av kunden. Precis som många företag använder Contoso leverans Office 365, vilket är ett bra exempel på SaaS!

<!--TODO: replace with final media which was submitted for Design-for-security-in-azure -->
![shared_responsibility.png](../media-COPIED-FROM-DESIGNFORSECURITY/shared_responsibilities.png)

## <a name="a-layered-approach-to-security"></a>Säkerhet på flera nivåer

*Skydd på djupet* är en strategi som använder en serie mekanismer till att fördröja en attack som syftar till att få obehörig åtkomst till information. Varje lager ger skydd så att om ett lager har överskrids kan ett lager för efterföljande redan är på plats för att förhindra ytterligare exponering. Microsoft gäller ett lager säkerhet, både i våra fysiska datacenter och mellan olika Azure-tjänster. Målet med djupgående skydd är att skydda och förhindra att information blir stulen av personer som inte har behörighet att komma åt den.

Djupgående skydd kan visualiseras som en uppsättning av koncentriska ringar, där de data som ska skyddas ligger i mitten. Varje ring är en extra säkerhetsnivå kring datan. Den här metoden innebär att man inte behöver förlita sig på en enda skyddsnivå. Den fördröjer angrepp och aviserar med telemetri som kan åtgärdas, antingen automatiskt eller manuellt. Låt oss ta en titt på var och en av dessa nivåer.

<!--TODO: replace with final media which was submitted for Design-for-security-in-azure -->
![Skydd på djupet](../media-COPIED-FROM-DESIGNFORSECURITY/defense_in_depth_layers_small.PNG)

### <a name="data"></a>Data

I nästan alla fall är angriparna ute efter data:

- Data som lagras i en databas
- Data som lagras på en disk i virtuella datorer
- Data som lagras på ett SaaS-program, till exempel Office 365
- Data som lagras i molnlagring

Ansvaret för att säkerställa att datan är skyddad ligger hos de som lagrar och styr åtkomsten till den. Det finns ofta regelkrav som bestämmer kontroller och processer som behövs för att säkerställa sekretess, integritet och tillgänglighet för data.

### <a name="application"></a>Program

- Se till att programmen är säker och kostnadsfritt sårbarheter.
- Store känsliga programhemligheter i en säker lagringsmedium.
- Kontrollera säkerhet en designkravet för alla programutveckling.

Om säkerhet introduceras i livscykeln för programutveckling kan antalet säkerhetsrisker i koden minskas. Rekommenderar att alla utvecklingsteam för att säkerställa att programmen är säkert som standard, och som de gör säkerhetskrav icke-negotiable.

### <a name="compute"></a>Beräkning

- Säker åtkomst till virtuella datorer.
- Implementera endpoint protection och hålla system korrigerad och aktuella.

Skadlig kod, okorrigerade system och felaktigt skyddade system öppnar din miljö för attacker. Fokus i detta skikt som ligger på att se till att dina beräkningsresurser är säkra och att du har rätt kontroller på plats för att minimera säkerhetsproblem.

### <a name="networking"></a>Nätverk

- Begränsa kommunikation mellan resurser.
- Neka som standard.
- Begränsa inkommande internet-åtkomst och begränsa utgående, där det är lämpligt.
- Implementera säkra anslutningar till lokala nätverk.

På den här nivån ligger fokus på att begränsa nätverksanslutningen mellan alla resurser för att möjliggöra bara vad som krävs. Genom att begränsa kommunikationen kan du minska risken för lateral förflyttning i hela nätverket.

### <a name="perimeter"></a>Perimeternätverk

- Använda datorn för protection service (DDoS) för att filtrera storskaliga attacker innan de kan orsaka en denial of service för slutanvändare.
- Använd perimeter brandväggar för att identifiera och varna på skadliga attacker mot ditt nätverk.

I nätverksperimetern handlar det om att skydda sig mot nätverksbaserade attacker mot dina resurser. Att identifiera dessa attacker, eliminerar deras inverkan och varna för dem är viktigt för att skydda ditt nätverk.

### <a name="policies--access"></a>Principer och åtkomst

- Kontrollera åtkomsten till infrastruktur och ändra kontroll.
- Använd enkel inloggning och Multi-Factor authentication.
- Granska ändringar och händelser.

Princip- och åtkomsthantering lagret är allt om att säkerställa identiteter är säkra, åtkomst beviljas är bara det som krävs och ändringar loggas.

### <a name="physical-security"></a>Fysisk säkerhet

- Är den första försvarslinjen fysiska att bygga in säkerhet och kontrollera åtkomst till maskinvara för databehandling i datacentret.

Avsikten är att tillhandahålla fysiska skydd mot åtkomst till tillgångar fysiska säkerheten. Detta säkerställer att andra nivåer inte kan kringgås, samt att förlust eller stöld hanteras korrekt.

Vi har sett att Azure hjälper mycket med dina säkerhetsfrågor. Men säkerhetsfrågorna är fortfarande en **ansvar som delas**. Hur mycket av det ansvaret infaller oss beror på vilken modell som vi använder med Azure.

Vi använder den *skydd på djupet* ringar som en riktlinje för överväger vilka skydd är lämpliga för våra data och miljöer.
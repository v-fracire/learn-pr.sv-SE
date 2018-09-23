Det finns inte någon ”enkel knapp” som kan användas för all säkerhet och det finns inte någon lösning som löser alla problem ur säkerhetssynpunkt. Låt oss anta att Lamna Healthcare har negligerat säkerheten i sin miljö. Företaget har insett att man måste göra fokuserade insatser på det här området. De vet inte exakt var de ska börja eller om de helt enkelt köpa en lösning rakt av som skyddar deras miljö. Företaget förstår att man behöver en helhetssyn, men man är osäkra på vad det egentligen innebär. Här kommer vi att identifiera nyckelbegrepp inom djupskydd, identifiera viktiga säkerhetstekniker och metoder som stöder en djupskyddsstrategi, och vi diskuterar hur de här begreppen används när du skapar dina egna Azure-tjänster.

#### <a name="defense-in-depth"></a>Skydd på djupet

> [!VIDEO https://www.microsoft.com/videoplayer/embed/RWjTfZ]

## <a name="a-layered-approach-to-security"></a>Säkerhet på flera nivåer

*Skydd på djupet* är en strategi som använder en serie mekanismer till att fördröja en attack som syftar till att få obehörig åtkomst till information. Varje nivå ger skydd, så att om en nivå överträds finns det en nivå till som förhindrar ytterligare exponering. Microsoft använder flernivåsäkerhet både i våra fysiska datacenter och mellan olika Azure-tjänster. Målet med ett djupgående skydd är att skydda och förhindra att information blir stulen av personer som inte har behörighet till den. De allmänna principer som används för att definiera en säkerhetsposition är sekretess, integritet och tillgänglighet (CIA).

- __Sekretess__ – Principen om lägsta behörighet. Begränsar informationsåtkomst till personer som uttryckligen beviljats åtkomst. Informationen omfattar skydd av användarlösenord, fjärråtkomstcertifikat och e-postinnehåll.

- __Integritet__ – Förhindra obehöriga ändringar av information i viloläge eller under överföring. Det är en vanlig metod som används vid dataöverföring där avsändaren skapar ett unikt fingeravtryck av datan med en enkelriktad hash-algoritm. Hashen skickas till mottagaren tillsammans med datan. Datans hash omberäknas och jämförs med originalet av mottagaren för att se till att data inte har tappats bort eller ändrats under överföringen.

- __Tillgänglighet__ – Se till att tjänsterna är tillgängliga för auktoriserade användare. DOS-attacker är en vanlig orsak till förlust av tillgängligheten för användare. Naturkatastrofer kan även innebära att systemdesignen förhindrar enskilda felpunkter och distribuerar flera instanser av ett program till geografiskt spridda platser.

## <a name="security-layers"></a>Säkerhetsnivåer

Djupgående skydd kan visualiseras som en uppsättning av koncentriska ringar, där de data som ska skyddas ligger i mitten. Varje ring är en extra säkerhetsnivå kring datan. Den här metoden innebär att man inte behöver förlita sig på en enda skyddsnivå. Den fördröjer angrepp och aviserar med telemetri som kan åtgärdas, antingen automatiskt eller manuellt. Låt oss ta en titt på var och en av nivåerna.

![En bild som visar djupskyddet med Data i mitten. Säkerhetsringarna kring dessa data är: program, beräkning, nätverk, perimeter, identitet och åtkomst samt fysisk säkerhet.](../media/defense_in_depth_layers_small.PNG)

### <a name="data"></a>Data

I nästan alla fall är angriparna ute efter data:

- Lagrade i en databas
- Lagrade på diskar i virtuella datorer
- Lagrade i ett SaaS-program, t.ex. Office 365
- Lagrade i molnlagring

Ansvaret för att säkerställa att data är skyddade ligger hos de som lagrar och styr åtkomsten till dessa data. Det finns ofta regelverk som fastställer vilka kontroller och processer som behövs för att säkerställa sekretess, integritet och tillgänglighet för datan.

### <a name="applications"></a>Program

- Se till att programmen är säkra utan sårbarheter
- Lagra känsliga programhemligheter i ett säkert lagringsmedium
- Göra säkerhet till ett designkrav för all programutveckling

Om säkerhet introduceras i livscykeln för programutveckling kan antalet säkerhetsrisker i koden minskas. Uppmuntra alla utvecklingsteam att säkerställa att programmen är säkra som standard. Gör säkerhetskraven icke förhandlingsbara.

### <a name="compute"></a>Beräkning

- Säker åtkomst till virtuella datorer
- Implementera slutpunktsskydd och se till att systemen är korrigerade och aktuella

Skadlig kod, okorrigerade system och felaktigt skyddade system gör din miljö sårbar för attacker. Fokus på denna nivå är att se till att dina beräkningsresurser är säkra och att du har rätt kontroller på plats för att minimera eventuella säkerhetsproblem.

### <a name="networking"></a>Nätverk

- Begränsa kommunikationen mellan resurser genom segmentering och åtkomstkontroller
- Neka som standard
- Begränsa inkommande Internet-åtkomst och begränsa den utgående åtkomsten när det är lämpligt
- Implementera en säker anslutning till lokala nätverk

På den här nivån ligger fokus på att begränsa nätverksanslutningen mellan alla resurser och endast tillåta det som krävs. Segmentera dina resurser och använd kontroller på nätverksnivå för att begränsa kommunikationen till enbart det som behövs. Genom att begränsa kommunikationen kan du minska risken för lateral förflyttning i hela nätverket.

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

Avsikten är att ge ett fysiskt skydd mot åtkomst till tillgångarna. Detta säkerställer att andra nivåer inte kan kringgås, samt att förlust eller stöld hanteras korrekt.

Varje nivå kan implementera en eller flera av CIA-åtgärderna.

|#|Ring|Exempel|Princip
|---|---|---|---|
|1|Data|Datakryptering i viloläge i Azure Blob Storage|Integritet|
|2|Program|Krypterade SSL/TLS-sessioner|Integritet|
|3|Beräkning|Tillämpa korrigeringar av operativsystem och programnivåer regelbundet|Tillgänglighet|
|4|Nätverk|Nätverkssäkerhetsregler|Sekretess|
|5|Perimeternätverk|DDoS-skydd|Tillgänglighet|
|6|Principer och åtkomst|Användarautentisering med Azure Active Directory|Integritet|
|7|Fysisk säkerhet|Biometriska åtkomstkontroller för Azures datacenter|Sekretess|

## <a name="shared-responsibilities"></a>Delat ansvar

När beräkningsmiljöer flyttas från kundstyrda datacentra till molndatacentra, övergår också ansvaret för säkerheten. Säkerhet är nu en angelägenhet för både molnprovidrar och kunder.

![En bild som visar hur molnleverantörer och kunder delar ansvarsområden under olika typer av beräkningstjänstimplementering: lokalt, infrastruktur som en tjänst, plattform som en tjänst och programvara som en tjänst. ](../media/shared_responsibilities.png)

## <a name="continuous-improvement"></a>Kontinuerlig förbättring

Hotlandskapet utvecklas i realtid och i massiv skala, därför blir en säkerhetsarkitektur aldrig färdig. Microsoft och våra kunder måste kunna svara på hoten smart, snabbt och i rätt omfattning.

[Azure Security Center](https://azure.microsoft.com/services/security-center/) ger kunderna en enhetlig säkerhetshantering och ett avancerat skydd mot hot som förstår och svarar på säkerhetshändelser lokalt och i Azure. Azure-kunderna har i sin tur ett ansvar för att kontinuerligt omvärdera och utveckla sin säkerhetsarkitektur.

## <a name="defense-in-depth-at-lamna-healthcare"></a>Skydd på djupet på Lamna Healthcare

Lamna Healthcare har satt tydligt fokus på ett djupgående skydd i alla IT-team. Eftersom organisationen är ansvarig för en stor mängd känsliga patientdata, inser de att en heltäckande metod är det bästa valet. 

De har skapat ett virtuellt team med representanter från varje IT-avdelning, som tillsammans med deras säkerhetsteam fokuserar på att förbättra detta i organisationen. De samarbetar med utbildare, tekniker och arkitekter om sårbarheter, hur man åtgärdar dem och de får vägledning medan projektet implementeras i organisationen.

De inser att den här insatsen aldrig har gjorts och de skapar principer, processer, tekniker och arkitektoniska granskningar som säkerställer att de ständigt ska försöka förbättra säkerheten.

## <a name="summary"></a>Sammanfattning

Vi har tittat på hur ett skydd på djupet kan se ut, vilka nivåerna i den här metoden är och vad varje nivå fokuserar på. Med den här säkerhetsmetoden i din arkitektur går du åt rätt håll för att säkerställa att du hanterar säkerhet på ett mer omfattande sätt i din miljö, i stället för att fokusera på en enda nivå eller teknik.
Anta att du arbetar på ett hälso- och sjukvårdsföretag. Du har äldre system, affärssystem och framtida planer för nya system. Du har hört att det finns fördelar med att använda molntjänster. Hur väljer du den bästa distributionsmodellen för olika lösningar? Ska det vara offentligt moln, privat moln eller hybridmoln?

## <a name="what-is-cloud-computing"></a>Vad är molntjänster?

Molntjänster innebär etableringen av tjänster och program på begäran via Internet. Servrar, program, data och andra resurser tillhandahålls som en tjänst. 

För användaren är informationen om tjänsterna abstrakt. Du kan snabbt etablera beräkningsresurser och använda tjänsten med minimal hantering. Det är fel att tro att molntjänster är ett datacenter som är tillgängligt via Internet. Molntjänster använder virtualisering, maskinvara och automatiserade processer för att kunna erbjuda en självbetjäningsupplevelse till kunder som liknar ett offentligt verktyg. 

Det finns tre distributionsmodeller för molntjänster: offentliga moln, privata moln och hybridmoln.

![Distributionsmodeller för molnet](../media-draft/2-cloud-deployment.png)

!!! Video TC-008 platshållare !!! 

> [!VIDEO https://channel9.msdn.com/Series/History/The-History-of-Microsoft-1995/player]

## <a name="public-cloud"></a>Offentligt moln

Offentliga moln är det vanligaste sättet att distribuera molntjänster på. Tjänsterna erbjuds via offentligt Internet och är tillgängliga för alla som vill köpa dem. Molnresurser, till exempel servrar och lagring, ägs och drivs av en extern molntjänstleverantör och tillhandahålls via Internet. Tjänster kan erbjudas utan kostnad eller mot en avgift på begäran, vilket innebär att kunderna betalar per användning för de CPU-cykler, det lagringsutrymme eller den bandbredd de nyttjar. Microsoft Azure är ett exempel på ett offentligt moln. 

Låt oss anta att din arbetsplats behöver en registreringswebbplats. Webbplatsen behöver kunna skalas och vara dynamisk när registreringen når höga nivåer vid olika tidpunkter under året. Dina kunder får åtkomst till webbplatsen från globala platser. Du kan använda det offentliga molnet för att automatiskt skala upp när du behöver möta efterfrågan vid registreringstoppar. När trafiken är låg kan webbplatsen skalas ned för att spara kostnader. Webbplatsen är dynamisk vid höga nivåer och du betalar bara för fler resurser när de behövs. Du kan också distribuera din webbplats i olika geografiska områden för att förbättra tillförlitligheten och svarstiderna.

Under utvecklingen av din webbplats vill utvecklarna skapa flera utvecklingsmiljöer för att påskynda utvecklingsprocessen. Utvecklarna kan använda det offentliga molnet för att snabbt etablera virtuella datorer i begränsade miljöer för att utveckla en lösning. När utvecklarna inte behöver miljön längre kan de ta bort den.

### <a name="why-public-cloud"></a>Varför ska man använda ett offentligt moln?

Offentliga moln kan distribueras snabbare än en lokal infrastruktur och med en nästan oändligt skalbar plattform. Samtliga anställda på ett företag kan använda samma program från valfritt kontor eller avdelning och med en valfri enhet så länge de kan ansluta till Internet. 

Exempel på varför du bör använda ett offentligt moln:

- **Användning av tjänsten på begäran, vid behov eller med prenumeration:** Med en modell på begäran eller med prenumeration betalar du för den del av processorn, lagringen och andra resurser som du använder eller reserverar.
- **Ingen inledande investering av maskinvara:** Inget krav på att köpa, hantera och underhålla lokal maskinvara och infrastruktur. Molntjänstleverantören har ansvar för all hantering och underhåll av systemet. 
- **Automatisering:** Etablera resurser snabbt i infrastrukturen med hjälp av en webbportal, skript eller automatiskt. 
- **Geografisk spridning:** Placera datan där den behövs utan att du behöver ha egna datacentra.
- **Minskat maskinvaruunderhåll:** Internetleverantören ansvarar för maskinvaruunderhållet.

## <a name="private-cloud"></a>Privat moln

Ett privat moln består av beräkningsresurser som uteslutande används av utvalda användare från ett företag eller organisation. Det kan finnas fysiskt i organisationens lokala datacenter eller det kan vara värdbaserat från tredje part. Det privata molnet är inte ett annat namn på ett traditionellt lokalt datacenter. Ett privat moln använder den lokala infrastrukturen och tjänster för att tillhandahålla liknande fördelar som det offentliga molnet. Det använder en abstraktionsplattform för att tillhandahålla *molnliknande* tjänster, till exempel Kubernetes-kluster eller en komplett molnmiljö som Azure Stack. Organisationen är ansvarig för inköp, konfiguration och underhåll av maskinvara. Kommunikationen mellan systemen sker vanligtvis i nätverksinfrastrukturen som företaget äger och underhåller. Det kan exempelvis vara ett privat internt nätverk eller en dedikerad fiberoptisk anslutning mellan byggnader.

Anta att du arbetar på sjuk- och hälsovårdsföretaget och du har ett program som används i ett av dina datacenter. Driftmiljön kan inte replikeras i det offentliga molnet. Du har fått ett nytt krav om att komma åt data på ett annat av dina datacenter. Den databas som innehåller datan måste vara kvar på den andra platsen på grund av juridiska skäl. Det här scenariot är ett privat moln. Organisationen har två datacentra. Du kan använda en offentlig moln-VPN via Internet för att ansluta dina datacentra. Dock anses scenariot vara ett privat moln, eftersom lösningen är privat i organisationen.

### <a name="why-private-cloud"></a>Varför ska man använda ett privat moln?

Ett privat moln kan ge mer flexibilitet åt en organisation. Organisationen kan anpassa molnmiljön för att uppfylla specifika affärsbehov. Eftersom resurserna inte delas med andra går det att ha höga kontroll- och säkerhetsnivåer. Med privata moln kan man dessutom få både skalbarhet och effektivitet.

Exempel på varför du bör använda ett privat moln:

- **Befintlig miljö:** En befintlig driftsmiljö som inte kan replikeras i det offentliga molnet. En stor investering i maskinvara och anställda med lösningskompetens. En stor organisation kan välja att anpassa sina beräkningsresurser.
- **Äldre program:** Verksamhetskritiska äldre program som det inte är enkelt att flytta fysiskt.
- **Datasuveränitet och säkerhet:** Politiska gränser och juridiska krav kan styra var datan ska finnas fysiskt.
- **Regelefterlevnad/certifiering:** PCI- eller HIPAA-efterlevnad. Certifierat lokalt datacenter.

## <a name="hybrid-cloud"></a>Hybridmoln

Ett hybridmoln är en beräkningsmiljö som kombinerar ett offentligt moln och ett privat moln genom att data och program kan delas. När beräknings- och databehandlingsefterfrågan varierar ger hybridmolntjänsterna företaget möjlighet att enkelt skala sina lokala infrastruktur till det offentliga molnet – utan att datacentra från tredje part får tillgång till samtliga data. Företagen får flexibilitet och beräkningskraft från det offentliga molnet för grundläggande och icke-känsliga beräkningsuppgifter, samtidigt som verksamhetskritiska program och data finns kvar lokalt bakom företagets brandvägg.

Med ett hybridmoln kan man eliminera behovet av att göra direkta investeringar i maskinvara för att hantera tillfälliga trafiktoppar. Man får även flexibilitet att hantera vilka resurser som ska vara lokala och vilka resurser som ska finnas i molnet. Företagen betalar bara för resurser som de använder i stället för att behöva köpa in, programmera och underhålla extraresurser och utrustning som kanske inte ens används under långa tidsperioder. Integrering sker vanligtvis via en säker VPN mellan molnleverantörer som Azure och lokala datacentra.

Anta att du arbetar på sjuk- och hälsovårdsföretaget och du har ett program där kunderna kan få åtkomst till sin hälsoinformation. Det finns ett regelverk som kräver att datan ska finnas på en fysisk plats. Kundwebbplatsen måste kunna vara dynamisk för de olika globala användarna.  En lösning kan därför vara att databasen hanteras på ett lokalt datacenter och webbplatsen finns i det offentliga molnet. En VPN-anslutning används mellan det lokala datacentrat och det offentliga molnet. Det här scenariot kan anses vara ett hybridmoln.

### <a name="why-hybrid-cloud"></a>Varför ska man använda hybridmoln?

Med hybridmoln kan din organisation styra och underhålla en privat infrastruktur för känsliga tillgångar. Det ger dig även flexibilitet att använda fler resurser i det offentliga molnet när du behöver dem. Med möjligheten att skala till det offentliga molnet betalar du för extra databehandlingskraft enbart när det behövs. Det kan också underlätta övergången till molnet. Du kan migrera gradvis genom att fasa in arbetsbelastningar över tid.

Exempel på varför du bör använda hybridmoln:

- **Befintlig maskinvaruinvestering:** Verksamheten kräver att du använder en befintlig driftsmiljö och maskinvara.
- **Juridiska krav:** Regelverk kräver att data ska finnas på en fysisk plats.
- **Unik driftsmiljö:** Det offentliga molnet kan inte replikera en äldre driftsmiljö.
- **Migrering:**: Flytta arbetsbelastningar till molnet över tid.

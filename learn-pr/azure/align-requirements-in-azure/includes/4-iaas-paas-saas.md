Databehandlingsresurser i molnet levereras som tre olika tjänstmodeller.

- Med **infrastruktur som en tjänst (IaaS)** får du en infrastruktur för omedelbar databehandling som du kan etablera och hantera via internet.
- Med **plattform som en tjänst (PaaS)** får du färdiga utvecklings- och distributionsmiljöer som du kan använda till att leverera egna molntjänster.
- **Programvara som en tjänst (SaaS)** levererar program över internet som en webbaserad tjänst.

När du väljer tjänstmodell ska du fundera på vem som ska ansvara för databehandlingsresursen. I ditt scenario kan du bestämma du hur ansvar du själv vill ha för administrationen.

![Modell med delat ansvar](../media/3-shared-responsibility.png)

## <a name="iaas"></a>IaaS

Infrastruktur som en tjänst är en infrastruktur för omedelbar databehandling som du etablerar och hanterar via internet. Med IaaS kan du snabbt skala om resurser för att möta efterfrågan och du betalar bara för det du använder. Med IaaS slipper du kostnaderna och besväret med att köpa och hantera egna fysiska servrar och annan infrastruktur i datacentret. Varje resurs erbjuds som en separat tjänstkomponent och du *hyr* resursen så länge du behöver den. IaaS är därför mycket flexibelt. Du kan etablera vanlig infrastruktur som virtuella datorer, lagring, virtuella undernät, brandväggar och VPN när du ska skapa din lösning. Du behöver inte hantera några fysiska servrar eller enheter. Du ansvarar dock för konfiguration och administration av komponenterna. Du måste till exempel konfigurera brandväggar, uppdatera de virtuella datorernas operativsystem, uppdatera databashanteringssystem och körningsmiljöer.

### <a name="common-scenarios"></a>Vanliga scenarier 

Anta att ditt hälsovårdsföretag måste köra en viss version av ett program. Programmet stöds bara i en viss version av ett operativsystem och du behöver bara en användare och en licens. Du kan skapa en virtuell dator med den programvara som behövs. Användaren kan ansluta till den virtuella datorn och använda programmet via Fjärrskrivbord.

Vi tänker oss ett annat scenario. Dina utvecklingsteam behöver flera unika utvecklingsmiljöer. Genom utvecklingscykeln måste de testa olika versioner av produkten. Utvecklarna kan etablera miljöer när det behövs. När en miljö inte längre behövs kan den enkelt tas bort.

Här är några andra vanliga scenarier:

**Driva webbplatser:** Om du vill ha bättre kontroll över driften av en webbplats kan det vara bättre att köra webbplatsen med IaaS istället för med en traditionell värdtjänst.

**Webbappar:** Med IaaS får du all infrastruktur du behöver för att hantera webbappar, som lagring, webb- och programservrar samt nätverksresurser. Organisationer kan snabbt distribuera webbappar med IaaS och enkelt skala om infrastrukturen när efterfrågan på appen ändras.

**Lagring, säkerhetskopiering och återställning:** Lagringshantering kan vara komplex och kräva stora kapitalinvesteringar, och du behöver kompetent personal som hanterar dina data så att du uppfyller olika juridiska krav. Med IaaS blir det enklare att hantera planering, administration, ändringar i efterfrågan och ökande lagringsbehov.

**Databehandling med höga prestanda:** Om du har en arbetsbelastning som kräver databehandling med höga prestanda kan du köra arbetsbelastningen i molnet och undvika direktkostnaden för maskinvara, och endast betala för användningen när den behövs. 

**Analys av stordata:** Om du har stora datamängder som kan innehålla viktiga mönster, trender och associationer så kan IaaS kan ge dig tillräcklig beräkningskraft för att bearbeta dina data och hitta dem.

### <a name="advantages"></a>Fördelar

**Undvik kapitalutgifter och minska dina löpande kostnader:** Med IaaS slipper du startkostnaden för att konfigurera och hantera ett lokalt datacenter, så det här är ett prisvärt alternativ för nystartade företag och företag som testar nya idéer. När du har bestämt dig för att lansera en ny produkt eller starta ett initiativ kan du ha infrastrukturen som behövs redo på bara några minuter eller timmar, snarare än de dagar eller veckor det skulle ta att konfigurera allt internt.

**Förbättra verksamhetens kontinuitet och haveriberedskap:** Det kan vara dyrt med hög tillgänglighet, verksamhetskontinuitet och haveriberedskap eftersom du behöver en hel del teknik och personal. Med rätt servicenivåavtal (SLA) kan IaaS minska de här kostnaderna och ge dig tillgång till program och data som vanligt vid ett haveri eller avbrott.

**Svara snabbare på ändrade förhållanden i verksamheten:** Med IaaS kan du snabbt skala om resurser när efterfrågan på programmet ändras, till exempel under storhelger, och sedan spara pengar genom att skala ned dem igen när aktiviteten minskar. Eftersom du inte behöver konfigurera infrastrukturen innan du kan utveckla och leverera appar får du ut dem till användarna snabbare med IaaS.

**Förbättra stabiliteten, tillförlitligheten och möjligheten till stöd:** Med IaaS behöver du inte underhålla eller uppgradera programvara och maskinvara eller felsöka problem med utrustningen. Med rätt avtal ser tjänstprovidern till att din infrastruktur är tillförlitlig och uppfyller serviceavtalen.

## <a name="paas"></a>PaaS

Plattform som en tjänst är en komplett miljö för utveckling och distribution i molnet. Med PaaS kan du skapa och distribuera allt från enkla molnbaserade appar till avancerade, molnkompatibla företagsprogram. Du köper resurserna från en molntjänstprovider enligt modellen betala per användning, och du har tillgång till dem via en säker internetanslutning. Precis som med IaaS så omfattar PaaS infrastruktur som servrar, lagring och nätverk. Dessutom kan även mellanprogram, utvecklingsverktyg och andra tjänster omfattas. PaaS har stöd för hela livscykeln för webbappar: skapande, testning, distribution, hantering och uppdatering. Med PaaS behöver du inte hantera programvarulicenser, mellanprogram eller tjänsternas infrastruktur. I stället hanterar du de program och tjänster du utvecklar, medan molntjänstprovidern vanligtvis hanterar allt annat.

### <a name="common-scenarios"></a>Vanliga scenarier

Låt oss anta att ditt hälsovårdsföretag behöver en webbplats för att beskriva en produkt. Utvecklarna vill använda PHP. Med PaaS kan utvecklarna *skapa en webbapp*. Detaljerna i infrastrukturen, som att skapa en virtuell dator, installera en webbserver och installera mellanprogram döljs via abstraktioner. Du behöver inte bry dig om vilket operativsystem appen körs i eller vilken fysisk maskinvara som krävs. Utvecklarna distribuerar filerna för webbplatsen till molnet och webbplats är tillgänglig på internet.

Vi tänker oss ett annat scenario. Ditt företag behöver en SQL-databas till dataanalysen i ett visst projekt. Du har inte den infrastruktur som behövs för det här. Du kan snabbt etablera en SQL Server-instans i molnet som uppfyller behoven i projektet. Dataanalytikerna kan ansluta till servern. SQL Server-databasen tillhandahålls som en tjänst. Därför behöver du inte bekymra dig om uppdateringar, säkerhetsuppdateringar eller optimering av den fysiska lagringen för läsning och skrivning.

Här är några andra vanliga scenarier:

**Utvecklingsramverk:** Med PaaS får du ett ramverk som utvecklare kan bygga vidare på när de ska skapa eller anpassa molnbaserade program. Ungefär som när du skapar ett Excel-makro så kan utvecklare skapa program med hjälp av inbyggda programkomponenter. Molnfunktioner som skalbarhet, hög tillgänglighet och flera klientorganisationer ingår vilket innebär mindre kodning för utvecklarna.

**Analys eller business intelligence:** Analysverktyg tillhandahålls som en tjänst och gör att du kan analysera och utvinna data. Organisationer kan hitta insikter och mönster som ger bättre prognoser, beslut kring produktutvecklingen, avkastning på investeringar och andra affärsbeslut.

### <a name="advantages"></a>Fördelar

Eftersom infrastrukturen levereras som en tjänst så har PaaS ungefär samma fördelar som IaaS. Funktionerna som mellanprogram, utvecklingsverktyg och andra affärsverktyg ger dock ytterligare fördelar:

**Kortare utvecklingstider:** PaaS-utvecklingsverktygen kan korta ned utvecklingstiden för nya program. Utvecklare kan använda förkodade programkomponenter som är inbyggda i plattformen, som komponenter för arbetsflöden, katalogtjänster, säkerhetsfunktioner och sökning. Med PaaS-komponenter kan utvecklingsteamet göra mer utan att du behöver anställa personal med rätt kompetens.

**Utveckla för flera plattformar:** En del tjänstproviders erbjuder utvecklingsalternativ för flera plattformar, som stationära datorer, mobila enheter och webbläsare, så att det går snabbare att utveckla plattformsoberoende appar.

**Använd avancerade verktyg till låg kostnad:** I modellen med betalning per användning kan individer och organisationer använda avancerade utvecklingsprogram samt verktyg för business intelligence och analys, som de inte skulle ha råd att köpa in direkt.

**Stöd för geografiskt utspridda utvecklingsteam:** Eftersom utvecklingsmiljön är tillgänglig via internet kan utvecklingsteamen samarbeta i projekt även när de befinner sig på olika platser.

**Hantera programmets livscykel effektivt:** Med PaaS får du alla funktioner du behöver för programmets hela livscykel: du kan skapa, testa, distribuera, hantera och uppdatera det i samma integrerade miljö.

## <a name="saas"></a>SaaS

Med programvara som en tjänst kan användarna ansluta till och använda molnbaserade appar via internet. Vanliga exempel är e-post, kalendrar och kontorsverktyg som Microsoft Office 365. Med SaaS får du en komplett programvarulösning som du köper från en molntjänstprovider enligt modellen betala per användning. Du *hyr* användningen av ett program för din organisation. Användarna ansluter till tjänsten via internet, vanligtvis med en webbläsare. Infrastruktur, mellanprogram, appens programvara och appdata ligger i tjänstproviderns datacenter. Tjänstprovidern hanterar både maskinvara och programvara, och med rätt serviceavtal ser du till att appen och dina data både är tillgängliga och skyddade. Med SaaS kan din organisation snabbt komma igång och köra en app till en minimal startkostnad.

Om du har använt en webbaserad e-posttjänst, som Outlook, Hotmail eller Yahoo! Mail så har du redan använt en typ av SaaS. Med de här tjänsterna loggar du in på ditt konto via internet, ofta med en webbläsare. E-postprogramvaran ligger i tjänstproviderns nätverk och där lagras även dina meddelanden. Du kan komma åt din e-post och lagrade meddelanden från en webbläsare på valfri dator eller internetansluten enhet.

### <a name="common-scenarios"></a>Vanliga scenarier

Låt oss anta att ditt hälsovårdsföretag behöver en CRM-lösning (Customer Relationship Management) för säljteamet. Teamet arbetar globalt. Du kan använda en SaaS CRM-provider och snabbt implementera en lösning för organisationens säljteam.

Du kan hyra produktivitetsappar för e-post, samarbete och kalenderfunktioner, eller avancerade affärsprogram som lösningar för CRM, resursplanering och dokumenthantering. Du betalar för användningen av apparna genom en prenumeration eller enligt användningsnivån.

### <a name="advantages"></a>Fördelar

**Få tillgång till avancerade program:** När du ska ge användarna tillgång till SaaS-appar behöver du inte köpa, installera, uppdatera eller underhålla någon maskinvara, programvara eller några mellanprogram. Med SaaS kan organisationer som inte har råd att köpa in, driftsätta och hantera infrastrukturen som behövs också få tillgång till avancerade företagsprogram som ERP- och CRM-lösningar.
Betala bara för det du använder. Du spara också pengar eftersom SaaS-tjänsten automatiskt skalas upp och ned med användningsnivån.

**Använd kostnadsfri klientprogramvara:** Användarna kan köra de flesta SaaS-appar direkt i sina webbläsare utan att behöva ladda ned och installera någon programvara. Du behöver inte köpa in eller distribuera någon klientprogramvaran till användarna.

**Gör enkelt personalen mer mobil:** Användarna kommer åt SaaS-appar och data från valfri internetansluten dator eller mobil enhet. Tjänstprovidern hanterar leveransen av tjänsten till enheterna.

**Använd appdata var du än befinner dig:** Eftersom data lagras i molnet kan användarna komma åt sin information från valfri internetansluten dator eller mobil enhet. När appdata lagras i molnet går heller inga data förlorade om en dator eller enhet skulle gå sönder.
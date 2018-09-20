Justeringar i serverkonfigurationen utförs ofta med utrustning i din lokala miljö. I det avseendet kan du se virtuella Azure-datorer som en förlängning av den här miljön. Du kan ändra konfiguration, hantera nätverk, öppna eller blockera trafik med mera via Azure Portal, Azure CLI eller Azure PowerShell-verktyg.

Vår server körs och Apache har installerats och används av sidor. Vårt säkerhetsteam uppmanar oss att låsa alla servrar och vi har inte gjort något åt den här virtuella datorn än. Vi har inte gjort något och Apache lyssnar på port 80. Låt oss utforska Azure-nätverkskonfigurationen för att se hur säkerhetssupport kan användas för att förstärka servern.

## <a name="opening-ports-in-azure-vms"></a>Öppna portar på virtuella Azure-datorer

Nya virtuella datorer är låsta som standard. 

Appar kan göra utgående begäranden men den enda inkommande trafik som tillåts är från det virtuella nätverket (t.ex. andra resurser på samma lokala nätverk) och från Azure Load Balancer (hälsoavsökningar).

Det finns två steg för att justera konfigurationen för att stödja olika protokoll i nätverket. När du skapar en ny virtuell dator har du möjlighet att öppna några vanliga portar (RDP, HTTP, HTTPS och SSH). Men om du behöver andra ändringar av brandväggen måste du justera dem manuellt.

Processen för det här omfattar två steg:

1. Skapa en nätverkssäkerhetsgrupp.
2. Skapa en inkommande regel som tillåter trafik på portarna som du behöver.

### <a name="what-is-a-network-security-group"></a>Vad är en nätverkssäkerhetsgrupp?

Virtuella nätverk är grunden för Azure-nätverksmodellen och ger isolering och skydd. Nätverkssäkerhetsgrupper (NSG) är det primära verktyget för att tillämpa och styra regler för nätverkstrafik på nätverksnivå. NSG:er är ett valfritt säkerhetslager som tillhandahåller en programvarubrandvägg genom att filtrera inkommande och utgående trafik på det virtuella nätverket. 

Säkerhetsgrupper kan kopplas till ett nätverksgränssnitt (för regler per värd), ett undernät i det virtuella nätverket (om du vill tillämpa på flera resurser) eller båda nivåerna. 

#### <a name="security-group-rules"></a>Regler för säkerhetsgrupper

Nätverkssäkerhetsgrupper använder _regler_ för att tillåta eller neka trafik i nätverket. Varje regeln identifierar käll- och måladress (eller intervall), protokoll, port (eller intervall), riktning (inkommande eller utgående), en numerisk prioritet och om trafiken som matchar regeln ska tillåtas eller nekas.

![En bild som visar nätverkssäkerhetsgrupper arkitektur i två olika undernät. Det finns två virtuella datorer i ett undernät, var och en med sina egna regler för gränssnittet.  Själva undernätet har en uppsättning regler som gäller för båda de virtuella datorerna. ](../media/7-nsg-rules.png)

Varje säkerhetsgrupp har en uppsättning standardsäkerhetsregler för att tillämpa standardnätverksregler som beskrivs ovan. Dessa standardregler kan inte ändras men de _kan_ åsidosättas.

#### <a name="how-azure-uses-network-rules"></a>Så använder Azure nätverksregler

För inkommande trafik bearbetar Azure först den säkerhetsgrupp som kopplas till undernätet och därefter den säkerhetsgrupp som tillämpas på nätverksgränssnittet. Utgående trafik hanteras i omvänd ordning (nätverksgränssnittet först, sedan undernätet).

> [!WARNING]  
> Tänk på att säkerhetsgrupper är valfria på båda nivåerna. Om ingen säkerhetsgrupp tillämpas, **tillåts all trafik** av Azure. Om den virtuella datorn har en offentlig IP-adress kan det vara en allvarlig risk, särskilt om operativsystemet inte har en inbyggd brandvägg.

Reglerna utvärderas i _prioritetsordning_, från regeln med **lägst prioritet**. Neka-regler **stoppar** alltid utvärderingen. Exempel: Om en regel för nätverksgränssnittet blockerar en utgående begäran kontrolleras inte några regler som tillämpas på undernätet. För att trafik ska tillåtas via säkerhetsgruppen måste den passera genom _alla_ tillämpade grupper.

Den sista regeln är alltid **Neka alla**. Det här är en standardregel som läggs till i varje säkerhetsgrupp för både inkommande och utgående trafik med prioriteten 65500. Det innebär att för att trafik ska passera genom säkerhetsgruppen _måste du ha en Tillåt-regel_. Annars blockeras den av den sista standardregeln.

> [!NOTE]  
> SMTP (port 25) är ett specialfall. Utgående SMTP-trafik kan komma att stoppas, beroende på din prenumerationsnivå och när ditt konto skapades. Du kan begära att ta bort denna begränsning med affärsrelaterad motivering.

Eftersom vi inte har skapat en säkerhetsgrupp för den här virtuella datorn ska vi göra det och tillämpa den.

## <a name="creating-network-security-groups"></a>Skapa nätverkssäkerhetsgrupper

Säkerhetsgrupper är hanterade resurser som det mesta i Azure. Du kan skapa dem i Azure Portal eller via kommandoradsverktyg för skript. Utmaningen är att definiera reglerna. Låt oss titta på att definierar en ny regel för att tillåta HTTP-åtkomst och blockera allt annat.
Vi har installerat vår anpassade programvara, konfigurerat en FTP-server och konfigurerat den virtuella datorn för att ta emot våra videofiler. Men om vi försöker ansluta till vår offentliga IP-adress med FTP kommer vi att märka att den är blockerad. 

Justeringar i serverkonfigurationen utförs ofta med utrustning i din lokala miljö. I det avseendet kan du se virtuella Azure-datorer som en förlängning av den här miljön. Du kan göra konfigurationsändringar, hantera nätverk, öppna eller blockera trafik och mycket mer via Azure Portal, Azure CLI eller Azure PowerShell.

Du har redan sett några av de grundläggande informations- och hanteringsalternativen på panelen **Översikt** för den virtuella datorn. Nu ska vi titta närmare på nätverkskonfigurationen.

## <a name="opening-ports-in-azure-vms"></a>Öppna portar i virtuella Azure-datorer

<!-- TODO: Azure portal is inconsistent here in applying the NSG.
By default, new VMs are locked down. 

Apps can make outgoing requests, but the only inbound traffic allowed is from the virtual network (e.g. other resources on the same local network), and from Azure's Load Balancer (probe checks). -->

Två steg krävs för att ändra konfigurationen och ge stöd för FTP. När du skapar en ny virtuell dator har du möjlighet att öppna några vanliga portar (RDP, HTTP, HTTPS och SSH). Om du behöver göra andra ändringar i brandväggen måste du dock göra dem själv.

Processen för det här omfattar två steg:

1. Skapa en nätverkssäkerhetsgrupp.
2. Skapa en inkommande regel som tillåter trafik på port 20 och 21 för aktivt FTP-stöd.

### <a name="what-is-a-network-security-group"></a>Vad är en nätverkssäkerhetsgrupp?

Virtuella nätverk är grunden för Azure-nätverksmodellen och ger isolering och skydd. Nätverkssäkerhetsgrupper (NSG) är det huvudsakliga verktyget för att tillämpa och styra regler för nätverkstrafik på nätverksnivå. NSG:er är ett valfritt säkerhetslager som tillhandahåller en programvarubrandvägg genom att filtrera inkommande och utgående trafik på det virtuella nätverket. 

Säkerhetsgrupper kan kopplas till ett nätverksgränssnitt (för regler per värd), ett undernät i det virtuella nätverket (om du vill tillämpa på flera resurser) eller båda nivåerna. 

#### <a name="security-group-rules"></a>Regler för säkerhetsgrupper

NGS:er använder _regler_ för att tillåta eller neka trafik genom nätverket. Varje regeln identifierar käll- och måladress (eller intervall), protokoll, port (eller intervall), riktning (inkommande eller utgående), en numerisk prioritet och om trafiken som matchar regeln ska tillåtas eller nekas. Följande bild visar NSG-regler som tillämpas på nivåerna för undernät och gränssnitt.

![En bild som visar nätverkssäkerhetsgrupper arkitektur i två olika undernät. Det finns två virtuella datorer i ett undernät, var och en med sina egna regler för gränssnittet.  Själva undernätet har en uppsättning regler som gäller för båda de virtuella datorerna.](../media/7-nsg-rules.png)

Varje säkerhetsgrupp har en uppsättning standardsäkerhetsregler för att tillämpa standardnätverksregler som beskrivs ovan. Dessa standardregler kan inte ändras men de _kan_ åsidosättas.

#### <a name="how-azure-uses-network-rules"></a>Så använder Azure nätverksregler

För inkommande trafik bearbetar Azure den säkerhetsgrupp som är associerad med undernätet först, och sedan den säkerhetsgrupp som är kopplad till nätverksgränssnittet. Utgående trafik bearbetas i omvänd ordning (nätverksgränssnittet först, sedan undernätet).

> [!WARNING]
> Tänk på att säkerhetsgrupper är valfria på båda nivåerna. Om ingen säkerhetsgrupp tillämpas **tillåts all trafik** av Azure. Om den virtuella datorn har en offentlig IP-adress kan detta utgöra en allvarlig risk, särskilt om operativsystemet inte har någon brandvägg.

Reglerna utvärderas i _prioritetsordning_, från regeln med **lägst prioritet**. Neka-regler **stoppar** alltid utvärderingen. Om en utgående begäran exempelvis blockeras av en regel för nätverksgränssnittet kontrolleras inte eventuella regler som tillämpas på undernätet. För att trafik ska släppas genom säkerhetsgruppen måste den passera _alla_ grupper som tillämpas.

Den sista regeln är alltid **Neka alla**. Det här är en standardregel som läggs till i varje säkerhetsgrupp för både inkommande och utgående trafik med prioriteten 65500. För att trafik ska släppas genom säkerhetsgruppen _måste du därför definiera en regel av typen ”Tillåt”_. Annars blockeras trafiken av den sista standardregeln.

> [!NOTE]
> SMTP (port 25) är ett specialfall – beroende på din prenumerationsnivå och när ditt konto har skapats kan utgående SMTP-trafik blockeras. Du kan begära att få begränsningen borttagen genom att skicka in en affärsrelaterad motivering.

Eftersom vi inte har skapat en säkerhetsgrupp för den här virtuella datorn ska vi göra det och tillämpa den.

## <a name="creating-network-security-groups"></a>Skapa nätverkssäkerhetsgrupper

Som det mesta i Azure är säkerhetsgrupper hanterade resurser. Du kan skapa dem på Azure Portal eller med det kommandoradsbaserade skriptverktyget. Utmaningen är att definiera reglerna. Nu ska vi se hur du skapar en ny regel för att tillåta FTP-åtkomst.
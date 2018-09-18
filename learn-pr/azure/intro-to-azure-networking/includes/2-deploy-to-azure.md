Första steget är sannolikt att återskapa din lokala konfiguration i molnet.

Den här grundläggande konfigurationen ger dig en uppfattning om hur nätverk konfigureras och hur nätverkstrafik färdas in i och ut från Azure.

## <a name="your-e-commerce-site-at-a-glance"></a>Kort om din e-handelswebbplats

En [N-nivåarkitektur](https://docs.microsoft.com/azure/architecture/guide/architecture-styles/n-tier) delar in ett program i två eller fler logiska nivåer. Arkitektoniskt sett kan en högre nivå komma åt tjänster från en lägre nivå, men en lägre nivå bör aldrig komma åt en högre nivå.

Nivåer hjälper till att avgränsa problemområden och ska helst utformas för att vara återanvändbara. En nivåindelad arkitektur förenklar även underhållet. Nivåer kan uppdateras eller ersättas oberoende av varandra och nya nivåer kan läggas till om det behövs.

_Med tre nivåer_ anger ett N-nivåprogram med tre nivåer. Ditt e-handelswebbprogram följer den här arkitekturen med tre nivåer:

* **Webbnivån** levererar webbgränssnittet till dina användare via en webbläsare.
* **Programnivån** kör affärslogik.
* **Datanivån** innehåller databaser och annan lagring som lagrar produktinformation och kundordrar.

Här är ett diagram. Spåra flödet från användaren till datanivån.

![Diagram över en enkel webbapp med tre nivåer](../media-draft/three-tier.png)

När användaren klickar på knappen för att lägga ordern skickas begäran till webbnivån tillsammans med användarens adress och betalningsinformation. Webbnivån skickar denna information till programnivån, som verifierar betalningsinformation och kontrollerar lagret. Programnivån kan sedan lagra ordern på datanivån för senare hämtning och expediering.

## <a name="your-e-commerce-site-running-on-azure"></a>Din e-handelswebbplats som körs på Azure

Azure tillhandahåller många olika sätt att hantera dina webbprogram, från fullständigt förkonfigurerade miljöer som lagrar din kod till virtuella datorer som du konfigurerar, anpassar och hanterar.

Vi antar att du väljer att köra din e-handelswebbplats på virtuella datorer. Så här kan det se ut i din testmiljö som körs på Azure.

![Diagram över en enkel webbapp med tre nivåer som körs i Azure](../media-draft/test-deployment.png)

Vi går igenom hur det här fungerar.

### <a name="what-is-an-azure-region"></a>Vad är en Azure-region?

En _region_ är ett Azure-datacenter inom en viss geografisk plats. USA, östra, USA, västra och Europa, norra är exempel på regioner. Som du ser körs programmet i regionen USA, östra.

### <a name="what-is-a-virtual-network"></a>Vad är ett virtuellt nätverk?

En _virtuellt nätverk_ är ett logiskt isolerat nätverk på Azure. Virtuella Azure-nätverk är något du känner till om du har konfigurerat nätverk på Hyper-V, VMware eller kanske på andra offentliga moln.

Webb-, program- och datanivåerna har en enskild virtuell dator. Varje virtuell dator tillhör ett virtuellt nätverk.

Användarna interagerar med webbnivån direkt, så den virtuella datorn har en offentlig IP-adress. Användarna interagerar inte med program- eller datanivåerna. Dessa datorer har därför en privat IP-adress.

Azure-datacenter hanterar den fysiska maskinvaran åt dig. Du konfigurerar virtuella nätverk via programvara, vilket gör att du kan hantera ett virtuellt nätverk precis som ditt eget nätverk. Du kan till exempel dela upp ett virtuellt nätverk i undernät för att få bättre kontroll över hur nätverket tilldelar IP-adresser. Du väljer även vilka andra nätverk det virtuella nätverket kan nå, oavsett om det är det offentliga Internet eller andra nätverk i det privata IP-adressutrymmet.

### <a name="whats-a-network-security-group"></a>Vad är en nätverkssäkerhetsgrupp?

En _nätverkssäkerhetsgrupp_, eller NSG, tillåter eller avvisar inkommande nätverkstrafik till dina Azure-resurser. Du kan betrakta en nätverkssäkerhetsgrupp som en brandvägg på molnnivå för nätverket.

Till exempel kan du se att den virtuella datorn på webbnivån tillåter inkommande trafik på portarna 22 (SSH) och 80 (HTTP). Varje nätverkssäkerhetsgrupp här tillåter trafik från alla källor. Du kan konfigurera en nätverkssäkerhetsgrupp för att endast acceptera trafik från kända källor, till exempel IP-adresser som du litar på.

> [!NOTE]
> Med port 22 kan du ansluta direkt till Linux-system via SSH. Här visar vi port 22 som öppen i utbildningssyfte. I praktiken kan du konfigurera VPN-åtkomst till ditt virtuella nätverk för att öka säkerheten.

## <a name="summary"></a>Sammanfattning

Ditt program med tre nivåer körs nu på Azure i regionen USA, östra. En _region_ är ett Azure-datacenter inom en viss geografisk plats.

Varje nivå kan bara komma åt tjänster från en lägre nivå. Den virtuella dator som kör på webbnivån har en offentlig IP-adress eftersom den tar emot trafik från Internet. De virtuella datorerna på de lägre nivåerna, program- och datanivåerna, har båda privata IP-adresser eftersom de inte kommunicerar direkt via Internet.

Med _virtuella nätverk_ kan du gruppera och isolera relaterade system. Du definierar _nätverkssäkerhetsgrupper_ för att styra vilken trafik som kan flöda via ett virtuellt nätverk.

Den konfiguration som du såg här är en bra början. Men när du distribuerar din e-handelswebbplats till produktion i molnet kommer du troligen stöta på samma problem som i den lokala distributionen.

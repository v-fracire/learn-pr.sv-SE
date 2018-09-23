Första steget är sannolikt att återskapa din lokala konfiguration i molnet.

Den här grundläggande konfigurationen ger dig en uppfattning om hur nätverk konfigureras och hur nätverkstrafik färdas in i och ut från Azure.

## <a name="your-e-commerce-site-at-a-glance"></a>Kort om din e-handelswebbplats

Större företagssystem består ofta av flera sammankopplade program och tjänster som fungerar tillsammans. Du kan ha ett klientdelswebbsystem som visar lager och låter kunder skapa en beställning. Den kanske pratar med olika webbtjänster för att tillhandahålla lagerdata, hantera användarprofiler, bearbeta kreditkort och fullfölja begäranden av bearbetade beställningar.

Det finns flera strategier och mönster som används av programvaruarkitekter och utvecklare för att göra dessa komplexa system enklare att utforma, skapa, hantera och underhålla. Låt oss titta på några av dem och starta med _löst sammansatta arkitekturer_.

#### <a name="benefits-of-loosely-coupled-architectures"></a>Fördelar med löst kopplade arkitekturer

> [!VIDEO https://www.microsoft.com/videoplayer/embed/RE2yHrc]

### <a name="using-an-n-tier-architecture"></a>Använd en arkitektur på N-nivå

Ett arkitekturmönster som kan användas för att skapa löst kopplade system är _N-nivå_.

En [N-nivåarkitektur](https://docs.microsoft.com/azure/architecture/guide/architecture-styles/n-tier) delar in ett program i två eller fler logiska nivåer. Arkitektoniskt sett kan en högre nivå komma åt tjänster från en lägre nivå, men en lägre nivå bör aldrig komma åt en högre nivå.

Nivåer hjälper till att avgränsa problemområden och ska helst utformas för att vara återanvändbara. En nivåindelad arkitektur förenklar även underhållet. Nivåer kan uppdateras eller ersättas oberoende av varandra och nya nivåer kan läggas till om det behövs.

_Med tre nivåer_ anger ett N-nivåprogram med tre nivåer. Ditt e-handelswebbprogram följer den här arkitekturen med tre nivåer:

* **Webbnivån** levererar webbgränssnittet till dina användare via en webbläsare.
* **Programnivån** kör affärslogik.
* **Datanivån** inkluderar databaser och annan lagring som lagrar produktinformation och kundbeställningar.

Följande bild visar flödet av begäran från användaren till datanivån.

![En bild som visar en arkitektur med tre nivåer där varje nivå finns på en dedikerad virtuell dator.](../media/2-three-tier.png)

När användaren klickar på knappen för att lägga beställningen skickas begäran till webbnivån tillsammans med användarens adress och betalningsinformation. Webbnivån skickar denna information till programnivån, som verifierar betalningsinformation och kontrollerar lagret. Programnivån kan sedan lagra ordern på datanivån för senare hämtning och expediering.

## <a name="your-e-commerce-site-running-on-azure"></a>Din e-handelswebbplats som körs på Azure

Azure tillhandahåller många olika sätt att hantera dina webbprogram, från fullständigt förkonfigurerade miljöer som lagrar din kod till virtuella datorer som du konfigurerar, anpassar och hanterar.

Vi antar att du väljer att köra din e-handelswebbplats på virtuella datorer. Så här kan det se ut i din testmiljö som körs på Azure. Följande bild visar en arkitektur med tre nivåer som körs på virtuella datorer med säkerhetsfunktionerna aktiverade för att begränsa inkommande begäranden. 

![En bild som visar en arkitektur med tre nivåer där varje nivå körs på en separat virtuell dator. Varje virtuell dator är märkt med dess IP-adress och är inuti sitt egna virtuella nätverk. Varje virtuellt nätverk har en nätverkssäkerhetsgrupp som listar de öppna portarna.](../media/2-test-deployment.png)

Vi går igenom hur det här fungerar.

:::row:::
  :::column:::
    ![En fäst plats på jorden som representerar en Azure-region](../media/2-azure-region.png)
  :::column-end:::
    :::column span="3"::: **Vad är en Azure-region?**

En _region_ är ett Azure-datacenter inom en viss geografisk plats. USA, östra, USA, västra och Europa, norra är exempel på regioner. I det här fallet ser du att programmet körs i regionen USA, östra.

  :::column-end:::
:::row-end:::
:::row:::
  :::column:::
    ![Två virtuella datorer som körs i ett virtuellt nätverk](../media/2-azure-vnet.png)
  :::column-end:::
    :::column span="3"::: **Vad är ett virtuellt nätverk?**

Ett _virtuellt nätverk_ är ett logiskt isolerat nätverk på Azure. Virtuella Azure-nätverk är något du känner till om du har konfigurerat nätverk på Hyper-V, VMware eller till och med andra offentliga moln.

Webb-, program- och datanivåerna har en virtuell dator var. Varje virtuell dator tillhör ett virtuellt nätverk.

Användarna interagerar med webbnivån direkt, så den virtuella datorn har en offentlig IP-adress. Användarna interagerar inte med program- eller datanivåerna. Dessa datorer har därför en privat IP-adress.

Azure-datacenter hanterar den fysiska maskinvaran åt dig. Du konfigurerar virtuella nätverk via programvara, vilket gör att du kan hantera ett virtuellt nätverk precis som ditt eget nätverk. Du kan till exempel dela upp ett virtuellt nätverk i undernät för att få bättre kontroll över hur nätverket tilldelar IP-adresser. Du väljer även vilka andra nätverk det virtuella nätverket kan nå, oavsett om det är det offentliga Internet eller andra nätverk i det privata IP-adressutrymmet.

  :::column-end:::
:::row-end:::
:::row:::
  :::column:::
    ![Två virtuella datorer med en delad nätverkssäkerhetsgrupp](../media/2-azure-nsg.png)
  :::column-end:::
    :::column span="3"::: **Vad är en nätverkssäkerhetsgrupp?**

En _nätverkssäkerhetsgrupp_ eller NSG tillåter eller avvisar inkommande nätverkstrafik till dina Azure-resurser. Du kan betrakta en nätverkssäkerhetsgrupp som en brandvägg på molnnivå för nätverket.

Till exempel kan du se att den virtuella datorn på webbnivån tillåter inkommande trafik på portarna 22 (SSH) och 80 (HTTP). Den här virtuella datorns nätverkssäkerhetsgrupp tillåter inkommande trafik över de här portarna från alla källor. Du kan konfigurera en nätverkssäkerhetsgrupp att endast acceptera trafik från kända källor, till exempel IP-adresser som du litar på.

> [!NOTE]
> Med port 22 kan du ansluta direkt till Linux-system via SSH. Här visar vi port 22 som öppen i utbildningssyfte. I praktiken kan du konfigurera VPN-åtkomst till ditt virtuella nätverk för att öka säkerheten.

  :::column-end:::
:::row-end:::

## <a name="summary"></a>Sammanfattning

Ditt program med tre nivåer körs nu på Azure i regionen USA, östra. En _region_ är ett Azure-datacenter inom en viss geografisk plats.

Varje nivå kan bara komma åt tjänster från en lägre nivå. Den virtuella dator som kör på webbnivån har en offentlig IP-adress eftersom den tar emot trafik från Internet. De virtuella datorerna på de lägre nivåerna, program- och datanivåerna, har båda privata IP-adresser eftersom de inte kommunicerar direkt via Internet.

Med _virtuella nätverk_ kan du gruppera och isolera relaterade system. Du definierar _nätverkssäkerhetsgrupper_ för att styra vilken trafik som kan flöda via ett virtuellt nätverk.

Den konfiguration som du såg här är en bra början. Men när du distribuerar din e-handelswebbplats till produktion i molnet kommer du troligen stöta på samma problem som i den lokala distributionen.

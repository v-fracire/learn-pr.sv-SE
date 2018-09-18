Organisationer har ofta flera lagringskonton så att de kan implementera olika kravuppsättningar. I exemplet med chokladtillverkaren skulle det finnas ett lagringskonto för privata affärsdata och ett för konsumentinriktade filer. Här lär du dig om de principfaktorer som styrs av ett lagringskonto och som hjälper dig att avgöra hur många konton du behöver.

## <a name="what-is-azure-storage"></a>Vad är Azure Storage?

Azure erbjuder många sätt att lagra data. Det finns flera alternativ för databaser som Azure SQL, Cosmos DB, Azure Tables osv. I Azure kan du lagra meddelanden på flera sätt, t.ex. med Azure Queues och Event Hubs. Du kan även lagra lösa filer med hjälp av alternativ som Azure Files och Azure Blobs.

Azure har valt fyra av dessa datatjänster och placerat dem tillsammans med namnet _Azure Storage_. De fyra tjänsterna är: Azure Blobs, Azure Files, Azure Queues och Azure Tables. Följande bild visar elementen i Azure Storage.

![Bild som visar de Azure-datatjänster som ingår i Azure Storage.](../media-drafts/2-azure-storage.png)

Dessa fyra gavs särskild behandling eftersom de alla är primitiva, molnbaserade lagringstjänster och ofta används tillsammans i samma program.

## <a name="what-is-a-storage-account"></a>Vad är ett lagringskonto?

En _lagringskonto_ är en container som grupperar en uppsättning Azure Storage-tjänster tillsammans. Endast datatjänster från Azure Storage kan ingå i ett lagringskonto (Azure Blobs, Azure Files, Azure Queues och Azure Tables). Följande bild visar ett lagringskonto som innehåller flera datatjänster.

![Bild av ett Azure-lagringskonto som innehåller en blandad uppsättning datatjänster.](../media-drafts/2-what-is-a-storage-account.png)

Genom att kombinera datatjänster i ett lagringskonto kan du hantera dem som en grupp. De inställningar du anger när du skapar kontot, eller dem som du ändrar efter skapandet, tillämpas på allt i kontot. Om du tar bort lagringskontot tas alla data som lagras i det bort.

Ett lagringskonto är en Azure-resurs och ingår i en resursgrupp. Följande bild visar en Azure-prenumeration som innehåller flera resursgrupper där varje grupp innehåller ett eller flera lagringskonton.

![Bild av en Azure-prenumeration som innehåller flera resursgrupper och lagringskonton.](../media-drafts/2-resource-groups-and-storage-accounts.png)

Andra Azure-datatjänster som Azure SQL och Cosmos DB hanteras som oberoende Azure-resurser och kan inte ingå i ett lagringskonto. Följande bild visar ett typiskt arrangemang: blobar, filer, köer och tabeller finns i lagringskonton medan andra tjänster inte gör det.

![Bild av en Azure-prenumeration som visar vissa datatjänster som inte kan läggas till i ett lagringskonto.](../media-drafts/2-typical-subscription-organization.png)

## <a name="storage-account-settings"></a>Inställningar för lagringskonto

Ett lagringskonto definierar en princip som gäller för alla lagringstjänster i kontot. Du kan till exempel ange att alla inneslutna tjänster ska lagras i datacentret i USA, västra, endast kan nås över https och faktureras till försäljningsavdelningens prenumeration.

De inställningar som styrs av ett lagringskonto är:

- **Prenumeration**: Den Azure-prenumeration som debiteras för tjänster i kontot.

- **Plats**: Det datacenter som lagrar tjänsterna i kontot.

- **Prestanda**: Avgör vilka datatjänster du kan ha på ditt lagringskonto och typen av den underliggande maskinvarudisken. **Standard** tillåter alla datatjänster (blob, fil, kö, tabell) och använder magnetiska hårddiskar. **Premium** begränsar dig till en viss typ av blob som kallas en _sidblob_ och använder SSD-diskar för lagring.

- **Replikering**: Avgör vilken strategi som används för att göra kopior av dina data för att skydda mot maskinvarufel och naturkatastrofer. Azure upprätthåller automatiskt minst en kopia av dina data i det datacenter som är associerat med lagringskontot. Detta kallas lokalt redundant lagring (LRS) och skyddar mot maskinvarufel men skyddar inte mot händelser som slår ut hela datacentret. Du kan uppgradera till ett av de andra alternativen, till exempel geo-redundant lagring (GRS), för att få replikering på andra datacenter över hela världen.

- **Åtkomstnivå**: Styr hur snabbt du kan komma åt blobar i det här lagringskontot. Frekvent ger snabbare åtkomst än Lågfrekvent men till ett högre pris. Detta gäller endast för blobar och fungerar som standardvärde för nya blobar.

- **Säker överföring krävs**: En säkerhetsfunktion som bestämmer vilket protokoll som stöds för åtkomst: aktiverat kräver https medan inaktiverat tillåter http.

- **Virtuella nätverk**: En säkerhetsfunktion som endast tillåter inkommande begäranden från de virtuella nätverk som du anger.

## <a name="how-many-storage-accounts-do-you-need"></a>Hur många lagringskonton behöver du?

Ett lagringskonto representerar en samling inställningar såsom plats, replikeringsstrategi, prenumeration osv. Du behöver ett lagringskonto för varje grupp med inställningar som du vill tillämpa på dina data. Följande bild visar två lagringskonton som skiljer sig i en inställning; den enstaka skillnaden är tillräcklig för att kräva separata lagringskonton.

![Bild som visar två lagringskonton med olika inställningar.](../media-drafts/2-multiple-storage-accounts.png)

Det antal lagringskonton som du behöver avgörs vanligtvis av din datamångfald, kostnadskänslighet och tolerans för hanteringsarbete.

### <a name="data-diversity"></a>Datamångfald

Organisationer genererar ofta data som skiljer sig i hur de används, hur känsliga de är, vilken grupp som betalar fakturorna osv. Mångfald längsmed dessa vektorer kan leda till flera lagringskonton. Vi tittar på två exempel:

1. Har du data som är specifika för ett land eller en region? I så fall bör du kanske lagra dem i ett datacenter i det landet av prestanda- eller efterlevnadsskäl. Du behöver ett lagringskonto för varje plats.

1. Har du vissa data som är privata och vissa som är till för offentlig användning? I så fall kan du aktivera virtuella nätverk för privata data och inte för offentliga data. Detta kräver också separata lagringskonton.

I allmänhet innebär ökad mångfald ett större antal lagringskonton.

### <a name="cost-sensitivity"></a>Kostnadskänslighet

Ett lagringskonto i sig medför ingen ekonomisk kostnad. Däremot påverkar de inställningar du väljer för kontot kostnaden för tjänsterna i kontot. Geo-redundant lagring kostar mer än lokalt redundant lagring. Premium-prestanda och frekvent lagringsnivå ökar kostnaden för blobar.

Du kan använda flera lagringskonton för att minska kostnaderna. Du kan till exempel partitionera dina data till kritiska och icke-kritiska kategorier. Du kan placera dina viktiga data i ett lagringskonto med geo-redundant lagring och placera dina icke-kritiska data i ett annat lagringskonto med lokalt redundant lagring.

### <a name="tolerance-for-management-overhead"></a>Tolerans för hanteringsarbete

Varje lagringskonto kräver tid och uppmärksamhet från en administratör i form av skapande och underhåll. Det ökar även komplexiteten för alla som lägger till data till molnlagringen. Alla i den här rollen måste förstå syftet med varje lagringskonto så att de lägger till nya data till rätt konto.

## <a name="summary"></a>Sammanfattning

Lagringskonton är ett kraftfullt verktyg som hjälper dig att få de prestanda och den säkerhet du behöver samtidigt som kostnaderna minimeras. En vanligt strategi är att börja med en analys av dina data och skapa partitioner som delar egenskaper som plats, fakturering, replikeringsstrategi osv. och sedan skapa ett lagringskonto för varje partition.

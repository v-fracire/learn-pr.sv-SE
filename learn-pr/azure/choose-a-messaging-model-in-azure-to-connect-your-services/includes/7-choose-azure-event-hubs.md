Det finns vissa program som producerar ett enormt antal händelser från nästan lika många källor. Termen ”Stordata” tillämpas ofta på sådana situationer, och det krävs unik infrastruktur för att hantera dem.

Anta att du arbetar på Contoso Aircraft Engines. Motorerna som din arbetsgivare tillverkar har hundratals sensorer. Innan ett flygplan kan tas upp varje morgon, kopplas motorerna till en testmiljö och gås igenom. Dessutom strömmas cachelagrade flygdata när flygplanet är anslutet till markutrustning.

Nu vill du använda historiska sensordata för att hitta mönster i sensoravläsningarna som indikerar att motorfel troligen kommer att inträffa snart. Du vill också att sensoravläsningarna i realtid ska jämföras mot de här felmönstren. Sedan kan du varna användarna nära nog i realtid om en motor visar oroväckande värden.

## <a name="what-is-azure-event-hubs"></a>Vad är Azure Event Hubs?

Azure Event Hub är en mellanhand för kommunikationsmönstret publicera-prenumerera, men till skillnad från Event Grid är det optimerat för mycket stora dataflöden, ett stort antal utgivare, säkerhet och återhämtning.

Medan Event Grid passar in mer eller mindre perfekt i mönstret publicera-prenumerera eftersom det bara hanterar prenumerationer och dirigerar kommunikation till de prenumeranterna, utför Händelsehubben ett stort antal ytterligare tjänster så att det mer liknar en Service Bus eller meddelandekö mer än en enkel händelseleverantör.

#### <a name="partitions"></a>Partitioner ####
När Händelsehubben tar emot meddelanden delar den upp dem i partitioner. Partitioner är buffertar där meddelandena sparas. Händelsebuffertarna gör att händelserna inte är flyktiga, och en händelse missas inte om en prenumerant är upptagen eller offline. Prenumeranten kan alltid använda bufferten för att ”komma ikapp”. Som standard lagras händelserna i bufferten i 24 timmar. Sedan tas de bort automatiskt.

Buffertarna kallas partitioner eftersom data delas upp mellan dem. Varje Händelsehubb har minst två partitioner, och varje partition har en separat uppsättning prenumeranter.

#### <a name="capture"></a>Capture ####
Händelsehubben kan skicka alla händelser direkt till Azure Data Lake eller Blob Storage för prisvärd permanent lagring.

#### <a name="authentication"></a>Autentisering ####
Alla utgivare autentiseras och får ett token utfärdat. Det innebär att Händelsehubben kan ta emot händelser från externa enheter och mobilappar utan att falska data kan förstöra analysen. 

## <a name="using-azure-event-hub"></a>Använda en Azure-händelsehubb

Händelsehubbar har stöd för pipelining av händelseströmmar till andra Azure-tjänster. Om det används med Stream Analytics möjliggörs till exempel komplexa dataanalyser nära nog i realtid, eftersom flera händelser kan korreleras och mönster upptäckas. I det här fallet anses Stream Analytics som en prenumerant.

När det gäller våra flygplansmotorer konfigurerar vi arkitekturen så att Händelsehubbar autentiserar informationen från våra motorer. Vi låter då Händelsehubbar spara alla data i Azure Data Lake, och vi kan senare använda dessa data för att träna upp och förbättra våra maskininlärningsmodeller. Slutligen hämtas våra händelseströmmar av Azure Stream Analytics-prenumeranter. Stream Analytics använder vår maskininlärningsmodell för att leta efter mönster i sensorinformationen som kan indikera problem.

Eftersom vi har flera partitioner och varje motor skickar sina data endast till en partition behöver varje instans av Stream Analytics-prenumeranten endast hantera en delmängd av den fullständiga datamängden i stället för att filtrera och korrelera samtliga data.

## <a name="choose-event-grid-or-event-hub"></a>Välj Event Grid eller Händelsehubb

Båda har stöd för *At Least Once*-semantik.

Om du behöver en enkel händelseinfrastruktur med publicera-prenumerera och betrodda utgivare (till exempel din egen webbserver), bör du välja Azure Event Grid.

### <a name="consider-event-hub-if"></a>Överväg att använda Händelsehubb om
* du behöver kunna autentisera ett stort antal utgivare
* du måste spara en händelseström i Azure Data Lake eller Blob Storage
* du behöver aggregering eller analys av din händelseström
* du behöver tillförlitlig meddelandehantering eller återhämtning 
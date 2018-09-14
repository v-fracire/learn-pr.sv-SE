Det finns vissa program som producerar ett enormt antal händelser från nästan lika många källor. Termen ”Stordata” tillämpas ofta på sådana situationer, och det krävs unik infrastruktur för att hantera dem.

Anta att du arbetar på Contoso Aircraft Engines. Motorerna som din arbetsgivare tillverkar har hundratals sensorer. Innan ett flygplan kan tas upp varje morgon, kopplas motorerna till en testmiljö och gås igenom. Dessutom strömmas cachelagrade flygdata när flygplanet är anslutet till markutrustning.

Nu vill du använda historiska sensordata för att hitta mönster i sensoravläsningarna som indikerar att motorfel troligen kommer att inträffa snart. Du vill också att sensoravläsningarna i realtid ska jämföras mot de här felmönstren. Sedan kan du varna användarna nära nog i realtid om en motor visar oroväckande värden.

## <a name="what-is-azure-event-hubs"></a>Vad är Azure Event Hubs?

Event Hubs är en mellanhand för kommunikationsmönstret publicera–prenumerera. Men till skillnad från Event Grid är det optimerat för mycket stora dataflöden, ett stort antal utgivare, säkerhet och återhämtning.

Medan Event Grid passar in perfekt i mönstret publicera-prenumerera eftersom det bara hanterar prenumerationer och dirigerar kommunikation till de prenumeranterna, utför Event Hubs ett stort antal ytterligare tjänster. Dessa ytterligare tjänster gör att det snarare liknar en tjänstebuss eller meddelandekö, mer än en enkel händelseavsändare.

#### <a name="partitions"></a>Partitioner ####
När Event Hubs tar emot meddelanden delar den upp dem i partitioner. Partitioner är buffertar där meddelandena sparas. Händelsebuffertarna gör att händelserna inte är flyktiga, och en händelse missas inte om en prenumerant är upptagen eller offline. Prenumeranten kan alltid använda bufferten för att ”komma ikapp”. Som standard lagras händelserna i bufferten i 24 timmar. Sedan tas de bort automatiskt.

Buffertarna kallas partitioner eftersom data delas upp mellan dem. Varje händelsehubb har minst två partitioner, och varje partition har en separat uppsättning prenumeranter.

#### <a name="capture"></a>Capture ####
Event Hubs kan skicka alla händelser direkt till Azure Data Lake eller Azure Blob Storage för prisvärd, permanent lagring.

#### <a name="authentication"></a>Autentisering ####
Alla utgivare autentiseras och får ett token utfärdat. Det innebär att Event Hubs kan ta emot händelser från externa enheter och mobilappar utan att falska data kan förstöra analysen. 

## <a name="using-event-hubs"></a>Använda Event Hubs

Event Hubs har stöd för pipelining av händelseströmmar till andra Azure-tjänster. Om det används med Azure Stream Analytics möjliggörs till exempel komplexa dataanalyser nära nog i realtid, eftersom flera händelser kan korreleras och mönster upptäckas. I det här fallet anses Stream Analytics som en prenumerant.

När det gäller våra flygplansmotorer konfigurerar vi arkitekturen så att Event Hubs autentiserar informationen från våra motorer. Sedan låter vi den använda capture för att spara alla data i Data Lake. Senare kan vi använda alla data för att träna och förbättra våra maskininlärningsmodeller. Slutligen hämtas våra händelseströmmar av Stream Analytics-prenumeranter. Stream Analytics använder vår maskininlärningsmodell för att leta efter mönster i sensorinformationen som kan indikera problem.

Eftersom vi har flera partitioner och varje motor skickar sina data endast till en partition behöver varje instans av Stream Analytics-prenumeranten endast hantera en delmängd av den fullständiga datamängden. Den behöver inte filtrera och korrelera dem alla.

## <a name="choose-event-grid-or-event-hubs"></a>Välja Event Grid eller Event Hubs

Båda har stöd för *minst en gång*-semantik.

Om du behöver en enkel händelseinfrastruktur med publicera-prenumerera och betrodda utgivare (till exempel din egen webbserver), bör du välja Event Grid.

Överväg att använda Event Hubs om
* du behöver kunna autentisera ett stort antal utgivare
* du måste spara en händelseström i Data Lake eller Blob Storage
* du behöver aggregering eller analys av din händelseström
* du behöver tillförlitlig meddelandehantering eller återhämtning. 
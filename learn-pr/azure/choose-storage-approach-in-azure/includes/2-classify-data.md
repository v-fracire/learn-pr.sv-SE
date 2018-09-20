Det kan finnas olika typer av data i en onlinebutik. Olika lagringslösningar kan vara lämpliga för olika datatyper. 

Programdata kan klassificeras på ett av följande tre sätt: strukturerade, halvstrukturerade och ostrukturerade. Här får du lära dig att klassificera dina data så att du kan välja rätt lagringslösning.

#### <a name="approaches-to-storing-data-in-the-cloud"></a>Metoder för att lagra data i molnet

> [!VIDEO https://www.microsoft.com/videoplayer/embed/RE2yEuY]

## <a name="structured-data"></a>Strukturerade data

Strukturerade data följer ett schema, så att alla dataelement har samma fält eller egenskaper. Strukturerade data kan lagras i en databastabell med rader och kolumner. För strukturerade data används nycklar till att visa hur en rad i en tabell förhåller sig till relaterade data i en annan rad i en annan tabell. Strukturerade data kallas också för relationsbaserade data eftersom schemat definierar datatabellerna och fälten i tabellerna, samt relationen mellan dem.

Det är enkelt att använda strukturerade data eftersom de är enkla att ange, köra frågor mot och analysera. Alla data följer samma format.

Här är några exempel på strukturerade data:

- Sensordata
- Finansiella data

## <a name="semi-structured-data"></a>Halvstrukturerade data

Halvstrukturerade data är mindre ordnade än strukturerade data och lagras inte i ett relationsformat, eftersom fälten inte enkelt kan passas in i tabeller, rader och kolumner. Halvstrukturerade data innehåller taggar som gör att du kan organisera och ordna data hierarkiskt. Halvstrukturerade data kallas även för icke-relationella data eller NoSQL-data.

#### <a name="what-is-nosql"></a>Vad är NoSQL?

> [!VIDEO https://www.microsoft.com/videoplayer/embed/RE2yEvd]

Här är några exempel på halvstrukturerade data:

- Nyckel-/värde-par
- Diagramdata
- JSON-filer
- XML-filer

## <a name="unstructured-data"></a>Ostrukturerade data

Det är ofta svårt att avgöra hur du ska organisera ostrukturerade data. Ostrukturerade data levereras ofta i filer, till exempel foton och videor. Själva videofilen kan ha en övergripande struktur och levereras med halvstrukturerade metadata, men de data som utgör själva videon är ostrukturerade. Därför klassificeras foton, videor och andra liknande filer som ostrukturerade data.

Här är några exempel på ostrukturerade data:

- Mediafiler som foton, videor och ljudfiler
- Office-filer som Word-dokument
- Textfiler
- Loggfiler

Nu när du vet skillnaderna mellan de olika datatyperna ska vi titta på de datamängder som används i en onlinebutiksapp och klassificera dem.

## <a name="product-catalog-data"></a>Data i produktkatalogen

Data i produktkatalogen för en onlinebutik är ganska strukturerade till sin natur, eftersom varje produkt har en produkt-SKU, en beskrivning, en kvantitet, ett pris, olika storleksalternativ, färgalternativ, ett foto och eventuellt en video. Den här typen av data kan därför verka relationsbaserad eftersom alla dataelement har samma struktur. När du lägger till nya produkter eller andra produkttyper kan du däremot över tid vilja lägga till nya fält till dina produkter. Om du köper nya gympaskor kan de till exempel ha en Bluetooth-funktion som skickar sensordata från skon till en fitnessapp i telefonen. Det här verkar bli allt vanligare, och du vill att dina kunder framöver ska kunna filtrera fram skor som är ”Bluetooth-aktiverad”. Du vill inte behöva gå tillbaka och uppdatera alla befintliga skodata med egenskapen Bluetooth-aktiverad utan endast lägga till det för nya skor.

När du lägger till egenskapen Bluetooth-aktiverad är dina skodata inte längre likadana eftersom du har infört skillnader i schemat. Om det här var det enda exemplet du räknar med att stöta på så skulle du kunna gå tillbaka och normalisera befintliga data så att alla produkter har fältet Bluetooth-aktiverad. Det här är dock bara ett av många specialfält du kan tänka dig behöva i framtiden och därför klassificeras dessa data som halvstrukturerade. Data organiseras med taggar, men varje produkt i katalogen kan innehålla unika fält.

Dataklassificering: **halvstrukturerade**

## <a name="photos-and-videos"></a>Foton och videor

Foton och videor som visas på produktsidor är ostrukturerade data. Även om mediefilen kan innehålla metadata är mediefilens innehåll ostrukturerat.

Dataklassificering: **ostrukturerade**

## <a name="business-data"></a>Affärsdata

Affärsanalytiker vill använda business intelligence till att utvärdera lagerflöden och granska säljdata. För att kunna utföra de här åtgärderna så måste data från flera olika månader aggregeras och därefter frågas. På grund av behovet av aggregerade data, måste dessa data struktureras så att en månad kan jämföras med en annan.

Dataklassificering: **strukturerade**

## <a name="summary"></a>Sammanfattning

Data kan klassificeras på ett av följande tre sätt: strukturerade, halvstrukturerade och ostrukturerade. Om du förstår skillnaderna mellan dem så att du kan klassificera dina egna data kan du välja rätt lagringslösning. 

Som sagt, strukturerade data är organiserade data som enkelt kan ordnas i rader och kolumner i tabeller. Halvstrukturerade data visserligen organiserade och har tydliga egenskaper och värden, men det kan finnas skillnader mellan datapunkterna. Ostrukturerade data passar inte in i tabeller, och det finns inte heller något schema.
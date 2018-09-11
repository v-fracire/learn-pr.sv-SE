Det kan finnas olika typer av data i en onlinebutik. Vissa data kan vara strukturerade, som kundernas kontaktuppgifter, och andra data som produktbilder kan vara ostrukturerade. Det finns tre kategorier av data: strukturerade, halvstrukturerade och ostrukturerade. Olika lagringslösningar kan vara lämpliga för olika kategorier.

Här får du lära dig att klassificera dina data i lämpliga kategorier så att du kan välja rätt lagringslösning.

## <a name="structured-data"></a>Strukturerade data

Strukturerade data följer ett schema, så att alla dataelement har samma fält eller egenskaper. Strukturerade data kan lagras i en databastabell med rader och kolumner. För strukturerade data används nycklar till att koppla en rad i en tabell till relaterade data i en annan rad i en annan tabell. Strukturerade data kallas också för relationsbaserade data eftersom schemat definierar datatabellerna och fälten i tabellerna, samt relationen mellan dem.

Det är enkelt att använda strukturerade data eftersom det gemensamma formatet gör det enkelt att ange, köra frågor mot och analysera dem.

Här är några exempel på strukturerade data:
* Sensordata
* Finansiella data

## <a name="semi-structured-data"></a>Halvstrukturerade data

Halvstrukturerade data är mindre ordnade än strukturerade data och lagras inte i relationsdatabaser, eftersom fälten inte enkelt kan passas in i tabeller, rader och kolumner. Halvstrukturerade data innehåller taggar som gör att du kan organisera och ordna data hierarkiskt.  

Halvstrukturerade data kallas även för icke-relationella data eller NoSQL-data.

Här är några exempel på halvstrukturerade data:
* JSON-filer
* XML-filer

## <a name="unstructured-data"></a>Ostrukturerade data

Det är ofta svårt att avgöra hur du ska organisera ostrukturerade data bara genom att titta på dem. Ostrukturerade data levereras ofta i filer som foton och videor, där själva videofilen kan ha en övergripande struktur och innehålla halvstrukturerade metadata. De data som utgör själva videon är dock ostrukturerade, och därför klassificeras foton, videor och andra liknande filer som ostrukturerade data.

Här är några exempel på ostrukturerade data:
* Mediefiler som foton, videor och ljudfiler
* Office-filer som Word-dokument
* Textfiler
* Loggfiler

Nu när du vet skillnaderna mellan de olika datatyperna ska vi titta på de datamängder som används i en onlinebutiksapp och klassificera dem.

### <a name="product-catalog-data"></a>Data i produktkatalogen

Data i produktkatalogen för en onlinebutik är ganska strukturerade till sin natur, eftersom varje produkt har en produkt-SKU, en beskrivning, en kvantitet, ett pris, olika storleksalternativ, färgalternativ, ett foto och eventuellt en video. Den här typen av data kan därför verka relationsbaserad eftersom alla dataelement har samma struktur. När du lägger till nya produkter kanske du däremot vill lägga till nya fält med tiden. Om du köper nya gympaskor kan de till exempel ha en Bluetooth-funktion som skickar sensordata från skon till en fitnessapp i telefonen för att hålla reda på farten. Det här verkar bli allt vanligare, och du vill att dina kunder framöver ska kunna filtrera fram skor som är ”Bluetooth Enabled”. Du vill inte gå tillbaka och uppdatera alla befintliga sko-data med egenskapen Bluetooth Enabled utan endast lägga till den för nya skor.

När du lägger till fältet Bluetooth Enabled är dina skodata inte längre likadana eftersom du har infört skillnader i schemat. Om det här var en engångshändelse och det enda exemplet du räknar med att stöta på så skulle du kunna gå tillbaka och normalisera befintliga data så att alla produkter har fältet Bluetooth Enabled. Det här är dock bara ett av många specialfält du kan tänka dig kommer behövas i framtiden, och därför klassificeras dessa data som halvstrukturerade. Data organiseras med taggar, men varje produkt i katalogen kan innehålla unika fält.

Dataklassificering: **halvstrukturerade**

### <a name="photos-and-videos"></a>Foton och videor

Foton och videor som visas på produktsidor är ostrukturerade data. Även om mediefilen kan innehålla metadata är mediefilens innehåll ostrukturerat.

Dataklassificering: **ostrukturerade**

### <a name="business-data"></a>Affärsdata

Affärsanalytiker vill använda business intelligence till att utvärdera lagerflöden och granska säljdata. Om det ska kunna göra det måste data från flera olika månader aggregeras så att analytikerna kan köra frågor mot dem. Eftersom data måste aggregeras måste de vara strukturerade så att en månad kan jämföras med en annan.

Dataklassificering: **strukturerade**

## <a name="summary"></a>Sammanfattning

Det finns tre kategorier av data: strukturerade, halvstrukturerade och ostrukturerade. Om du förstår skillnaderna mellan dem och kan avgöra vilken kategori dina data tillhör kan du välja rätt lagringslösning. Strukturerade data är organiserade data som enkelt kan ordnas i rader och kolumner i tabeller. Halvstrukturerade data visserligen organiserade och har tydliga egenskaper och värden, men det kan finnas skillnader mellan datapunkterna. Ostrukturerade data passar inte in i tabeller, och det finns inte heller något schema.

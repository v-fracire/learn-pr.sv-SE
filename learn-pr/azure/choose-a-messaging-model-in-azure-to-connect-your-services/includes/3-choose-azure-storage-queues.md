Anta att du planerar arkitekturen för ditt musikdelningsprogram. Du vill säkerställa att musikfilerna överförs till webb-API:et på ett tillförlitligt sätt från den mobila appen – sedan ska informationen om de nya låtarna levereras direkt till appen när en artist lägger till ny musik i sin samling. Det här är ett perfekt användningsområde för ett meddelandebaserat system. Azure erbjuder två lösningar på det här problemet:

- Azure Queue Storage
- Azure Service Bus

## <a name="what-is-azure-queue-storage"></a>Vad är Azure Queue Storage?
Queue Storage är en tjänst som använder Azure Storage för att lagra stora mängder meddelanden som på ett säkert sätt kan nås från var som helst i världen med ett enkelt REST-baserat gränssnitt. Köer kan innehålla miljontals meddelanden; det begränsas bara av kapaciteten för det lagringskonto som äger det.

## <a name="what-is-azure-service-bus"></a>Vad är Azure Service Bus?
Service Bus är ett system för asynkron meddelandekö som är avsett för företagsprogram. De här apparna använder ofta flera kommunikationsprotokoll, har olika datakontrakt, högre säkerhetskrav och kan innehålla både molnbaserade och lokala tjänster. Service Bus är byggt ovanpå en dedikerad meddelandeinfrastruktur som har utformats för just dessa scenarier.

Båda tjänsterna baseras på konceptet med en ”kö” som kvarhåller skickade meddelanden tills målet är redo att ta emot dem. Om du aldrig har arbetat med ett meddelandekösystem förut finns det flera praktiska fördelar att upptäcka.

## <a name="increased-reliability"></a>Ökad tillförlitlighet
Köer används av distribuerade program som en tillfällig lagringsplats för meddelanden som väntar på leverans till en målkomponent. Källkomponenten kan lägga till ett meddelande till kön, och målkomponenter kan hämta meddelandet längst fram i kön för bearbetning. Köer ökar tillförlitligheten för meddelanden eftersom det vid hög efterfrågan kan innebära att meddelanden får vänta tills en målkomponent är redo att bearbeta dem.

## <a name="message-delivery-guarantees"></a>Garantier för meddelandeleverans
Kösystem garanterar vanligtvis att alla meddelanden i kön levereras till en målkomponent. Dessa garantier kan dock fungera på olika sätt:

- **Leverans minst en gång.** Den här metoden garanterar att varje meddelande levereras till minst en av de komponenter som hämtar meddelanden från kön. Observera att det i vissa fall kan hända att samma meddelande levereras mer än en gång. Om det till exempel finns två instanser av en webbapp som hämtar meddelanden från en kö skickas vanligtvis varje meddelande till endast en av dessa instanser. Men om det tar lång tid för en instans att bearbeta meddelandet och en tidsgräns går ut kan meddelandet skickas till den andra instansen också. Koden i din webbapp bör utformas för att ta hänsyn till den här möjligheten.

- **Leverans högst en gång.** Med den här metoden går det inte att garantera att varje meddelande levereras, och det finns en mycket liten risk att det inte kommer fram alls. Till skillnad från hur alternativet ”Leverans minst en gång” fungerar finns det dock ingen risk att meddelandet levereras två gånger. Detta kallas ibland ”automatisk dubblettidentifiering”.

- **Först in först ut (FIFO).** I de flesta meddelandesystem lämnar meddelanden vanligtvis kön i samma ordning som de har lagts till, men du bör fundera på om den här ordningen ska vara garanterad. Om det distribuerade programmet kräver att meddelanden behandlas i exakt rätt ordning måste du välja ett kösystem med FIFO-garanti.

## <a name="transactional-support"></a>Transaktionsstöd
Vissa nära relaterade grupper av meddelanden kan orsaka problem om leveransen misslyckas för ett av meddelandena i gruppen.

Tänk dig exempelvis ett e-handelsprogram. När användaren klickar på knappen **Köp** genereras kanske en serie meddelanden och skickas till olika bearbetningsmål:

- Ett meddelande med orderinformationen skickas till en expedieringscentral
- Ett meddelande med information om totalsumman och betalningen skickas till ett kreditkortsföretag. 
- Ett meddelande med kvittoinformationen skickas till en databas för att generera en faktura för kunden

I det här fallet vi vill vara säkra på att _alla_ meddelanden bearbetas eller att inga av dem bearbetas. Det skulle orsaka en hel del problem om kreditkortsmeddelandet inte levererades och alla våra beställningar slutfördes utan betalning! Du kan undvika dessa typer av problem genom att gruppera de två meddelandena i en transaktion. Meddelandetransaktioner lyckas eller misslyckas som en enda enhet – precis som i databasvärlden. Om leveransen av kreditkortsinformationen misslyckas kommer även orderinformationen att göra det.

## <a name="which-service-should-i-choose"></a>Vilken tjänst bör jag välja?
Eftersom du har förstått att kommunikationsstrategin för den här arkitekturen ska vara ett meddelande måste du välja om du ska använda Azure Storage-köer eller Azure Service Bus. Båda kan användas för att lagra och leverera meddelanden mellan dina komponenter. De har något annorlunda funktionsuppsättningar, vilket innebär att du kan använda den ena, den andra eller båda två beroende på det problem som du vill lösa.

#### <a name="choose-service-bus-queues-if"></a>Välj Service Bus-köer om:

- Du behöver en garanti om leverans högst en gång.
- Du behöver en FIFO-garanti.
- Du behöver gruppera meddelanden i transaktioner.
- Du vill ta emot meddelanden utan att avsöka kön.
- Du behöver ange en modell för rollbaserad åtkomst till köerna.
- Du behöver hantera meddelanden som är större än 64 KB men mindre än 256 KB.
- Storleken på din kö inte kommer att bli större än 80 GB.
- Du vill kunna publicera och använda batchar av meddelanden.

Kölagring har inte riktigt lika många funktioner, men om du inte behöver några av de funktionerna kan det vara ett enklare val. Dessutom är det den bästa lösningen om din app har något av följande krav.

#### <a name="choose-queue-storage-if"></a>Välj Queue-lagring om:

- Du behöver en spårningslogg för alla meddelanden som skickas via kön.
- Du förväntar dig att kön kommer att överskrida 80 GB i storlek.
- Du vill följa förloppet för bearbetning av ett meddelande i kön.

## <a name="summary"></a>Sammanfattning

En kö är en enkel, tillfällig lagringsplats för meddelanden som skickas mellan komponenterna i ett distribuerat program. Använd en kö till att ordna meddelanden och smidigt hantera oväntade toppar i efterfrågan.

Använd Storage-köer om du vill ha ett enkelt kösystem som är lätt att koda. Använd Service Bus-köer för mer avancerade behov.
Anta att du arbetar för en större nyhetsorganisation som rapporterar aviseringar om de senaste nyheterna. Företaget har ett globalt nätverk av journalister som hela tiden skickar uppdateringar via en webbportal och en mobilapp. Ett webbtjänstlager på mellannivå tar sedan dessa aviseringsuppdateringar och publicerar dem online via flera kanaler.

Men det har framkommit att det saknas aviseringar i systemet när viktiga globala händelser inträffar. Det här är ett _enormt_ problem eftersom våra konkurrenter därmed blir först med att leverera dessa nyheter! Du har varit handplockade som företagets viktigaste utvecklare att identifiera och åtgärda problemet.

Mellannivån har tillräckligt med kapacitet för att hantera vanliga belastningar. Men en närmare titt på serverloggarna visar att systemet överbelastas när många journalister försöker ladda upp stora mängder nyhetsinformation samtidigt. Vissa skribenter klagar på att portalen slutar svara, och andra säger att deras nyheter försvunnit helt och hållet. Du har upptäckt ett direkt samband mellan de rapporterade problemen och toppar i efterfrågan på servrar på mellannivå.

Du behöver alltså ett sätt att hantera dessa oväntade toppar. Du vill inte lägga till fler instanser av webbplatsen och webbtjänsten på mellannivå, eftersom det är dyrt och inte behövs under normala förhållanden. Vi skulle kunna sätta igång instanser dynamiskt, men det skulle ta tid och vi skulle få problem med att vänta tills de nya servrarna är igång.

Du kan lösa problemet med hjälp av Azure Queue Storage. En lagringskö är en högpresterande meddelandebuffert som kan fungera som meddelandekö mellan komponenterna på klientdelen (”producenterna”) och på mellannivån (”konsumenten”). 

Våra komponenter på klientdelen placerar ett meddelande för varje ny avisering i en kö. På mellannivån tas dessa meddelanden sedan emot ett i taget från kön för bearbetning. Vid tidpunkter med hög efterfrågan växer kön, men inga nyheter går förlorade och programmet fortsätter att svara. När behovet sjunker tillbaka till normalnivåerna kan webbtjänsten komma ikapp genom att kvarvarande uppgifter i kön gås igenom.

Nu ska vi visa hur du kan hantera hög efterfrågan och förbättra flexibiliteten i dina distribuerade program med Azure Queue Storage.

## <a name="learning-objectives"></a>Utbildningsmål

- Skapa ett Azure Storage-konto med stöd för köer.
- Skapa en kö med C# och Azure Storage-klientbiblioteket för .NET.
- Lägga till, hämta och ta bort meddelanden från en kö med C# och Azure Storage-klientbiblioteket för .NET.
Onlineåterförsäljaren som du arbetar för planerar att inom kort utöka till ett nytt geografiskt område. Flytten utökar din kundbas och dina transaktionsvolymer. Du måste se till att din databas klarar att hantera expansion när så krävs.

Om du har en partitionsstrategi säkerställer det att när din databas behöver växa kan den enkelt göra det och fortsätta att utföra effektiva frågor och transaktioner.

## <a name="what-is-a-partition-strategy"></a>Vad är en strategi för partition?

Om du fortsätter att lägga till nya data till en enskild server eller en enskild partition får du så småningom slut på utrymme. För att förbereda för det behöver du en partitioneringsstrategi för att **skala ut** istället för upp. Att skala ut kallas även för horisontell skalning, och det gör att du kan lägga till fler partitioner till din databas när ditt program behöver det.

Partitionen och utskalningsstrategin i Azure Cosmos DB drivs av partitionsnyckeln, vilket är ett värde som ställs in när du skapar en samling. När partitionsnyckeln är inställd går det inte att ändra den utan att återskapa samlingen, så det är viktigt att välja rätt partitionsnyckel tidigt i utvecklingsprocessen.  

I den här delen får du lära dig att välja en partitionsnyckel som är rätt för ditt scenario och som drar nytta av autoskalningen som Azure Cosmos DB kan göra åt dig.

## <a name="partition-key-basics"></a>Grundläggande om partitionsnycklar

En partitionsnyckel representerar ett värde i din databas som används ofta. I vårt scenario med en onlinebutik är det ett bra val att använda värden `UserID` eller `ProductID` som partitionsnyckeln eftersom den blir unik och kommer sannolikt att användas för att slå upp poster. `UserID` är ett bra val, eftersom ditt program ofta behöver hämta anpassningsinställningar, kundvagn, orderhistorik och profilinformation för användaren, för att bara nämna några. `ProductID` är också ett bra val, eftersom programmet måste fråga lagernivåer, leveranskostnader, färgalternativ, lagerplatser med mera.

En partitionsnyckel bör syfta till att distribuera åtgärder i databasen. Du vill distribuera begäranden för att undvika heta partitioner. En frekvent partition (hot partition) är en enda partition som får många fler begäran än de andra. Det kan skapa en flaskhals för dataflödet. För exempelvis ditt e-handelsprogram skulle den aktuella tiden vara ett dåligt val av partitionsnyckel, eftersom alla inkommande data skulle gå till en enda partitionsnyckel. `UserID` eller `ProductID` skulle passa bättre, eftersom alla användare på webbplatsen troligtvis skulle lägga till och uppdatera sin kundvagn eller profilinformation med ungefär samma frekvens, vilket distribuerar läsningar och skrivningar i alla användarpartitioner. På samma sätt skulle uppdateringar till produktdata även troligen bli jämnt fördelade, vilket gör `ProductID` till en bra val av partitionsnyckel.

Varje partitionsnyckel har ett maximalt lagringsutrymme på 10 GB, vilket är storleken på en fysisk partition i Azure Cosmos DB. Så om din enskilda post för `UserID` eller `ProductID` kommer att överstiga 10 GB ska du fundera på att använda en sammansatt nyckel istället, så varje post blir mindre. Ett exempel på en sammansatt nyckel är `UserID-date`, vilket skulle se ut så här: **CustomerName-08072018**. Metoden med sammansatt nyckel skulle göra det möjligt för dig att skapa en ny partition för varje dag som en användare besökte webbplatsen.

## <a name="best-practices"></a>Bästa praxis

När du försöker bestämma rätt sammansatt nyckel och lösningen inte är uppenbar får du här några tips på vad du kan tänka på.

* Var inte rädd för att ha för många partitionsnycklar. Ju fler partitionsnycklar du har, desto mer skalbarhet får du.

* För att bestämma vilken partitionsnyckel som är bäst för en arbetsbelastning som är mer inriktad på läsning tar du en titt på de tre till fem översta frågorna du tänker använda. Värdet som oftast ingår i WHERE-satsen är en stark kandidat för partitionsnyckeln.

* För arbetsbelastningar som är mer inriktade på skrivning måste du förstå arbetsbelastningens transaktionsbehov, eftersom partitionsnyckeln omfattar transaktioner för flera dokument.

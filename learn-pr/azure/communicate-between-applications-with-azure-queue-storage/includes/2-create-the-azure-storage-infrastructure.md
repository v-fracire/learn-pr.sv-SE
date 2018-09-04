Direkt kommunikation mellan komponenterna i ett distribuerat program kan vara problematisk eftersom den kan avbrytas när nätverksbandbredden är låg eller när efterfrågan är hög.

I exemplet med fotodelning är planen att använda en kö för att eliminera direktlänken mellan dina kundriktade appar och din webbtjänst på mellannivå.

## <a name="what-is-azure-queue-storage"></a>Vad är Azure Queue Storage?

Azure Queue Storage är en Azure-tjänst som implementerar molnbaserade köer. Varje kö har en lista med meddelanden. Programkomponenter kommer åt en kö med hjälp av ett REST-API eller ett Azure-tillhandahållet klientbibliotek. Du kommer normalt har en eller flera _avsändarkomponenter_ och en eller flera _mottagarkomponenter_. Avsändarkomponenterna lägger till meddelande i kön. Mottagarkomponenter hämtar meddelanden som ligger först i kön för bearbetning.

![Lagringsköer](../media-draft/2-queue-overview.png)

Prissättning baseras på köstorleken och antalet åtgärder. Större meddelandeköer kostar mer än mindre köer. Debitering sker även för varje åtgärd, till exempel att lägga till ett meddelande och ta bort ett meddelande. Information om priser finns i [Prissättning för Azure Queues Storage](https://azure.microsoft.com/pricing/details/storage/queues/).

## <a name="why-use-queues"></a>Varför bör jag använda köer?

Köer ökar robustheten genom att tillfälligt lagra väntande meddelanden. När efterfrågan är låg eller normal förblir storleken på kön liten eftersom målkomponenten tar bort meddelanden från kön snabbare än de läggs till. När efterfrågan är hög kan köstorleken öka, men meddelandena går inte förlorade. Målkomponenten kan fånga och tömma kön när efterfrågan blir normal igen.

En enskild kö kan ha en storlek på upp till 500 TB, så den kan potentiellt kan lagra miljontals meddelanden. Måldataflödet för en enskild kö är 2 000 meddelanden per sekund, så den kan hantera stora mängder.

Köer gör att ditt program kan skalas automatiskt och omedelbart när efterfrågan ändras. Det gör att de är användbara för kritiska affärsdata som skulle vara skadliga att förlora. Azure erbjuder många andra tjänster som skalar automatiskt. Till exempel är funktionen **Autoskalning** tillgänglig på Azure VM-skalningsuppsättningar, molntjänster, App Service-planer och App Service-miljöer. På så sätt kan du definiera regler som Azure använder för att identifiera perioder med hög efterfrågan och automatiskt lägga till kapacitet utan hjälp från en administratör. Automatisk skalning svarar på efterfrågan snabbt men inte omedelbart. Däremot hanterar Azure Queue Storage hög efterfrågan omedelbart genom att lagra meddelanden tills bearbetningsresurser blir tillgängliga.

## <a name="what-is-a-message"></a>Vad är ett meddelande?

Ett meddelande i en kö är en bytematris på upp till 64 KB. Meddelandeinnehåll tolkas inte alls av Azure-komponenter.

Om du vill skapa ett strukturerat meddelande kan du formatera meddelandeinnehållet med hjälp av XML eller JSON. Din kod ansvarar för att generera och tolka ditt anpassade format. Du kan till exempel skapa ett anpassat JSON-meddelande som ser ut som följande:

    ```JavaScript
    {
        "Message": {
            "To": "admin@contoso.com",
            "From": "user1@contoso.com",
            "Subject": "Support request",
            "Body": "My computer will not switch on."
        }
    }
    ```

## <a name="storage-account-settings-for-queues"></a>Lagringskontoinställningar för köer

En kö måste vara medlem i ett lagringskonto. När du skapar ett lagringskonto som kommer att innehålla köer bör du överväga följande inställningar:

- Köer är endast tillgängliga som en del av Azure-lagringskonton för generell användning. Du kan inte lägga till dem till Blob Storage-konton.
- Välj en plats som ligger nära antingen källkomponenterna eller målkomponenterna eller helst båda.
- Data i ett Azure Storage-konto replikeras alltid till flera servrar för att skydda mot diskfel och andra maskinvaruproblem. Du kan välja replikeringsstrategi: lokalt redundant lagring (LRS) har en låg kostnad men är sårbar för katastrofer som påverkar ett helt datacenter, medan geo-redundant lagring (GRS) replikerar data till andra Azure-datacenter.
- Prestandanivån avgör hur ditt meddelande lagras: Standard använder magnetiska hårddiskar medan Premium använder SSD-diskar. Välj Standard om du förväntar dig att toppar i efterfrågan kommer att vara kortvariga. Överväg Premium om kölängden ibland blir lång och om du behöver minimera åtkomsttiden för meddelanden.
- Kräv säker överföring om känslig information kan skickas via kön. Den här inställningen ser till att alla anslutningar till kön krypteras med hjälp av Secure Sockets Layer (SSL).

> [!NOTE]
> Inställningen för **Åtkomstnivå** gäller enbart för Blob Storage och påverkar inte köer.

## <a name="summary"></a>Sammanfattning

Många distribuerade program använder direkt kommunikation för att utbyta information. En webbsida kan till exempel anropa en metod i en webbtjänst. Den här strukturen är tillförlitlig så länge kommunikationen sker snabbt mellan dessa komponenter och målkomponenten kan svara i god tid. Om du inte garantera snabb kommunikation eller har problem med hög efterfrågan är en kö ett sätt att lägga till extra robusthet i programmet.
Du har nu lagt till de klientbibliotek som behövs i appen och det har blivit dags att ansluta till ditt Azure Storage-konto.

Om du ska kunna arbeta med data i ett lagringskonto behöver din app två typer av data:

1. En åtkomstnyckel
1. REST-API-slutpunkten

## <a name="security-access-keys"></a>Åtkomstnycklar för säkerhet

Varje lagringskonto har två unika _åtkomstnycklar_ som används till att skydda lagringskontot. Om din app behöver ansluta till flera lagringskonton måste appen ha en åtkomstnyckel för varje sådant lagringskonto.

![En bild som visar ett program som är anslutet till två olika lagringskonton i molnet. Varje lagringskonto kan nås med en unik nyckel.](..\media\6-multiple-accounts.png)

## <a name="rest-api-endpoint"></a>Slutpunkt för REST-API:t

Förutom åtkomstnycklar för autentisering mot lagringskontona behöver appen känna till lagringstjänstens slutpunkter så att den kan utfärda REST-förfrågningar. 

REST-slutpunkten är en kombination av lagringskontots _namn_, datatypen och en känd domän. Exempel:

| Datatyp | Exempel på slutpunkt |
|-----------|------------------|
| Blobar     | `https://[name].blob.core.windows.net/` |
| Köer    | `https://[name].queue.core.windows.net/` |
| Tabeller     | `https://[name].table.core.windows.net/` |
| Filer     | `https://[name].file.core.windows.net/` |

Om du har en anpassad domän kopplad till Azure kan du även skapa en anpassad domänadress för slutpunkten.

## <a name="connection-strings"></a>Anslutningssträngar

Det enklaste sättet att hantera åtkomstnycklar och slutpunkternas webbadresser inom program är att använda **anslutningssträngar för lagringskonton**. I en anslutningssträng finns all anslutningsinformation som behövs i en enda textsträng.

Anslutningssträngar i Azure Storage ser ut ungefär som i exemplet nedan, men med åtkomstnyckeln och kontonamnet för ditt specifika lagringskonto:

```
DefaultEndpointsProtocol=https;AccountName={your-storage};
   AccountKey={your-access-key};
   EndpointSuffix=core.windows.net
```

## <a name="security"></a>Säkerhet

Åtkomstnycklar ger åtkomst till ditt lagringskonto och du ska därför undvika att lämna ut dem till något system eller person som du inte vill ska ha tillgång till ditt lagringskonto. Åtkomstnycklar har samma funktion som användarnamn och lösenord vid inloggning på en dator.

Anslutningsinformation för lagringskonton lagras vanligtvis i en miljövariabel, databas eller konfiguration.

> [!IMPORTANT]
> Observera att det kan vara riskabelt att lagra den här informationen i en konfigurationsfil om du tänker inkludera samma fil i källkodskontrollen och lagra den på en offentlig lagringsplats. Det här är tyvärr ett misstag som många användare gör och som kan leda till att vem som helst kan läsa källkoden på den offentliga lagringsplatsen och hitta anslutningsinformationen till ditt lagringskonto.

Varje lagringskonto har två åtkomstnycklar. Anledningen till detta är att vi vill möjliggöra rotation (och regenerering) av nycklar med jämna mellanrum. Inkludera gärna metoden i ert interna regelverk för att skydda lagringskontona. Den här processen kan köras från Azure Portal eller kommandoradsverktyget för Azure CLI/PowerShell.

Om du roterar en nyckel så ogiltigförklaras det ursprungliga nyckelvärdet omedelbart. Detta gör det lätt att återkalla behörigheten om någon obehörig kommit över nyckeln. Då det finns stöd för två nycklar kan du rotera nycklar utan driftstopp i apparna som använder nycklarna. Appen kan växla över till den alternativa åtkomstnyckeln medan den andra nyckeln återskapas. Om lagringskontot används av flera appar bör samtliga appar använda samma nyckel när den här tekniken används. Här är grundtanken:

1. Uppdatera anslutningssträngarna i programkoden så att de refererar till lagringskontots sekundära åtkomstnyckel.
2. Återskapa den primära åtkomstnyckeln för lagringskontot i Azure Portal eller på kommandoraden.
3. Uppdatera anslutningssträngarna i koden så att de refererar till den nya primärnyckeln.
4. Återskapa den sekundära åtkomstnyckeln på samma sätt.

> [!TIP]
> Vi rekommenderar starkt att du regelbundet roterar dina åtkomstnycklar så att de hålls privata, precis som att du ändrar dina lösenord. Om du använder nyckeln i ett serverprogram kan du lagra åtkomstnyckeln i **Azure Key Vault**. Key Vault har stöd för synkronisering direkt med lagringskontot och roterar nycklarna automatiskt då och då. Med Key Vault får du ett ytterligare säkerhetslager så att din app aldrig behöver arbeta direkt med åtkomstnycklar.

### <a name="shared-access-signatures-sas"></a>Signaturer för delad åtkomst (SAS)

Åtkomstnycklar är den enklaste metoden om du vill autentisera åtkomsten till ett lagringskonto. De ger ändå fullständig åtkomst till allt i lagringskontot, ungefär som ett rotlösenord till en dator.

Lagringskontona har en fristående autentiseringsmekanism som kallas _signaturer för delad åtkomst_. Den har stöd för tidsbegränsade och avgränsade behörigheter i de fall där du vill ge begränsad tillgång till andra. Du bör använda den här metoden när du ger andra användare läs- och skrivbehörighet till data i ditt lagringskonto. I slutet av den här modulen finns länkar till dokumentationen om det här avancerade ämnet.

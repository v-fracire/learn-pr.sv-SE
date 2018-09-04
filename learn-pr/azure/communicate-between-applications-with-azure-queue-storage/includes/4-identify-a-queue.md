Om du vill skriva kod för att få åtkomst till en kö behöver du tre uppgifter:
 - namn på lagringskonto
 - könamn
 - auktoriseringstoken.

I vårt exempel med delning av foton används den här informationen av båda programmen som pratar med kön (webbklientdelen som lägger till meddelanden och medelnivån som bearbetar dem).

## <a name="queue-identity"></a>Köidentitet

Varje kö har ett namn som du tilldelar under skapandet. Namnet måste vara unikt inom ditt lagringskonto, men det behöver inte vara globalt unikt.

Kombinationen av namnet på ditt lagringskonto och könamnet identifierar en kö unikt.

## <a name="access-authorization"></a>Åtkomstauktorisering

Varje begäran till en kö måste auktoriseras. Det finns flera alternativ för att auktorisera klientåtkomst till din köer: Azure Active Directory, delad nyckel, signaturer för delad åtkomst och så vidare. Vi använder auktorisering med delad nyckel eftersom det är ett enkelt sätt att börja arbeta med köer.

Din delade nyckel är tillgänglig i avsnittet **Inställningar** på ditt lagringskonto.

## <a name="access-using-rest"></a>Åtkomst med hjälp av REST

Du kan komma åt en kö med hjälp av ett REST API. Om du vill göra detta använder du en adress baserat på köidentiteten. Formatet för http visas nedan:

```
http://<storage account>.queue.core.windows.net/<queue name> 
```

Du placerar den delade nyckeln i auktoriseringsrubriken för varje begäran.

## <a name="access-using-the-azure-storage-client-library-for-net"></a>Åtkomst med hjälp av Azure Storage-klientbiblioteket för .NET

Azure Storage-klientbiblioteket för .NET är ett bibliotek som tillhandahålls av Microsoft som formulerar REST-begäranden och parsar REST-svar åt dig. Detta minskar avsevärt den mängd kod som du behöver skriva. Åtkomst med hjälp av klientbiblioteket kräver fortfarande samma uppgifter (lagringskontonamn, könamn och delad nyckel), men de är ordnade på olika sätt.

Klientbiblioteket använder en **anslutningssträng** för att upprätta din anslutning. En anslutningssträng är en enskild sträng som kombinerar ett lagringskontonamn och en delad nyckel. Formatet ser ut ungefär så här:

```
DefaultEndpointsProtocol=https;AccountName=<your storage account name>;AccountKey=<your key>;EndpointSuffix=core.windows.net
```

Observera att anslutningssträngen inte innehåller könamnet. Könamnet används senare i koden när du upprättar en anslutning till kön.

Din anslutningssträng är tillgänglig i avsnittet **Inställningar** på ditt lagringskonto.

## <a name="summary"></a>Sammanfattning

Vi använder Azure Storage-klientbiblioteket för att få åtkomst till vår kö. Det innebär att vi behöver hämta en anslutningssträng för vårt lagringskonto. Sedan använder vi anslutningssträngen i både avsändarprogrammet och mottagarprogrammet eftersom båda apparna behöver programmatisk åtkomst till den delade kön.
Nu när vi har ett lagringskonto kan vi titta närmare på hur vi arbetar med kön som kontot ska innehålla.

Du behöver tre uppgifter för att använda en kö:

 1. Namn på lagringskontot
 2. Könamn
 3. Auktoriseringstoken

Den här informationen används av båda programmen som pratar med kön (webbklientdelen som lägger till meddelanden och mellannivån som bearbetar dem).

## <a name="queue-identity"></a>Köidentitet

Varje kö har ett namn som du tilldelar under skapandet. Namnet måste vara unikt inom lagringskontot, men det behöver inte vara globalt unikt (till skillnad från namnet på lagringskontot).

Kombinationen av namnet på lagringskontot och könamnet ger en unik identifikation av kön.

## <a name="access-authorization"></a>Åtkomstauktorisering

Varje begäran till en kö måste auktoriseras och det finns flera alternativ att välja bland.

| Typ av auktorisering | Beskrivning |
|--------------------|-------------|
| **Azure Active Directory** | Du kan använda rollbaserad autentisering och identifiera specifika klienter baserat på AAD-autentiseringsuppgifterna. |
| **Delad nyckel** | Det här kallas ibland för en **kontonyckel** och är en krypterad nyckelsignatur som är associerad med lagringskontot. Varje lagringskonto har två sådana nycklar som kan skickas med varje begäran för att autentisera åtkomsten. Den här metoden fungerar ungefär som ett rotlösenord – den ger _fullständig åtkomst_ till lagringskontot. |
| **Signatur för delad åtkomst** | En signatur för delad åtkomst (SAS) är en genererad URI som ger klienter begränsad åtkomst till objekt i lagringskontot. Du kan begränsa åtkomsten till specifika resurser och behörigheter, och ange ett dataintervall för att automatiskt inaktivera åtkomsten efter en viss tidsperiod.  |

> [!NOTE]
> Vi använder auktorisering med kontonyckel eftersom det är det enklaste sättet att komma igång med köer, men vi rekommenderar att du antingen använder signatur för delad åtkomst (SAS) eller Azure Active Directory (AAD) för appar i produktion.

### <a name="retrieving-the-account-key"></a>Hämta kontonyckeln
 
Du hittar din kontonyckel under **Inställningar > Åtkomstnycklar** för lagringskontot i Azure Portal. Du kan också hämta den via kommandoraden:

```azurecli
az storage account keys list ...
```

```powershell
Get-AzureRmStorageAccountKey ...
```

## <a name="accessing-queues"></a>Åtkomst till köer

Du kan komma åt en kö via ett REST API. Det gör du med en URL som kombinerar lagringskontots namn med domänen `queue.core.windows.net` och sökvägen till kön som du vill arbeta med. Till exempel: `http://<storage account>.queue.core.windows.net/<queue name>`. Du måste ta med en `Authorization`-rubrik i varje begäran. Värdet kan vara någon av de tre typerna av auktorisering.

### <a name="using-the-azure-storage-client-library-for-net"></a>Använda Azure Storage-klientbiblioteket för .NET

Azure Storage-klientbiblioteket för .NET är ett bibliotek som tillhandahålls av Microsoft samt som formulerar REST-begäranden och parsar REST-svar åt dig. Detta minskar avsevärt den mängd kod som du behöver skriva. När du får åtkomst med klientbiblioteket behöver du fortfarande samma uppgifter (lagringskontots namn, könamn och kontonyckel), men de är ordnade på ett annat sätt.

Klientbiblioteket använder en **anslutningssträng** för att upprätta din anslutning. Du hittar anslutningssträngen i avsnittet **Inställningar** för lagringskontot i Azure Portal. Du kan också använda Azure CLI eller PowerShell.

En anslutningssträng är en sträng med ett lagringskontonamn och en kontonyckel. Den måste vara känd för att programmet ska komma åt lagringskontot. Formatet ser ut ungefär så här:

```csharp
string connectionString = "DefaultEndpointsProtocol=https;AccountName=<your storage account name>;AccountKey=<your key>;EndpointSuffix=core.windows.net"
```

> [!WARNING]
> Strängvärdet bör lagras på en säker plats eftersom alla som har åtkomst till den här anslutningssträngen kan göra ändringar i kön.

Observera att anslutningssträngen inte innehåller könamnet. Könamnet anges i koden när du upprättar en anslutning till kön.

Vi hämtar vår anslutningssträng från Azure och konfigurerar ett nytt program som ska använda den.
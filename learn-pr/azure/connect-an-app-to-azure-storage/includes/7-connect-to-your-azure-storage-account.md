Du har nu lagt till de klientbibliotek som behövs i appen och det har blivit dags att ansluta till ditt Azure Storage-konto.

Om du vill arbeta med data i ett lagringskonto kan din app måste ha två typer av data:

1. En åtkomstnyckel
1. REST API-slutpunkt

## <a name="security-access-keys"></a>Snabbtangenter för säkerhet

Varje lagringskonto har två unika _åtkomstnycklar_ som används för att skydda lagringskontot. Om din app behöver ansluta till flera lagringskonton, sedan kräver din app en åtkomstnyckel för varje lagringskonto.

![En bild som visar ett program som är ansluten till två olika lagringskonton i molnet. Varje lagringskonto är tillgänglig med en unik nyckel.](..\media\6-multiple-accounts.png)

## <a name="rest-api-endpoint"></a>REST API-slutpunkt

Förutom åtkomst till nycklar för autentisering till storage-konton, kommer din app behöver veta tjänstslutpunkter för lagring att utfärda REST-begäranden. 

REST-slutpunkten är en kombination av ditt lagringskonto _namn_, datatypen och en känd domän. Exempel:

| Datatyp | Exempel-slutpunkt |
|-----------|------------------|
| Blobar     | `https://[name].blob.core.windows.net/` |
| Köer    | `https://[name].queue.core.windows.net/` |
| Tabell     | `https://[name].table.core.windows.net/` |
| Filer     | `https://[name].file.core.windows.net/` |

Om du har en anpassad domän som är kopplad till Azure, kan du också skapa en anpassad domän-URL för slutpunkten.

## <a name="connection-strings"></a>Anslutningssträngar

Det enklaste sättet att hantera den här informationen är att använda en **lagringskontots anslutningssträng**. En anslutningssträng innehåller alla nödvändiga anslutningsinformationen i en enda textsträng.

Anslutningssträngarna i Azure Storage ser ut ungefär som i exemplet nedan, men med åtkomstnyckeln och kontonamnet för ditt specifika lagringskonto angivet:

```
DefaultEndpointsProtocol=https;AccountName={your-storage};
   AccountKey={your-access-key};
   EndpointSuffix=core.windows.net
```

## <a name="security"></a>Säkerhet

Åtkomstnycklar ger åtkomst till ditt lagringskonto och du ska därför undvika att lämna ut dem till något system eller person som du inte vill ska ha tillgång till ditt lagringskonto. Åtkomstnycklar har samma funktion som användarnamn och lösenord vid inloggning på en dator.

Anslutningsinformation för lagringskonton lagras vanligtvis i en miljövariabel, databas eller konfiguration.

> [!IMPORTANT]
> Det är viktigt att Observera att lagra den här informationen i en konfigurationsfil kan vara farliga om du inkluderar den filen i källkontrollen och lagra den i en offentlig lagringsplats. Det här är tyvärr ett misstag som många användare gör och som kan leda till att vem som helst kan läsa källkoden på den offentliga lagringsplatsen och hitta anslutningsinformationen till ditt lagringskonto.

Varje lagringskonto har två åtkomstnycklar. Anledningen till detta är att vi vill möjliggöra rotation (regenererad) av nycklar med jämna mellanrum. Inkludera gärna metoden i ert interna regelverk för att skydda lagringskontona. Den här processen kan göras från Azure portal eller Azure CLI / PowerShell kommandoradsverktyget.

Om du roterar en nyckel så ogiltigförklaras det ursprungliga nyckelvärdet omedelbart. Detta gör det lätt att återkalla behörigheten om någon obehörig kommit över nyckeln. Då det finns stöd för två nycklar kan du rotera nycklar utan driftstopp i apparna som använder nycklarna i fråga. Din app kan växla till alternativa åtkomstnyckeln medan den andra nyckeln har återskapats. Om lagringskontot används av flera appar bör samtliga appar använda samma nyckel i samband med att den här tekniken används. Här är grunden:

1. Uppdatera anslutningssträngarna i programkoden så att de refererar till lagringskontots sekundära åtkomstnyckel.
2. Återskapa den primära åtkomstnyckeln för ditt lagringskonto med hjälp av verktyget Azure-portalen eller kommandoraden.
3. Uppdatera anslutningssträngarna i koden så att de refererar till den nya primärnyckeln.
4. Återskapa den sekundära åtkomstnyckeln på samma sätt.

> [!TIP]
> Vi rekommenderar starkt att du regelbundet rotera dina åtkomstnycklar för att se till att de förblir privata, precis som att ändra ditt lösenord. Om du använder nyckeln i ett serverprogram, kan du använda en **Azure Key Vault** att lagra åtkomstnyckeln för dig. Nyckelvalv har stöd för att synkronisera direkt till Storage-kontot och automatiskt rotera nycklarna regelbundet. Med ett Key Vault ger ett extra lager av säkerhet, så att din app har aldrig att arbeta direkt med en åtkomstnyckel.

### <a name="shared-access-signatures-sas"></a>Signaturer för delad åtkomst (SAS)

Snabbtangenter är den enklaste metoden för att autentisera åtkomst till ett lagringskonto. Men de ger fullständig åtkomst till något i lagringskontot liknar ett rotlösenord på en dator.

Storage-konton erbjuder en separat autentiseringsmekanism som kallas _signaturer för delad åtkomst_ som stöd för förfallodatum och begränsade behörigheter för scenarier där du måste bevilja begränsad åtkomst. Du bör använda den här metoden när du tillåter att andra användare att läsa och skriva data till ditt lagringskonto. Det finns länkar till dokumentationen om den här avancerade avsnitt i slutet av modulen.

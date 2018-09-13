Du har nu lagt till de klientbibliotek som behövs i appen och det har blivit dags att ansluta till ditt Azure Storage-konto. Men hur kan din app veta vilket konto den ska ansluta till och i vilken region lagringskontot finns? Utan konfiguration kan appen inte veta hur den ansluter till ditt Azure Storage-konto. 

I den här övningen ska du hitta anslutningsinformation om lagringskontot i Azure-portalen och lägga in den i din appkonfiguration så att din app är redo att ansluta till ditt Azure Storage-konto.

## <a name="access-keys"></a>Åtkomstnycklar

Varje lagringskonto har en uppsättning åtkomstnycklar som ger åtkomst till lagringskontot. Om din app behöver ansluta till flera lagringskonton måste appen ha en åtkomstnyckel för varje sådant lagringskonto.

![multiple accounts](..\media-draft\6-multiple-accounts.png)

Eftersom Azure är en molnleverantör behöver appen komplettera åtkomstnycklarna för autentisering till lagringskonton med information om lagringstjänstens slutpunkter som ger åtkomst till ditt lagringskonto via Internet. Det enklaste sättet att hantera den här informationen är att använda en anslutningssträng till lagringskontot, där all den anslutningsinformation som behövs finns sparad i en enda textsträng.

Anslutningssträngarna i Azure Storage ser ut ungefär som i exemplet nedan, men med åtkomstnyckeln och kontonamnet för ditt specifika lagringskonto angivet:

```csharp
DefaultEndpointsProtocol=https;AccountName={your-storage};AccountKey={your-access-key};EndpointSuffix=core.windows.net
```

## <a name="security"></a>Säkerhet

Åtkomstnycklar ger åtkomst till ditt lagringskonto och du ska därför undvika att lämna ut dem till något system eller person som du inte vill ska ha tillgång till ditt lagringskonto. Åtkomstnycklar har samma funktion som användarnamn och lösenord vid inloggning på en dator.

Anslutningsinformation för lagringskonton lagras vanligtvis i en miljövariabel, databas eller konfiguration.

Observera att det kan vara riskabelt att lagra den här informationen i en konfigurationsfil om du tänker inkludera samma fil i källkodskontrollen och lagra den på en offentlig lagringsplats. Det här är tyvärr ett misstag som många användare gör och som kan leda till att vem som helst kan läsa källkoden på den offentliga lagringsplatsen och hitta anslutningsinformationen till ditt lagringskonto.

Lagringskontona har en fristående autentiseringsmekanism med delade åtkomstsignaturer som har stöd för tidsbegränsade och avgränsade behörigheter i de fall där du vill ge andra personer begränsad tillgång. Vi kommer inte att gå närmare in på det här och det är heller inte nödvändig kunskap för den här modulen. Mer information finns dock [här](https://docs.microsoft.com/azure/storage/common/storage-dotnet-shared-access-signature-part-1).

Varje lagringskonto har två åtkomstnycklar. Anledningen till detta är att vi vill möjliggöra rotation (regenererad) av nycklar med jämna mellanrum. Inkludera gärna metoden i ert interna regelverk för att skydda lagringskontona. Den här processen kan köras från Azure-portalen eller Azure CLI.

Om du roterar en nyckel så ogiltigförklaras det ursprungliga nyckelvärdet omedelbart. Detta gör det lätt att återkalla behörigheten om någon obehörig kommit över nyckeln. Då det finns stöd för två nycklar kan du rotera nycklar utan driftstopp i apparna som använder nycklarna i fråga. Appen kan växla över till den alternativa åtkomstnyckeln medan den andra nyckeln regenereras. Om lagringskontot används av flera appar bör samtliga appar använda samma nyckel i samband med att den här tekniken används. Klicka [här](https://docs.microsoft.com/azure/storage/common/storage-create-storage-account#manage-your-storage-access-keys) för mer information.

## <a name="advanced-security-options"></a>Avancerade säkerhetsalternativ

Azure tillhandahåller lagring med hög säkerhet i form av tjänsten Azure Key Vault som specialutformats för att på ett säkert sätt lagra alla slags hemligheter såsom åtkomstnycklar, lösenord och certifikat. Med den här funktionen kan du på ett säkert sätt lagra dina åtkomstnycklar och få dem automatiskt roterade.

Den här funktionen är överkurs för den här modulen, men mer information finns [här](https://docs.microsoft.com/azure/key-vault/key-vault-ovw-storage-keys).

## <a name="summary"></a>Sammanfattning

Om du vill ansluta till ett Azure Storage-konto behöver du två saker. En åtkomstnyckel, som liknar ett användarnamn och ett lösenord och bör behandlas konfidentiellt. Du behöver också en slutpunkt för lagringstjänsten i form av en anslutningssträng.

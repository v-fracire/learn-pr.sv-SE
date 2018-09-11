Azure Key Vault är ett *hemligt arkiv*: en centraliserad molntjänst för lagring av programhemligheter. Key Vault hjälper till att förhindra ovannämnda scenarier genom att lagra programhemligheter på en enda central plats och tillhandahåller säker åtkomst, behörighetskontroll och loggning av användaråtkomst.

De främsta fördelarna med att använda Key Vault är dessa:

- Minskad risk för oavsiktliga hemlighetsläckage genom att hemligheter lagras säkert, utanför konfiguration och källkontroll, och scenarier elimineras där hemligheter kopieras i filer eller klistras in i e-postmeddelanden eller chattar.
- Begränsad åtkomst till hemligheter med åtkomstprinciper som är specialanpassade utifrån de program och personer som behöver dem.
- Centraliserad lagring av hemligheter, så att flera användare och instanser av program kan komma åt hemliga värden som bara behöver uppdateras på ett enda ställe.
- Åtkomstloggning och -övervakning som hjälper dig att förstå hur och när hemligheter används.

Hemligheter lagras i enskilda *valv* som är Azure-resurser med egna konfigurations- och säkerhetsprinciper som du kan skapa med något av de vanliga Azure-hanteringsverktygen som Azure-portalen eller Azure CLI. Hemlighetsåtkomst och valvhantering sker via ett REST-API som även stöds av alla Azure-hanteringsverktyg samt klientbibliotek som är tillgängliga på många populära språk. Varje valv har en unik webbadress där dess API finns.

> [!IMPORTANT]
> **Key Vault är utformat för att lagra konfigurationshemligheter för serverprogram.** Det är inte avsett för att lagra data som tillhör appanvändarna, och det bör inte användas i en apps klientsida. Det återspeglas i dess prestandaegenskaper, API och kostnadsmodell.
>
> Användardata ska lagras någon annanstans, som i en Azure SQL-databas med transparent datakryptering eller ett lagringskonto med Storage Service Encryption. Du kan förvara hemligheter som används av ditt program för att få åtkomst till dessa datalager.

## <a name="what-is-a-secret-in-key-vault"></a>Vad är en hemlighet i Key Vault?

I Key Vault är en hemlighet ett namn-värde-par med strängar. Hemliga namn måste vara mellan 1 och 127 tecken långa, får endast innehålla alfanumeriska tecken och streck och måste vara unikt inom ett valv. Ett hemligt värde kan vara valfri UTF-8-sträng upp till 25 KB.

> [!TIP]
> Hemliga namn måste inte betraktas som särskilt hemliga i sig. Du kan lagra dem i appens konfiguration om implementationen anropar den. Samma gäller för valvnamn och webbadresser.

> [!NOTE]
> Key Vault stöder ytterligare två typer av hemligheter utöver strängar &mdash; *nycklar* och *certifikat* &mdash; och tillhandahåller praktiska funktioner som är specifika för deras användningsområden. Den här modulen täcker inte dessa funktioner och koncentrerar sig på hemliga strängar som lösenord och anslutningssträngar.

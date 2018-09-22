Hemligheter är inte hemligheter om de delas med alla. Att lagra konfidentiella objekt som anslutningssträngar, säkerhetstoken, certifikat och lösenord i din kod är en ren inbjudan till att stjäla och använda dem till något som inte är din avsikt. Till och med att lagra den här typen av data i din webbkonfiguration är en dålig idé – det tillåter i stort sätt vem som helst med åtkomst till källkoden eller webbservern att komma åt dina privata data.

I stället bör du alltid placera dessa hemligheter i **Azure Key Vault**.

## <a name="what-is-azure-key-vault"></a>Vad är Azure Key Vault?
Azure Key Vault är ett *hemligt arkiv*: en centraliserad molntjänst för lagring av programhemligheter. Key Vault håller dina konfidentiella data säkra genom att lagra programhemligheter på en enda central plats och tillhandahåller säker åtkomst, behörighetskontroll och loggning av användaråtkomst.

Hemligheter lagras i enskilda *valv* som alla har sina egna konfigurations- och säkerhetsprinciper för åtkomstkontroll. Du kan sedan komma åt dina data via ett REST-API eller en klient-SDK som är tillgänglig för de flesta språk.

> [!IMPORTANT]
> **Key Vault är utformat för att lagra konfigurationshemligheter för serverprogram.** Det är inte avsett för att lagra data som tillhör appanvändarna, och det bör inte användas i en apps klientsida. Det återspeglas i dess prestandaegenskaper, API och kostnadsmodell.
>
> Användardata ska lagras någon annanstans, som i en Azure SQL-databas med transparent datakryptering eller ett lagringskonto med Storage Service Encryption. Du kan förvara hemligheter som används av ditt program för att få åtkomst till dessa datalager.

## <a name="why-use-a-key-vault-for-my-secrets"></a>Varför bör jag använda ett Key Vault för mina hemligheter?

Nyckelhantering och lagring av hemligheter kan vara komplicerat och felbenäget när det utförs manuellt. Att rotera certifikat manuellt innebär potentiellt att de saknas i flera timmar eller dagar. Som nämnts ovan innebär sparande av dina anslutningssträngar i en konfigurationsfil eller kodlagringsplats att någon kan stjäla dina autentiseringsuppgifter.

Key Vault gör att användare kan lagra anslutningssträngar, hemligheter, lösenord, certifikat, åtkomstprinciper, fillås (som skrivskyddar objekt i Azure) och automatiseringsskript.  Det loggar även åtkomst och aktivitet, gör att du kan övervaka åtkomstkontroll (IAM) i din prenumeration och erbjuder verktyg för diagnostik, mått, aviseringar och felsökning så att du får den åtkomst du behöver.

Läs mer om hur du använder ett Azure Key Vault i [Hantera hemligheter i dina serverappar med Azure Key Vault](../../manage-secrets-with-azure-key-vault/index.yml).

## <a name="summary"></a>Sammanfattning

Du kan slippa stöld av autentiseringsuppgifter, manuell nyckelrotation och certifikatförnyelse genom att hantera dina hemligheter väl med Azure Key Vault.

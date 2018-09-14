Hemligheter är inte hemligheter som delas med alla. Lagrar konfidentiell objekt och anslutningssträngar kan säkerhetstoken, certifikat och lösenord i din kod bara bjuder in andra att ta dem och använda dem för något annat än som du tänkt dem för. Även lagra den här typen av data i din Webbkonfiguration är en felaktig idé – du i stort sett vilket gör att alla som har åtkomst till källkoden eller web serveråtkomst till dina privata data.

I stället bör du alltid placera dessa hemligheter i **Azure Key Vault**.

## <a name="what-is-azure-key-vault"></a>Vad är Azure Key Vault
Azure Key Vault är ett *hemligt arkiv*: en centraliserad molntjänst för lagring av programhemligheter. Key Vault håller dina känsliga data säkert genom att hålla programhemligheter i en enda central plats och ger säker åtkomst, kontrollen behörighet och åtkomst-loggning.

Hemligheter är lagrade på enskilda *valv*, var och en med sin egen konfigurations- och säkerhetsprinciper för åtkomstkontroll. Du kan sedan få till dina data via ett REST-API eller en klient-SDK tillgänglig för de flesta språk.

> [!IMPORTANT]
> **Key Vault är utformat för att lagra konfigurationshemligheter för serverprogram.** Det är inte avsett för att lagra data som tillhör appanvändarna, och det bör inte användas i en apps klientsida. Det återspeglas i dess prestandaegenskaper, API och kostnadsmodell.
>
> Användardata ska lagras någon annanstans, som i en Azure SQL-databas med transparent datakryptering eller ett lagringskonto med Storage Service Encryption. Du kan förvara hemligheter som används av ditt program för att få åtkomst till dessa datalager.

## <a name="why-use-a-key-vault-for-my-secrets"></a>Varför ska man använda ett Key Vault för min hemligheter

Nyckelhantering och lagra hemligheter kan vara komplicerat och felbenägna när utföras manuellt. Rotera certifikat manuellt innebär potentiellt utan utan några timmar eller dagar. Som nämnts ovan är sparande av dina anslutningar strängar i konfigurationen fil eller kod databasen innebär att någon kan stjäla dina autentiseringsuppgifter.

Key Vault kan du lagra anslutningssträngar, hemligheter, lösenord, certifikat, åtkomstprinciper, fillås (skrivskydda objekt i Azure) och automatiserade skript.  Också loggas åtkomst och aktivitet, kan du övervaka åtkomstkontroll (IAM) i din prenumeration, och den har också diagnostik, mått, aviseringar och felsökningsverktyg för att se till att du har den åtkomst som du behöver.

![Azure Key Vault](../media-draft/Key-Vault.png)

<!-- TODO: get link to TC module -->
<!--
You can learn more about using a Key Vault in the module [Manage secrets in your server apps with Azure Key Vault](learn/modules/manage-secrets-with-azure-key-vault).-->

## <a name="summary"></a>Sammanfattning

Autentiseringsuppgifter för stöld och rotation av manuell förnyelse kan vara en sak i det förflutna om som du hanterar dina hemligheter, med Azure Key Vault-certifikat.

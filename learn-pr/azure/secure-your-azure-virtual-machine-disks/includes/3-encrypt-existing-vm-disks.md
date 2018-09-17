Anta att ditt företag har beslutat att implementera Azure Disk Encryption (ADE) för alla virtuella datorer. Du behöver utvärdera hur du distribuerar kryptering till befintliga virtuella datorvolymer.

Här ska vi titta på kraven för ADE och de steg som behövs för att kryptera diskar på befintliga virtuella Windows-datorer.

## <a name="azure-disk-encryption-prerequisites"></a>Förhandskrav för Azure Disk Encryption

Innan du kan kryptera din första virtuella datordisk så måste du göra följande:

1. Skapa ett nyckelvalv
1. Konfigurera ett Azure AD-program (Microsoft Azure Active Directory) samt ange tjänstens huvudnamn
1. Ange nyckelvalvets åtkomstprincip för Azure AD-appen
1. Ställa in avancerade åtkomstprinciper för nyckelvalvet

### <a name="azure-key-vault"></a>Azure Key Vault

De krypteringsnycklar som används av ADE kan lagras i ett Azure Key Vault. Azure Key Vault är ett verktyg för att lagra och komma åt hemligheter på ett säkert sätt. En hemlighet är något som du vill begränsa åtkomst till, till exempel API-nycklar, lösenord eller certifikat. Det ger högst tillgänglig, skalbar och säker lagring i FIPS (Federal Information Processing Standards) 140-2 nivå 2-verifierade Maskinvarusäkerhetsmoduler (HSM). Med Key Vault kan du behålla fullständig kontroll över de nycklar som används för att kryptera dina data, och även hantera och granska din nyckelanvändning. Du kan konfigurera och hantera ditt nyckelvalv via Microsoft Azure-portalen, Azure PowerShell och Azure CLI.

>[!NOTE]
> Azure Disk Encryption kräver att ditt nyckelvalv och dina virtuella datorer finns i samma Azure-region. Det säkerställer att krypteringshemligheter inte korsar regionala gränser.

### <a name="azure-ad-application-and-service-principal"></a>Azure AD-program och tjänstens huvudnamn

För att få åtkomst till eller ändra resurser, exempelvis krypteringskonfigurationen för en virtuell dator med skript eller kod, så måste du först konfigurera ett **Azure Active Directory-program (AD)**. Azure AD är en molnbaserad katalog- och identitetshanteringstjänst för flera klientorganisationer. I den kombineras viktiga katalogtjänster, åtkomsthantering för program och identitetsskydd i en enda lösning.

Du behöver också ett **huvudnamn för Azure-tjänsten**. Huvudnamnen för tjänsten är de tjänstkonton som används för att köra skript eller kod. På så sätt kan du tilldela särskilda behörigheter och omfattningar som behövs för att köra en uppgift mot en viss Azure-resurs.

Azure AD innehåller två element: programobjektet är en **_definition_** av programmet (vad det gör) och tjänstens huvudnamn är den **_specifika instansen_** av programmet.

Den här metoden överensstämmer med principen om **lägsta behörighet**, där de behörigheter som tilldelats appen begränsas till de lägsta som krävs för att appen ska kunna utföra sina uppgifter.

Du kan konfigurera och hantera Azure AD-program och tjänstens huvudnamn via Microsoft Azure-portalen, Azure PowerShell och Azure CLI.

### <a name="key-vault-access-policies"></a>Åtkomstprinciper för nyckelvalvet

Innan du kan lagra krypteringsnycklar i ett nyckelvalv kräver ADE information om det **klient-ID** och den **klienthemlighet** för Azure Active Directory-programmet som har behörighet att skriva till nyckelvalvet.

Du behöver också ge Azure åtkomst till krypteringsnycklarna i ditt nyckelvalv så att de görs tillgängliga för den virtuella datorn för start och dekryptering av volymerna.

## <a name="set-key-vault-advanced-access-policies"></a>Ställa in avancerade åtkomstprinciper för nyckelvalvet

**Avancerade åtkomstprinciper** aktivera diskkryptering på nyckelvalvet och utan dem så kommer krypteringsdistributioner att misslyckas. 

Det finns tre principer som måste aktiveras:

- **Key Vault för diskkryptering**. Krävs för Azure Disk Encryption.
- **Key Vault för distribution**. Ger resursleverantören Microsoft.Compute behörighet att hämta hemligheter från nyckelvalvet. Den här principen behövs när du skapar en virtuell dator.
- **Key Vault för malldistribution, om sådan behövs**. Ger Azure Resource Manager behörighet att hämta hemligheter från nyckelvalvet. Den här principen behövs när du använder Azure Resource Manager-mallar för distribution av virtuella datorer.

Nyckelvalvets åtkomstprinciper kan konfigureras och hanteras via Microsoft Azure-portalen, Azure PowerShell eller Azure CLI.

### <a name="what-is-the-azure-disk-encryption-prerequisites-configuration-script"></a>Vad är förhandskonfigurationsskriptet för Azure Disk Encryption?

**Förhandskonfigurationsskriptet för Azure Disk Encryption** konfigurerar alla (eller valfritt antal) förhandskrav för kryptering. Skriptet ser även till att ditt nyckelvalv befinner sig i samma region som den virtuella datorn du ska kryptera. Det skapar en resursgrupp och ett nyckelvalv, och anger nyckelvalvets åtkomstprincip. Skriptet skapar även ett resurslås på nyckelvalvet för att skydda det mot oavsiktlig borttagning.

## <a name="encrypting-an-existing-vm-disk"></a>Kryptera en befintlig virtuell datordisk

Det finns två steg du behöver utföra för att kryptera en befintlig virtuell datordisk när du använder förhandskonfigurationsskriptet för Azure Disk Encryption:

1. Köra förhandskonfigurationsskriptet för Azure Disk Encryption
1. Kryptera den virtuella Azure-datorn i PowerShell

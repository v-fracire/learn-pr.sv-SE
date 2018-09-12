Anta att ditt företag har beslutat att implementera Azure Disk Encryption (ADE) för alla virtuella datorer. Du behöver utvärdera hur du distribuerar kryptering till befintliga virtuella datorvolymer.

Här ska vi titta på kraven för ADE och de steg som behövs för att kryptera diskar på befintliga virtuella Windows-datorer.

## <a name="azure-disk-encryption-prerequisites"></a>Förhandskrav för Azure Disk Encryption

Innan du kan kryptera din första virtuella datordisk så måste du

1. Skapa ett nyckelvalv
1. Konfigurerar ett Azure AD-program och tjänstens huvudnamn
1. Ange nyckelvalvets åtkomstprincip för Azure AD-appen
1. Ställa in avancerade åtkomstprinciper för nyckelvalvet

### <a name="azure-key-vault"></a>Azure Key Vault

De krypteringsnycklar som används av ADE kan lagras i ett Azure Key Vault. Azure Key Vault är ett verktyg för att lagra och komma åt hemligheter på ett säkert sätt. En hemlighet är något som du vill begränsa åtkomst till, till exempel API-nycklar, lösenord eller certifikat. Det ger högt tillgänglig och skalbar och säker lagring i FIPS (Federal Information Processing Standards) 140-2 nivå 2-verifierade Maskinvarusäkerhetsmoduler (HSM). Med Key Vault kan du behålla fullständig kontroll över de nycklar som används för att kryptera dina data och du kan hantera och granska din nyckelanvändning. Du kan konfigurera och hantera ditt nyckelvalv med Azure-portalen, Azure PowerShell och Azure CLI.

>[!NOTE]
> Azure Disk Encryption kräver att ditt Nyckelvalv och dina virtuella datorer finns i samma Azure-region. Det säkerställer att krypteringshemligheter inte korsar regionala gränser.

### <a name="azure-ad-application-and-service-principal"></a>Azure AD-program och tjänstens huvudnamn

För att få åtkomst till eller ändra resurser, exempelvis krypteringskonfigurationen för en virtuell dator med skript eller kod så måste du först ställa in ett **Azure Active Directory-program (AD)**. Azure Active Directory (Azure AD) är en molnbaserad katalogtjänst för identitetshantering för flera innehavare som kombinerar viktiga katalogtjänster, åtkomsthantering för program och identitetsskydd i en och samma lösning.

Du behöver också en Azure **tjänsts huvudnamn**. Tjänstens huvudnamn är tjänstkonton du använder för att köra skript eller kod så att du kan tilldela särskilda behörigheter och omfattningar som behövs för att köra uppgiften mot en viss Azure-resurs.

Det finns två element i Azure AD. Programobjektet är **_definitionen_** av programmet (vad det gör) och tjänstens huvudnamn är den **_specifika instansen_** av programmet.

Den här metoden överensstämmer med principen om **lägsta behörighet** där de behörigheter som tilldelats appen är begränsade till det minsta som krävs för att appen ska kunna utföra sina uppgifter.

Du kan konfigurera och hantera Azure AD-program och tjänstens huvudnamn med Azure-portalen, Azure PowerShell och Azure CLI.

### <a name="key-vault-access-policies"></a>Åtkomstprinciper för nyckelvalvet

Innan du kan lagra krypteringsnycklar i Key Vault, kräver ADE information om det **klient-ID** och den **Klienthemlighet** för Azure Active Directory-programmet som har behörighet att skriva till Key Vault.

Du behöver också ge Azure åtkomst till krypteringsnycklarna i ditt nyckelvalv så att de görs tillgängliga för den virtuella datorn för start och dekryptering av volymerna.

## <a name="set-key-vault-advanced-access-policies"></a>Ställa in avancerade åtkomstprinciper för nyckelvalvet

**Avancerade åtkomstprinciper** aktivera diskkryptering på nyckelvalvet och utan dem så kommer krypteringsdistributioner att misslyckas. 

Det finns tre principer som måste aktiveras:

- **Key Vault för diskkryptering** krävs för Azure Disk Encryption.
- **Key Vault för distribution av** låter Microsoft.Compute-resursprovidern hämta hemligheter från nyckelvalvet. Den här principen behövs när du skapar en virtuell dator.
- **Key Vault för malldistribution, vid behov** låter Azure Resource Manager hämta hemligheter från nyckelvalvet. Den här principen är obligatorisk när du använder ARM-mallar för distribution av virtuella datorer.

Nyckelvalvets åtkomstprinciper kan konfigureras och hanteras med hjälp av Azure-portalen, Azure PowerShell eller Azure CLI.

### <a name="what-is-the-azure-disk-encryption-prerequisites-configuration-script"></a>Vad är förhandskonfigurationsskriptet för Azure Disk Encryption

**Förhandskonfigurationsskriptet för Azure Disk Encryption** konfigurerar alla (eller så många du vill) av förhandskraven för kryptering. Skriptet ser även till att ditt Key Vault befinner sig i samma region som den virtuella datorn du ska kryptera. Det skapar en resursgrupp, ett nyckelvalv och anger nyckelvalvets åtkomstprincip. Skriptet skapar även ett resurslås på nyckelvalvet för att skydda det mot oavsiktlig borttagning.

## <a name="encrypting-an-existing-vm-disk"></a>Kryptera en befintlig virtuell datordisk

Det finns två steg för att kryptera en befintlig virtuell datordisk när du använder **förhandskonfigurationsskriptet för Azure Disk Encryption**:

1. Kör förhandskonfigurationsskriptet för Azure Disk Encryption.
1. Kryptera den virtuella Azure-datorn i PowerShell

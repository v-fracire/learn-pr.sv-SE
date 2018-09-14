Anta att ditt företag har beslutat att implementera Azure Disk Encryption (ADE) för alla virtuella datorer. Du behöver utvärdera hur du distribuerar kryptering till befintliga virtuella datorvolymer.

Här ska vi titta på kraven för ADE och de steg som behövs för att kryptera diskar på befintliga virtuella Windows-datorer.

## <a name="azure-disk-encryption-prerequisites"></a>Krav för Azure Disk Encryption

Innan du kan kryptera din första VM-disk, måste du:

1. Skapa ett nyckelvalv.
1. Konfigurera ett Azure Active Directory (Azure AD)-program och tjänstens huvudnamn.
1. Ange nyckelvalvets åtkomstprincip för Azure AD-app.
1. Ange nyckelvalv avancerade åtkomstprinciper.

### <a name="azure-key-vault"></a>Azure Key Vault

Krypteringsnycklarna som används av ADE kan lagras i Azure Key Vault. Azure Key Vault är ett verktyg för att lagra och komma åt hemligheter på ett säkert sätt. En hemlighet är något som du vill begränsa åtkomst till, till exempel API-nycklar, lösenord eller certifikat. Detta ger hög tillgänglighet och skalbar säker lagring, i FIPS Federal Information Processing Standards () 140-2 Level 2-verifierade Maskinvarusäkerhetsmoduler (HSM). Med Key Vault kan du behålla fullständig kontroll över de nycklar som används för att kryptera dina data och du kan hantera och granska din nyckelanvändning. Du kan konfigurera och hantera ditt nyckelvalv med Azure-portalen, Azure PowerShell och Azure CLI.

>[!NOTE]
> Azure Disk Encryption kräver att ditt nyckelvalv och dina virtuella datorer finns i samma Azure-region; Detta säkerställer att kryptering hemligheter inte mellan regionala gränser.

### <a name="azure-ad-application-and-service-principal"></a>Azure AD-program och tjänstens huvudnamn

För att få åtkomst till eller ändra resurser, exempelvis krypteringskonfigurationen för en virtuell dator med skript eller kod så måste du först ställa in ett **Azure Active Directory-program (AD)**. Azure AD är en flera innehavare, molnbaserad katalog och identity management-tjänsten. Den kombinerar viktiga katalogtjänster, åtkomsthantering för program och identitetsskydd i en och samma lösning.

Du behöver också en Azure **tjänsts huvudnamn**. Tjänstens huvudnamn är service-konton som används för att köra skript eller kod. Förutom att du kan tilldela särskilda behörigheter och omfång som behövs för att köra uppgiften mot en viss Azure-resurs.

Det finns två element i Azure AD: programobjektet är den **_definition_** av programmet (den gör) och tjänsten huvudnamn är den **_specifika instansen_**  av programmet.

Den här metoden överensstämmer med principen om **lägsta behörighet** där de behörigheter som tilldelats appen är begränsade till det minsta som krävs för att appen ska kunna utföra sina uppgifter.

Du kan konfigurera och hantera Azure AD-program och tjänstens huvudnamn med Azure-portalen, Azure PowerShell och Azure CLI.

### <a name="key-vault-access-policies"></a>Åtkomstprinciper för nyckelvalvet

Innan du kan lagra krypteringsnycklar i ett nyckelvalv, ADE kräver information på den **klient-ID** och **Klienthemlighet** för Azure AD-programmet som är tillåtet att skriva till nyckelvalvet.

Du behöver också ge Azure åtkomst till krypteringsnycklarna i ditt nyckelvalv så att de görs tillgängliga för den virtuella datorn för start och dekryptering av volymerna.

## <a name="set-key-vault-advanced-access-policies"></a>Ställa in avancerade åtkomstprinciper för nyckelvalvet

**Avancerade åtkomstprinciper** aktivera diskkryptering på nyckelvalvet och utan dem så kommer krypteringsdistributioner att misslyckas. 

Det finns tre principer som måste aktiveras:

- **Key Vault för diskkryptering**. Krävs för Azure Disk Encryption.
- **Key Vault för distribution av**. Gör det möjligt att hämta hemligheter från nyckelvalvet Microsoft.Compute-resursprovidern. Den här principen krävs när du skapar en virtuell dator.
- **Key Vault för malldistribution, om det behövs**. Gör det möjligt för Azure Resource Manager för att hämta hemligheter från nyckelvalvet. Den här principen är obligatorisk när du använder Azure Resource Manager-mallar för distribution av virtuella datorer.

Nyckelvalvets åtkomstprinciper kan konfigureras och hanteras med hjälp av Azure-portalen, Azure PowerShell eller Azure CLI.

### <a name="what-is-the-azure-disk-encryption-prerequisites-configuration-script"></a>Vad är nödvändiga konfigurationsskriptet för Azure Disk Encryption

Den **nödvändiga konfigurationsskriptet för Azure Disk Encryption** ställer in alla (eller så många som du vill) av förutsättningarna för kryptering. Skriptet innebär också att nyckelvalvet är i samma region som den virtuella datorn du ska kryptera. Den skapar en resursgrupp och nyckelvalv och ange nyckelvalvets åtkomstprincip. Skriptet skapar även ett resurslås på nyckelvalvet för att skydda det mot oavsiktlig borttagning.

## <a name="encrypting-an-existing-vm-disk"></a>Kryptera en befintlig virtuell datordisk

Det finns två steg för att kryptera en befintlig VM-disk när du använder Azure Disk Encryption nödvändiga konfigurationsskriptet:

1. Kör krävs för Azure Disk Encryption konfigurationsskript.
1. Kryptera virtuella Azure-datorer i PowerShell.

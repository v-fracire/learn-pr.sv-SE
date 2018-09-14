För de flesta organisationer är data den mest värdefulla och oersättliga tillgången. Kryptering fungerar som den senaste och starkaste försvarslinjen i en skiktad säkerhetsstrategi. 

Contoso leverans vet att kryptering är det enda skydd som har sina data när de lämnar datacentret medan den springer till de mobila apparna.

## <a name="what-is-encryption"></a>Vad är kryptering?

Kryptering är en process där data görs oläsliga och oanvändbara. För att krypterade data ska kunna användas eller läsas måste de *dekrypteras*, vilket kräver användning av en hemlig nyckel. Det finns två översta typer av kryptering: **symmetriska** och **asymmetrisk**.

Vid symmetrisk kryptering används samma nyckel för att kryptera och dekryptera data. Ett exempel kan vara ett lösenordshanteringsprogram. Du anger dina lösenord och de krypteras med din egen personliga nyckel (din nyckel härleds ofta från ditt huvudlösenord). När data behöver hämtas används samma nyckel och informationen dekrypteras.

Vid asymmetrisk kryptering används ett par med en offentlig nyckel och en privat nyckel. Någon av nycklarna kan kryptera men en enda nyckel kan inte dekryptera sin egen krypterade data. Om du vill dekryptera, måste den parade nyckeln. Asymmetrisk kryptering används för till exempel säkerhet TLS (Transport Layer) (används på HTTPS) och logga data.

Både symmetrisk och asymmetrisk kryptering är viktigt för att skydda data på rätt sätt. 

Inom kryptering skiljer man på kryptering i vila och kryptering under överföring.

## <a name="encryption-in-transit"></a>Kryptering under överföring

Data under överföring är de data som aktivt flyttar från en plats till en annan, till exempel via Internet eller via ett privat nätverk. Säker överföring kan hanteras av flera olika lager. Det kan göras genom att kryptera data på programnivån innan de skickas över ett nätverk. HTTPS är ett exempel på programlagret i överföringen kryptering. 

Du kan också ställa in en säker kanal, som ett virtuellt privat nätverk (VPN), på nätverksnivå, att överföra data mellan två system. 

Kryptering av data under överföring skyddar data från utomstående observatörer och tillhandahåller en mekanism för överföring av data samtidigt som risken för exponering minskar. 

<!--TODO: replace with final media which was submitted for Design-for-security-in-azure -->
![Kryptering under överföring](../media-COPIED-FROM-DESIGNFORSECURITY/encryption-in-transit.png)


## <a name="encryption-at-rest"></a>Kryptering i vila

Vilande data är data som har lagrats på ett fysiskt medium. Detta kan vara data som lagras på en serverdisk, data som lagras i en databas eller data som lagras i ett lagringskonto. Oavsett lagringsmekanism garanterar krypteringen av vilande data att dessa lagrade data inte kan läsas utan de nycklar och hemligheter som krävs för att dekryptera dem. Om en angripare har att hämta en hårddisk med krypterade data och har inte åtkomst till krypteringsnycklarna, skulle angriparen inte äventyra data utan bra svårt.

Vilka data som krypteras, vad de används för och hur viktiga de är för organisationen kan variera. Det här kan vara finansiell information som är viktiga för verksamheten, immateriell egendom som har utvecklats av företag, personliga data om kunder och anställda som verksamheten lagrar och till och med nycklar och hemligheter som används för kryptering av data .

<!--TODO: replace with final media which was submitted for Design-for-security-in-azure -->
![Kryptering i vila](../media-COPIED-FROM-DESIGNFORSECURITY/encryption-at-rest.png)

## <a name="encryption-on-azure"></a>Kryptering i Azure

Nu ska vi se hur du kan kryptera data i olika tjänster i Azure.

### <a name="encrypt-raw-storage"></a>Kryptera lagring av rådata

Med Kryptering för lagringstjänst (SSE) för vilande data kan du skydda dina data i enlighet med säkerhets- och efterlevnadskraven i din organisation. Med den här funktionen krypterar Azures lagringsplattform automatiskt dina data innan de lagras i Azure Managed Disks, Azure Blob Storage, Azure Files eller Azure Queue Storage, och dekrypterar dem innan de hämtas. Hanteringen av kryptering, kryptering i vila, dekryptering och nyckelhantering i Kryptering för lagringstjänst är transparent för program som använder tjänsterna.

### <a name="encrypt-virtual-machines"></a>Kryptera virtuella datorer

Kryptering av lagringstjänst ger krypteringnivån skydd för data som skrivs till fysisk disk, men hur kan du skydda de virtuella hårddiskarna (VHD) för virtuella datorer? Om en angripare fick åtkomst till dina Azure-prenumeration och exfiltrated virtuella hårddiskar på virtuella datorer, hur skulle du se till att de skulle kunna komma åt data som lagras på den virtuella Hårddisken?

Azure Disk Encryption är en funktion som hjälper dig att kryptera din Windows- och Linux IaaS VM-diskar. Azure Disk Encryption använder branschstandarden BitLocker-funktion i Windows och dm-crypt i Linux för att kryptera volymer OS och datadiskar. Lösningen är integrerad med Azure Key Vault för att styra och hantera diskkrypteringsnycklarna och hemligheterna (och du kan använda hanterade tjänstidentiteter för åtkomst till Key Vault).

För Contoso leverans var med hjälp av virtuella datorer en av sina första flyttar mot molnet. Med alla de virtuella hårddiskarna krypterad är ett mycket enkelt och låg påverkan sätt att se till att de gör allt de kan för att skydda sina data.

### <a name="encrypt-databases"></a>Kryptera databaser

Med transparent datakryptering (TDE) kan du skydda Azure SQL Database och Azure Data Warehouse mot skadlig aktivitet. TDE utför realtidskryptering och realtidsdekryptering av databasen, tillhörande säkerhetskopior och transaktionsloggfiler i vila, utan att några ändringar krävs i programmet. Som standard är TDE aktiverat för alla nyligen distribuerade Azure SQL Database-instanser.

TDE krypterar lagringen av en hel databas med hjälp av en symmetrisk nyckel kallad databaskrypteringsnyckeln. Som standard Azure tillhandahåller en unik krypteringsnyckel per logisk SQL Server-instans och hanterar all information. Ta med din egen nyckel (BYOK) stöds även med nycklar som lagras i Azure Key Vault.

Eftersom TDE är aktiverat som standard, kan Contoso leverans vara säker på att de har rätt skydd för data som lagras i sina databaser.

### <a name="encrypt-secrets"></a>Kryptera hemligheter

Vi har sett att krypteringstjänster alla använder nycklar för att kryptera och dekryptera data, så gör vi ser till att nycklarna själva skyddas? Företag kan också ha lösenord, anslutningssträngar eller andra känsliga uppgifter som de behöver för att lagra på ett säkert sätt.

Azure Key Vault är en molntjänst som fungerar som ett säkert lager för hemligheter. Du kan skapa flera säkra containrar, kallade valv, i Key Vault. De här valven stöds av säkerhetsmoduler på maskinvarunivå (HSM:er). Med valv så minskar risken för att säkerhetsinformation förloras av misstag eftersom lagringen av hemligheter centraliseras. Nyckelvalv kan du också styra och logga åtkomst till något som lagras i dem. Azure Key Vault kan hantera förfrågningar om och förnyande av TLS-certifikat, att tillhandahålla de funktioner som krävs för en robust certifikatlösning för hantering av livscykeln. Key Vault har stöd för alla typer av hemligheter. Dessa hemligheter kan vara lösenord, Databasautentiseringsuppgifter, API-nycklar och certifikat.

Eftersom Azure AD-identiteter kan beviljas åtkomst för att använda Azure Key Vault-hemligheter, program med hanterade tjänstidentiteter aktiverat kan automatiskt och hämta smidigt hemligheter som de behöver.

Kryptering är ofta den sista försvarslinje mot angripare och är en viktig del av en överlappande tillvägagångssättet för att skydda dina system. Azure tillhandahåller inbyggda funktioner och tjänster för att kryptera och skydda data mot oavsiktlig exponering. Skydda kundernas data lagras i Azure-tjänster är av avgörande betydelse för Microsoft och ska tas med i alla design. Grundläggande tjänster som Azure Storage, Azure Virtual Machines, Azure SQL Database och Azure Key Vault hjälper dig att skydda din miljö genom kryptering.
För de flesta organisationer är data den mest värdefulla och oersättliga tillgången. Kryptering fungerar som den sista och starkaste försvarslinjen i en skiktad säkerhetsstrategi. 

Contoso Shipping vet att kryptering är det enda skydd deras data har när de lämnar datacentret och skickas till de där mobilapparna.

## <a name="what-is-encryption"></a>Vad är kryptering?

Kryptering är en process där data görs oläsliga och oanvändbara. För att krypterade data ska kunna användas eller läsas måste de *dekrypteras*, vilket kräver användning av en hemlig nyckel. Det finns två överordnade typer av kryptering: **symmetrisk** och **asymmetrisk**.

Vid symmetrisk kryptering används samma nyckel för att kryptera och dekryptera data. Ett exempel kan vara ett lösenordshanteringsprogram. Du anger dina lösenord och de krypteras med din egen personliga nyckel (din nyckel härleds ofta från ditt huvudlösenord). När data behöver hämtas används samma nyckel och informationen dekrypteras.

Vid asymmetrisk kryptering används ett par med en offentlig nyckel och en privat nyckel. Båda nycklarna kan kryptera, men inte dekryptera, sina egna krypterade data. För att dekryptera krävs den parade nyckeln. Asymmetrisk kryptering används för sådant som TLS (används i https) och datasignering.

Både symmetrisk och asymmetrisk kryptering är viktigt för att skydda data på rätt sätt. 

Inom kryptering skiljer man på kryptering i vila och kryptering under överföring.

## <a name="encryption-in-transit"></a>Kryptering under överföring

Data under överföring är de data som aktivt flyttar från en plats till en annan, till exempel via internet eller via ett privat nätverk. Säker överföring kan hanteras av flera olika lager. Det kan göras genom att data krypteras på programnivå innan de skickas över ett nätverk. HTTPS är ett exempel på överföringskryptering av programlager. 

Du kan också ställa in en säker kanal, t.ex. en VPN-anslutning i ett nätverkslager, för att överföra data mellan två system. 

Kryptering av data under överföring skyddar data från utomstående observatörer och tillhandahåller en mekanism för överföring av data samtidigt som risken för exponering minskar. 

<!--TODO: replace with final media which was submitted for Design-for-security-in-azure -->
![Kryptering under överföring](../media-COPIED-FROM-DESIGNFORSECURITY/encryption-in-transit.png)


## <a name="encryption-at-rest"></a>Kryptering i vila

Vilande data är data som har lagrats på ett fysiskt medium. Detta kan vara data som lagras på en serverdisk, data som lagras i en databas eller data som lagras i ett lagringskonto. Oavsett lagringsmekanism garanterar krypteringen av vilande data att dessa lagrade data inte kan läsas utan de nycklar och hemligheter som krävs för att dekryptera dem. Om en angripare får åtkomst till en hårddisk med krypterade data, men inte har tillgång till krypteringsnycklarna, är det extremt svårt för angriparen att komma åt dessa data.

Vilka data som krypteras, vad de används för och hur viktiga de är för organisationen kan variera. Det kan röra sig om finansiell information som är viktig för företaget, immateriell egendom som har utvecklats av företaget, personliga data som företaget lagrar om kunder eller anställda eller de nycklar och hemligheter som används för själva datakrypteringen.

<!--TODO: replace with final media which was submitted for Design-for-security-in-azure -->
![Kryptering i vila](../media-COPIED-FROM-DESIGNFORSECURITY/encryption-at-rest.png)

## <a name="encryption-on-azure"></a>Kryptering i Azure

Nu ska vi se hur du kan kryptera data i olika tjänster i Azure.

### <a name="encrypt-raw-storage"></a>Kryptera lagring av rådata

Med Kryptering för lagringstjänst (SSE) för vilande data kan du skydda dina data i enlighet med säkerhets- och efterlevnadskraven i din organisation. Med den här funktionen krypterar Azures lagringsplattform automatiskt dina data innan de lagras i Azure Managed Disks, Azure Blob Storage, Azure Files eller Azure Queue Storage, och dekrypterar dem innan de hämtas. Hanteringen av kryptering, kryptering i vila, dekryptering och nyckelhantering i Kryptering för lagringstjänst är transparent för program som använder tjänsterna.

### <a name="encrypt-virtual-machines"></a>Kryptera virtuella datorer

Kryptering av lagringstjänst tillhandahåller krypteringsskydd på låg nivå för data som skrivs till en fysisk disk, men hur skyddar du de virtuella hårddiskarna på virtuella datorer? Vad händer om en angripare får åtkomst till din Azure-prenumeration och extraherar de virtuella hårddiskarna från dina virtuella datorer? Hur kan du vara säker på att angriparen inte kan komma åt data som lagras på de virtuella hårddiskarna?

Azure Disk Encryption (ADE) är en funktion som hjälper dig att kryptera dina IaaS-baserade virtuella Windows- och Linux-diskar. ADE använder branschstandardfunktionen BitLocker i Windows och DM-Crypt i Linux för att tillhandahålla volymkryptering för operativsystemet och datadiskarna. Lösningen är integrerad med Azure Key Vault och hjälper dig att kontrollera och hantera diskkrypteringsnycklarna och hemligheterna (och du kan använda hanterade tjänstidentiteter för att komma åt nyckelvalvet).

Att använda virtuella datorer var ett av de första stegen mot molnet för Contoso Shipping. Att kryptera alla de virtuella hårddiskarna är ett mycket enkelt sätt med låg verksamhetspåverkan för att säkerställa att de gör allt de kan för att skydda sina data.

### <a name="encrypt-databases"></a>Kryptera databaser

Med transparent datakryptering (TDE) kan du skydda Azure SQL Database och Azure Data Warehouse mot skadlig aktivitet. TDE utför realtidskryptering och realtidsdekryptering av databasen, tillhörande säkerhetskopior och transaktionsloggfiler i vila, utan att några ändringar krävs i programmet. Som standard är TDE aktiverat för alla nyligen distribuerade Azure SQL-databaser.

TDE krypterar lagringen av en hel databas med hjälp av en symmetrisk nyckel kallad databaskrypteringsnyckeln. Som standard tillhandahåller Azure en unik krypteringsnyckel per logisk SQL-server och hanterar all information. BYOK (Bring Your Own Key) stöds även med nycklar som lagras i Azure Key Vault.

Eftersom transparent datakryptering är aktiverad som standard kan Contoso vara säkra på att de har rätt skydd för data som lagras i deras databaser.

### <a name="encrypt-secrets"></a>Kryptera hemligheter

Vi har sett att alla krypteringstjänster använder nycklar för att kryptera och dekryptera data. Men hur ser vi till att själva nycklarna är säkra? Företag kanske också har lösenord, anslutningssträngar och andra känsliga uppgifter som måste lagras på ett säkert sätt.

Azure Key Vault är en molntjänst som fungerar som ett säkert lager för hemligheter. Du kan skapa flera säkra containrar, kallade valv, i Key Vault. De här valven förlitar sig på säkerhetsmoduler på maskinvarunivå (HSM:er). Med valv så minskar risken för att säkerhetsinformation förloras av misstag eftersom lagringen av hemligheter centraliseras. Key Vault kontrollerar och loggar dessutom åtkomsten till allt som lagras i valven. Azure Key Vault kan hantera förfrågningar om och förnyelser av TLS-certifikat (Transport Layer Security), och ger tillgång till alla de funktioner som krävs för en robust livscykelhantering av certifikat. Key Vault har stöd för alla typer av hemligheter. Exempel på sådana hemligheter är lösenord, databasautentiseringsuppgifter, API-nycklar och certifikat.

Eftersom Azure AD-identiteter kan beviljas behörighet att använda Azure Key Vault-hemligheter, kan MSI-aktiverade program automatiskt och smidigt hämta de hemligheter som de behöver.

Kryptering är ofta den sista skyddsbarriären mot angripare och en viktig del i en skiktbaserad strategi för att skydda dina system. Azure tillhandahåller inbyggda funktioner och tjänster som krypterar data och skyddar data mot oavsiktlig exponering. Skydd av kunddata som lagras i Azure-tjänster har högsta prioritet för Microsoft och ska integreras i all design. Grundläggande tjänster som Azure Storage, Azure Virtual Machines, Azure SQL Database och Azure Key Vault hjälper dig att skydda din miljö genom kryptering.
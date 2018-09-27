För de flesta organisationer är data den mest värdefulla och oersättliga tillgången. Kryptering fungerar som den sista och starkaste försvarslinjen i en skiktad säkerhetsstrategi. 

Contoso Shipping vet att kryptering är det enda skydd deras data har när de lämnar datacentret och skickas till de där mobilapparna.

## <a name="what-is-encryption"></a>Vad är kryptering?

Kryptering är en process där data görs oläsliga och oanvändbara för oauktoriserade personer. För att krypterade data ska kunna användas eller läsas måste de *dekrypteras*, vilket kräver användning av en hemlig nyckel. Det finns två överordnade typer av kryptering: **symmetrisk** och **asymmetrisk**.

Vid symmetrisk kryptering används samma nyckel för att kryptera och dekryptera data. Ett exempel kan vara ett lösenordshanteringsprogram. Du anger dina lösenord och de krypteras med din egen personliga nyckel (din nyckel härleds ofta från ditt huvudlösenord). När data behöver hämtas används samma nyckel och informationen dekrypteras.

Vid asymmetrisk kryptering används ett par med en offentlig nyckel och en privat nyckel. Endera nyckel kan kryptera men en nyckel kan inte dekryptera sina egna krypterade data. För att dekryptera krävs den parkopplade nyckeln. Asymmetrisk kryptering används för saker som TLS (Transport Layer Security) (används i HTTPS) och datasignering.

Både symmetrisk och asymmetrisk kryptering är viktigt för att skydda data på rätt sätt. 

Inom kryptering skiljer man på kryptering i vila och kryptering under överföring.

## <a name="encryption-in-transit"></a>Kryptering under överföring

Data under överföring är de data som aktivt flyttar från en plats till en annan, till exempel via internet eller via ett privat nätverk. Säker överföring kan hanteras av flera olika lager. Det kan göras genom att data krypteras på programnivå innan de skickas över ett nätverk. HTTPS är ett exempel på överföringskryptering av programlager. 

Du kan också ställa in en säker kanal som ett virtuellt privat nätverk (VPN) i ett nätverkslager för att överföra data mellan två system. 

Kryptering av data under överföring skyddar data från utomstående observatörer och tillhandahåller en mekanism för överföring av data samtidigt som risken för exponering minskar. 

![Kryptering under överföring](../media/encryption-in-transit.png)


## <a name="encryption-at-rest"></a>Kryptering i vila

Vilande data är data som har lagrats på ett fysiskt medium. Detta kan vara data som lagras på en serverdisk, data som lagras i en databas eller data som lagras i ett lagringskonto. Oavsett lagringsmekanism garanterar krypteringen av vilande data att dessa lagrade data inte kan läsas utan de nycklar och hemligheter som krävs för att dekryptera dem. Om en angripare får åtkomst till en hårddisk med krypterade data, men inte har tillgång till krypteringsnycklarna, är det extremt svårt för angriparen att komma åt dessa data.

Vilka data som krypteras, vad de används för och hur viktiga de är för organisationen kan variera. Det kan röra sig om finansiell information som är viktig för företaget, immateriell egendom som har utvecklats av företaget, personliga data som företaget lagrar om kunder eller anställda eller de nycklar och hemligheter som används för själva datakrypteringen.

![Kryptering i vila](../media/encryption-at-rest.png)

## <a name="encryption-on-azure"></a>Kryptering i Azure

Nu ska vi se hur du kan kryptera data i olika tjänster i Azure.

:::row:::
  :::column:::
    ![Bild som representerar krypterad lagring](../media/4-encrypt-raw-storage.png)
  :::column-end:::
    :::column span="3"::: **Kryptera lagring av rådata**

Med Kryptering för lagringstjänst (SSE) för vilande data kan du skydda dina data i enlighet med säkerhets- och efterlevnadskraven i din organisation. Med den här funktionen krypterar Azures lagringsplattform automatiskt dina data innan de lagras i Azure Managed Disks, Azure Blob Storage, Azure Files eller Azure Queue Storage, och dekrypterar dem innan de hämtas. Hanteringen av kryptering, kryptering i vila, dekryptering och nyckelhantering i Kryptering för lagringstjänst är transparent för program som använder tjänsterna.
  :::column-end:::
:::row-end:::

:::row:::
  :::column:::
    ![Bild som representerar en krypterad virtuell dator](../media/4-encrypt-virtual-machines.png)
  :::column-end:::
    :::column span="3"::: **Kryptera virtuella datorer**

Kryptering av lagringstjänsten ger krypteringsskydd på låg nivå för data som skrivs till en fysisk disk, men hur skyddar du de virtuella hårddiskarna på virtuella datorer? Vad händer om en angripare får åtkomst till din Azure-prenumeration och extraherar de virtuella hårddiskarna från dina virtuella datorer? Hur kan du vara säker på att angriparen inte kan komma åt data som lagras på de virtuella hårddiskarna?

Azure Disk Encryption är en funktion som hjälper dig att kryptera dina IaaS-baserade virtuella Windows- och Linux-diskar. Azure Disk Encryption använder branschstandardfunktionen BitLocker i Windows och DM-Crypt i Linux för att tillhandahålla volymkryptering för operativsystemet och datadiskarna. Lösningen är integrerad med Azure Key Vault och hjälper dig att kontrollera och hantera diskkrypteringsnycklarna och hemligheterna (och du kan använda hanterade tjänstidentiteter för att komma åt nyckelvalvet).

Att använda virtuella datorer var ett av de första stegen mot molnet för Contoso Shipping. Att kryptera alla de virtuella hårddiskarna är ett mycket enkelt sätt med låg verksamhetspåverkan för att säkerställa att du gör allt du kan för att skydda företagets data.
  :::column-end:::
:::row-end:::

:::row:::
  :::column:::
    ![Bild som representerar en krypterad databas](../media/4-encrypt-databases.png)
  :::column-end:::
    :::column span="3"::: **Kryptera databaser**

Med transparent datakryptering kan du skydda Azure SQL Database och Azure Data Warehouse mot skadlig aktivitet. TDE utför realtidskryptering och realtidsdekryptering av databasen, tillhörande säkerhetskopior och transaktionsloggfiler i vila, utan att några ändringar krävs i programmet. Som standard är TDE aktiverat för alla nyligen distribuerade Azure SQL Database-instanser.

TDE krypterar lagringen av en hel databas med hjälp av en symmetrisk nyckel kallad databaskrypteringsnyckeln. Som standard tillhandahåller Azure en unik krypteringsnyckel per logisk SQL Server-instans och hanterar all information. BYOK (Bring Your Own Key) stöds även med nycklar som lagras i Azure Key Vault.

Eftersom transparent datakryptering är aktiverad som standard kan du vara säker på att Contoso Shipping har rätt skydd för data som lagras i deras databaser.
  :::column-end:::
:::row-end:::

:::row:::
  :::column:::
    ![Bild som representerar en krypterad hemlighet](../media/4-encrypt-secrets.png)
  :::column-end:::
    :::column span="3"::: **Kryptera hemligheter**

Vi har sett att alla krypteringstjänster använder nycklar för att kryptera och dekryptera data. Men hur ser vi till att själva nycklarna är säkra? Företag kanske också har lösenord, anslutningssträngar och andra känsliga uppgifter som måste lagras på ett säkert sätt.

Azure Key Vault är en molntjänst som fungerar som ett säkert lager för hemligheter. Du kan skapa flera säkra containrar, kallade valv, i Key Vault. De här valven förlitar sig på säkerhetsmoduler på maskinvarunivå (HSM:er). Med valv så minskar risken för att säkerhetsinformation förloras av misstag eftersom lagringen av hemligheter centraliseras. Nyckelvalv kontrollerar och loggar dessutom åtkomsten till allt som lagras i dem. Azure Key Vault kan hantera begäranden om och förnyelser av TLS-certifikat (Transport Layer Security) och ger tillgång till de funktioner som krävs för en robust livscykelhantering av certifikat. Key Vault har stöd för alla typer av hemligheter. Exempel på sådana hemligheter är lösenord, databasautentiseringsuppgifter, API-nycklar och certifikat.

Eftersom Azure AD-identiteter kan beviljas behörighet att använda Azure Key Vault-hemligheter, kan MSI-aktiverade program automatiskt och smidigt hämta de hemligheter som de behöver.
  :::column-end:::
:::row-end:::

## <a name="summary"></a>Sammanfattning

Som du säkert vet så är kryptering ofta den sista skyddsbarriären mot angripare och en viktig del i en skiktbaserad strategi för att skydda dina system. Azure tillhandahåller inbyggda funktioner och tjänster som krypterar och skyddar data mot oavsiktlig exponering. Skydd av kunddata som lagras i Azure-tjänster har högsta prioritet för Microsoft och ska integreras i all design. Grundläggande tjänster som Azure Storage, Azure Virtual Machines, Azure SQL Database och Azure Key Vault hjälper dig att skydda din miljö genom kryptering.
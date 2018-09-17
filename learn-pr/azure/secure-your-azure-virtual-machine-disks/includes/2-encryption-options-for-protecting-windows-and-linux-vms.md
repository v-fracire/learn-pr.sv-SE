Anta att ditt företags affärspartner har säkerhetsprinciper som kräver att deras affärsdata skyddas med stark kryptering. Du använder ett B2B-program som körs på Windows-servrar och lagrar data på en serverdatadisk. Nu när du övergår till molnet behöver du visa dina affärspartners att de data som lagras på dina virtuella Azure-datorer inte kan nås av obehöriga användare, enheter eller program. Du måste välja en strategi för kryptering av dina B2B-data.

Dina granskningskrav kräver att dina krypteringsnycklar måste hanteras lokalt och inte av tredje part. Det är också viktigt för dig att dina Azure-baserade servrar inte tappar i prestanda eller hanterbarhet. Så innan du implementerar kryptering vill du vara säker på att det inte påverkar din prestanda.

## <a name="what-is-encryption"></a>Vad är kryptering?

Kryptering handlar om att omvandla meningsfull information till något som verkar meningslöst, till exempel en slumpmässig sekvens med bokstäver och siffror. I krypteringsprocessen används någon form av **nyckel** som ett led i algoritmen som skapar krypterade data. En nyckel krävs också för att utföra dekrypteringen. Nycklarna kan vara **_symmetriska_**, så att samma nyckel används för kryptering och dekryptering, eller **_asymmetriska_**, så att olika nycklar används. Ett exempel på det senare är de **offentliga/privata** nyckelpar som används för digitala certifikat.

### <a name="symmetric-encryption"></a>Symmetrisk kryptering

Algoritmer som använder symmetriska nycklar, till exempel Standard AES (Advanced Encryption), är normalt snabbare än algoritmer för offentliga nycklar och används ofta för att skydda stora datamängder. Eftersom det finns bara en måste det finnas procedurer för att skydda nyckeln från att komma ut till allmänheten.

### <a name="asymmetric-encryption"></a>Asymmetrisk kryptering

Med asymmetriska algoritmer behöver endast den privata nyckeln i paret vara säker och privat, vilket namnet antyder. Den offentliga nyckeln kan ges till vem som helst utan att riskera krypterade data. Nackdelen med algoritmer med offentlig nyckel är dock att de är mycket långsammare än symmetriska algoritmer och kan inte användas till att kryptera stora datamängder.

## <a name="key-management"></a>Nyckelhantering

Krypteringsnycklarna i Azure kan antingen hanteras av Microsoft eller av kunden. Ofta efterfrågas kundhanterade nycklar av organisationer som behöver kunna redovisa att de efterlever HIPAA och andra bestämmelser. Detta kan kräva att åtkomsten till nycklarna loggas och att regelbundna nyckeländringar sker och registreras.

## <a name="azure-disk-encryption-technologies"></a>Diskkrypteringstekniker för Azure

De viktigaste krypteringsbaserade diskskyddsteknikerna för virtuella Azure-datorer är:

- Kryptering för lagringstjänster (SSE)
- Azure Disk Encryption (ADE)

### <a name="storage-service-encryption"></a>Kryptering för lagringstjänst

Azure Storage Service Encryption (SSE) är en krypteringstjänst som är inbyggd i Azure och som används för att skydda vilande data. Azures lagringsplattform krypterar data automatiskt innan de lagras hos flera lagringstjänster, inklusive Azure Managed Disks. 256-bitars AES-kryptering är aktiverad som standard, och hanteras av en administratör för lagringskontot.

### <a name="azure-disk-encryption"></a>Azure Disk Encryption

Azure Disk Encryption (ADE) hanteras av den virtuella datorns ägare. ADE styr kryptering av VM-kontrollerade diskar med Windows och Linux med hjälp av **BitLocker** på virtuella Windows-datorer och **DM-Crypt** på virtuella Linux-datorer. BitLocker-diskkryptering är en funktion för dataskydd som är integrerad med operativsystemet och hanterar hot om datastöld eller säkerhetsblottor som uppstår till följd av förlorade, stulna eller felaktigt inaktiverade datorer. På liknande sätt krypterar DM-Crypt vilande data för Linux innan de skrivs till lagring.

ADE säkerställer att alla data på virtuella datordiskar krypteras i vila i Azure-lagringen, och ADE krävs för virtuella datorer som säkerhetskopieras till Recovery-valvet.

När du använder ADE startar virtuella datorer under kundkontrollerade nycklar och principer. ADE är integrerat med Azure Key Vault för hantering av dessa diskkrypteringsnycklar och hemligheter.

> [!NOTE] 
> ADE saknar stöd för kryptering av virtuella datorer på Basic-nivå, och du kan inte använda en lokal nyckelhanteringstjänst (KMS) med ADE.

## <a name="when-to-use-encryption"></a>När du ska använda kryptering?

Data utsätts för risk i rörelse (vid överföring över internet eller ett annat nätverk) och när de är i vila (sparade på en lagringsenhet). Data i vila är det viktigaste när du skyddar data på diskar för virtuella Azure-datorer. Till exempel, någon kan ladda ner den virtuella hårddiskfilen (VHD) som associeras med en virtuell Azure-dator och spara den på sin dator. Innehållet i den virtuella hårddisken är potentiellt tillgänglig för alla som kan montera VHD-filen på sin dator om den inte är krypterad.

PÅ operativsystemdiskar (OS) krypteras data, till exempel lösenord automatiskt, så att det inte går att få tillgång till sådan information, även om den virtuella hårddisken inte krypteras. Program kan också automatiskt kryptera sina egna data. Men även med detta skydd, om någon med skadliga avsikter får åtkomst till en datadisk och disken inte är krypterad har de möjlighet att exploatera kända svagheter i dessa programs dataskydd. Denna typ av kryphål kan inte uppstå om du har diskkryptering på plats.

Kryptering för lagringstjänst (SSE) är en del av Azure, och bör inte ha någon märkbar inverkan på prestanda för läs- och skrivåtgärder för den virtuella datorns disk. Hanterade diskar med SSE nu är standard och det bör inte finnas någon anledning att ändra den standardinställningen. Azure Disk Encryption (ADE) använder operativsystemsverktyg för virtuella datorer (BitLocker och DM-Crypt), vilket innebär att den virtuella datorn måste delta när en virtuell disk ska krypteras eller dekrypteras. Den extra processorverksamheten hos den virtuella datorn som ingår i detta är normalt försumbar, med undantag för vissa situationer. Om du till exempel kör ett processorintensivt program kan det finnas anledningar att låta operativsystemdisken förbli okrypterad för att maximera prestanda. I sådana situationer kan du lagra programdata på en separat krypterad datadisk, så att du får de prestanda du behöver utan att äventyra säkerheten.

Azure tillhandahåller två kompletterande krypteringstekniker som används för att skydda Virtuella Azure-diskar. Dessa tekniker, SSE och ADE, krypterar olika lager och har olika syften. Båda använder AES-256-bitarskryptering. Båda tekniker ger ett djupgående skydd mot obehörig åtkomst till din Azure-lagring och specifika virtuella hårddiskar.

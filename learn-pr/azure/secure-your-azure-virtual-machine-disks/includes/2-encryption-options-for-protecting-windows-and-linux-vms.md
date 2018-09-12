Anta att ditt företags affärspartner har säkerhetsprinciper som kräver att deras affärsdata skyddas med stark kryptering. Du använder ett B2B-program som körs på Windows-servrar och lagrar data på en serverdatadisk. Nu när du övergår till molnet behöver du visa dina affärspartners att de data som lagras på dina virtuella Azure-datorer inte kan nås av obehöriga användare, enheter eller program. Du måste välja en strategi för att implementera kryptering av B2B-data.

Dina granskningskrav kräver att dina krypteringsnycklar måste hanteras lokalt och inte av tredje part. Det är också viktigt för dig att dina Azure-baserade servrar inte förlorar sin prestanda eller hanterbarhet. Så innan du implementerar kryptering vill du vara säker på att det inte påverkar din prestanda.

## <a name="what-is-encryption"></a>Vad är kryptering?

Kryptering handlar om hur du konverterar meningsfull information till något som verkar meningslöst, till exempel en slumpmässig sekvens med bokstäver och siffror. Krypteringsprocessen används någon form av **nyckel** som en del av algoritmen som skapar krypterade data och en nyckel behövs också för att utföra dekrypteringen. Nycklar kan vara **_symmetriska_**, där samma nyckel används för kryptering och dekryptering, eller **_asymmetriska_**, där olika nycklar används t.ex **offentliga/privata** nyckelpar som används i digitala certifikat.

### <a name="symmetric-encryption"></a>Symmetrisk kryptering

Algoritmer som använder symmetriska nycklar, till exempel Standard AES (Advanced Encryption), är normalt snabbare än algoritmer för offentliga nycklar och används ofta för att skydda stora datamängder. Eftersom det finns bara en måste det finnas procedurer för att skydda nyckeln från att komma ut till allmänheten.

### <a name="asymmetric-encryption"></a>Asymmetrisk kryptering

Med asymmetriska algoritmer behöver endast den privata nyckeln i paret vara säker och privat, vilket namnet antyder. Den offentliga nyckeln kan ges till vem som helst utan att riskera krypterade data. Nackdelen med algoritmer med offentlig nyckel är dock att de är mycket långsammare än symmetriska algoritmer och kan inte användas till att kryptera stora datamängder.

## <a name="key-management"></a>Nyckelhantering

Krypteringsnycklarna i Azure kan antingen hanteras av Microsoft eller av kunden. Behovet av kundhanterade nycklar kommer ofta från organisationer som behöver visa att de efterlever HIPAA och andra bestämmelser. Detta kan kräva att åtkomsten till nycklarna loggas och att regelbundna nyckeländringar sker och registreras.

## <a name="azure-disk-encryption-technologies"></a>Diskkrypteringstekniker för Azure

De viktigaste krypteringsbaserade diskskyddsteknikerna för virtuella Azure-datorer är:

- Kryptering för lagringstjänster (SSE)
- Azure Disk Encryption (ADE)

### <a name="storage-service-encryption"></a>Kryptering för lagringstjänster

Azure Storage Service Encryption (SSE) är en krypteringstjänst som är inbyggd i Azure och som användes till att överföra data i vila. Azure storage-plattformen krypterar automatiskt data innan de lagras till flera lagringstjänster, inklusive Azure Managed Disks. Kryptering är aktiverat som standard med 256-bitars AES-kryptering och hanteras av administratör för lagringskonto.

### <a name="azure-disk-encryption"></a>Azure Disk Encryption

Azure Disk Encryption (ADE) hanteras av VM-ägaren och styr kryptering av VM-kontrollerade diskar med Windows och Linux med hjälp av **BitLocker** på virtuella Windows-datorer och **DM-Crypt** på virtuella Linux-datorer. BitLocker-diskkryptering är en funktion för dataskydd som är integrerad med operativsystemet och hanterar hot om datastöld eller exponering från förlorade, stulna eller felaktigt inaktiverade datorer. På samma sätt krypterar DM-Crypt data i vila för Linux innan de skrivs till lagring.

ADE säkerställer att alla data på virtuella datordiskar är krypterade i vila i Azure storage och att ADE krävs för virtuella datorer som är säkerhetskopierade i Recovery-valvet.

När du använder ADE så startar virtuella datorer under kundkontrollerade nycklar och principer. ADE är integrerat med Azure Key Vault för hantering av dessa diskkrypteringsnycklar och hemligheter.

> [!NOTE] 
> ADE har inte stöd för kryptering av virtuella datorer på Basic-nivå och du kan inte använda en lokal nyckelhanteringstjänst (KMS) med ADE

## <a name="when-to-use-encryption"></a>När du ska använda kryptering?

Data utsätts för risk i rörelse (vid överföring över internet eller ett annat nätverk) och när de är i vila (sparade på en lagringsenhet). Data i vila är det viktigaste när du skyddar data på diskar för virtuella Azure-datorer. Till exempel, någon kan ladda ner den virtuella hårddiskfilen (VHD) som associeras med en virtuell Azure-dator och spara den på sin dator. Innehållet i den virtuella hårddisken är potentiellt tillgänglig för alla som kan montera VHD-filen på sin dator om den inte är krypterad.

PÅ operativsystemdiskar (OS) krypteras data, till exempel lösenord automatiskt, så att det inte går att få tillgång till sådan information, även om den virtuella hårddisken inte krypteras. Program kan också automatiskt kryptera sina egna data. Men även med detta skydd, om någon med skadliga avsikter får åtkomst till en datadisk och disken inte är krypterad har de möjlighet att exploatera kända svagheter i dessa programs dataskydd. Sådana kryphål är inte möjliga med diskkryptering på plats.

Storage Service Encryption (SSE) är en del av Azure, och det bör finnas någon märkbar effekt på prestandan hos en virtuell disks i/o när du använder SSE. Hanterade diskar med SSE nu är standard och det finns ingen anledning att ändra detta. Azure Disk Encryption (ADE) gör att användning av operativsystemsverktyg för virtuella datorer (BitLocker och DM-Crypt), så den virtuella datorn måste delta när kryptering eller dekryptering av en virtuell disk sker. Effekten på den extra processorverksamheten på den virtuella datorn är normalt försumbar, utom i vissa situationer. Till exempel, om du kör ett processorintensivt program kan det finnas anledningar att låta operativsystemdisken förbli okrypterad för maximal funktion. I en situation som denna kan du lagra programdata på en separat krypterad datadisk, så att du får rätt prestanda utan att äventyra säkerheten.

Azure tillhandahåller två kompletterande krypteringstekniker som används för att skydda Virtuella Azure-diskar. Dessa tekniker, SSE och ADE, krypterar olika lager och har olika syften, men båda använder AES-256-bitarskryptering. Med båda teknikerna får du ett djupgående skydd mot obehörig åtkomst till din Azure-lagring och specifika virtuella hårddiskar.

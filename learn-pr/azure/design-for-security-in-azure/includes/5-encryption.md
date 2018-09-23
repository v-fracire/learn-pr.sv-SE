Data är en organisations mest värdefulla och oersättliga tillgång och kryptering är den sista och starkaste skyddsbarriären i en skiktbaserad säkerhetsstrategi. Vårdleverantören Lamna Healthcare lagrar stora mängder känsliga data. De råkade nyligen ut för ett intrång där okrypterade känsliga patientdata exponerades och de är nu fullt medvetna om luckorna i företagets dataskyddsfunktioner. De vill veta hur de hade kunnat använda kryptering på ett bättre sätt för att skydda sig själva och sina patienter mot den här typen av incidenter. I det här avsnittet ska vi titta närmare på vad kryptering är, hur du hanterar kryptering av data och vilka krypteringsfunktioner som är tillgängliga i Azure.

## <a name="what-is-encryption"></a>Vad är kryptering?

Kryptering är en process där data görs oläsliga och oanvändbara. För att krypterade data ska kunna användas eller läsas måste de *dekrypteras*, vilket kräver användning av en hemlig nyckel. Det finns två överordnade typer av kryptering: **Symmetrisk** och **Asymmetrisk**.

Vid symmetrisk kryptering används samma nyckel för att kryptera och dekryptera data. Ett exempel kan vara ett lösenordshanteringsprogram. Du anger dina lösenord och de krypteras med din egen personliga nyckel (din nyckel härleds ofta från ditt huvudlösenord). När data behöver hämtas används samma nyckel och informationen dekrypteras.

Vid asymmetrisk kryptering används ett par med en offentlig nyckel och en privat nyckel. Båda nycklarna kan kryptera men inte dekryptera sina egna krypterade data. För att dekryptera krävs den parkopplade nyckeln. Asymmetrisk kryptering används bl.a. för TLS (används i https) och datasignering.

Både symmetrisk och asymmetrisk kryptering är viktigt för att skydda data på rätt sätt. 

Inom kryptering skiljer man på kryptering i vila och kryptering under överföring.

### <a name="encryption-at-rest"></a>Kryptering i vila

Vilande data är data som har lagrats på ett fysiskt medium. Detta kan vara data som lagras på en serverdisk, data som lagras i en databas eller data som lagras i ett lagringskonto. Oavsett lagringsmekanism garanterar krypteringen av vilande data att dessa lagrade data inte kan läsas utan de nycklar och hemligheter som krävs för att dekryptera dem. Om en angripare får åtkomst till en hårddisk med krypterade data, men inte har tillgång till krypteringsnycklarna, är det extremt svårt för angriparen att komma åt dessa data. I ett sådant scenario skulle angriparen vara tvungen att försöka utföra attacker mot krypterade data, vilket är mycket svårare och mer resurskrävande än att komma åt okrypterade data på en hårddisk.

Vilka data som krypteras, vad de används för och hur viktiga de är för organisationen kan variera. Det kan röra sig om finansiell information som är viktig för företaget, immateriell egendom som har utvecklats av företaget, personliga data som företaget lagrar om kunder eller anställda eller de nycklar och hemligheter som används för själva datakrypteringen.

![Ett bildexempel på kryptering i vila.](../media/encryption-at-rest.png)

### <a name="encryption-in-transit"></a>Kryptering under överföring

Data under överföring är de data som aktivt flyttar från en plats till en annan, till exempel via Internet eller via ett privat nätverk. Säker överföring kan hanteras genom att data krypteras innan de skickas via ett nätverk, eller genom att en säker kanal konfigureras för att skicka okrypterade data mellan två system. Kryptering av data under överföring skyddar data från utomstående observatörer och tillhandahåller en mekanism för överföring av data samtidigt som risken för exponering minskar. 

![Ett bildexempel på kryptering under överföring. Data krypteras innan de överförs. När data nått målet dekrypteras de.](../media/encryption-in-transit.png)

## <a name="identify-and-classify-data"></a>Identifiera och klassificera data

Nu ska vi gå tillbaka till det problem som Lamna Healthcare försöker lösa. De har tidigare haft problem med exponeringen av känsliga data, vilket indikerar att man inte krypterar de data som borde krypteras. De måste börja med att identifiera och klassificera vilka data som lagras och sedan anpassa lagringen efter de företagskrav och bestämmelser som reglerar lagringen av data. Det kan vara bra att klassificera dessa data i förhållande till hur en exponering påverkar organisationen, dess kunder eller partner. En exempelklassificering skulle kunna se ut så här:

|Dataklassificering|Förklaring|Exempel|
|---|---|---|
|Begränsade|Data som klassificeras som begränsade utgör en stor risk om de exponeras, ändras eller tas bort. Starka skyddsnivåer krävs för dessa data. |Data som innehåller personnummer, kreditkortsnummer, personliga hälsojournaler|
|Privata| Data som klassificeras som privata utgör en begränsad risk om de exponeras, ändras eller tas bort. Rimliga skyddsnivåer krävs för dessa data. Data som inte har klassificerats som begränsade eller offentliga kommer att klassificeras som privata.  |Personliga poster som innehåller information som adress, telefonnummer, akademiska resultat, kundinköpsposter|
|Offentliga| Data som klassificeras som offentliga utgör ingen risk om de exponeras, ändras eller tas bort. Inget skydd krävs för dessa data. |Offentliga ekonomiska rapporter, offentliga principer, produktdokumentation för kunder|

Genom att inventera de olika typer av data som lagras kan företaget få en bättre bild av var känsliga data lagras och var kryptering förekommer eller inte.

En grundlig förståelse av de företagskrav och bestämmelser som gäller för data som organisationen lagrar är också viktigt. De bestämmelser som en organisation måste följa föreskriver i hög utsträckning att data ska krypteras. Lamna Healthcare lagrar känsliga data som regleras av HIPAA (Health Insurance Portability and Accountability Act), som innehåller krav på hur patientdata ska hanteras och lagras. Andra branscher kan omfattas av andra myndighetskrav och bestämmelser. Ett finansinstitut kan lagra kontoinformation som regleras av PCI-standarder (Payment Card Industry). En organisation som gör affärer i EU kan omfattas av den allmänna dataskyddsförordningen (GDPR), som definierar hanteringen av personuppgifter inom EU. Särskilda affärsbehov kan också diktera att data som kan få ekonomiska konsekvenser för organisationen ska krypteras, t.ex. konkurrensinformation.

När du har klassificerat dina data och definierat dina krav kan du använda olika verktyg och tekniker för att implementera och kräva kryptering i din arkitektur.

## <a name="encryption-on-azure"></a>Kryptering i Azure

Nu ska vi se hur du kan kryptera data i olika tjänster i Azure.

### <a name="encrypting-raw-storage"></a>Kryptera lagring av rådata

Med Kryptering för lagringstjänst (SSE) för vilande data kan du skydda dina data i enlighet med säkerhets- och efterlevnadskraven i din organisation. Med den här funktionen krypterar Azures lagringsplattform automatiskt dina data innan de lagras i Azure Managed Disks, Azure Blob Storage, Azure Files eller Azure Queue Storage, och dekrypterar dem innan de hämtas. Hanteringen av kryptering, kryptering i vila, dekryptering och nyckelhantering i Kryptering för lagringstjänst är transparent för program som använder tjänsterna.

För Lamna Healthcare innebär det att när de använder tjänster som stöder Kryptering för lagringstjänst, så krypteras deras data på det fysiska lagringsmediet. Om någon obehörig mot förmodan får åtkomst till den fysiska disken är alla data oläsliga eftersom de krypterades när de skrevs till den fysiska disken.

### <a name="encrypting-virtual-machines"></a>Kryptera virtuella datorer

Kryptering av lagringstjänsten ger krypteringsskydd på låg nivå för data som skrivs till en fysisk disk, men hur skyddar du de virtuella hårddiskarna på virtuella datorer? Vad händer om en angripare får åtkomst till din Azure-prenumeration och extraherar de virtuella hårddiskarna från dina virtuella datorer? Hur kan du vara säker på att angriparen inte kan komma åt data som lagras på de virtuella hårddiskarna?

Azure Disk Encryption (ADE) är en funktion som hjälper dig att kryptera dina IaaS-baserade virtuella Windows- och Linux-diskar. ADE använder branschstandardfunktionen BitLocker i Windows och DM-Crypt i Linux för att tillhandahålla volymkryptering för operativsystemet och datadiskarna. Lösningen är integrerad med Azure Key Vault och hjälper dig att kontrollera och hantera diskkrypteringsnycklarna och hemligheterna (och du kan använda hanterad identitet för Azure-tjänster för att få åtkomst till nyckelvalvet).

 Lamna Healthcare kan använda ADE på sina virtuella datorer för att säkerställa att data som lagras på virtuella hårddiskar är skyddade och uppfyller företagets organisations- och efterlevnadskrav. Eftersom startdiskar också krypteras, kan de både styra och granska användningen.

### <a name="encrypting-databases"></a>Kryptera databaser

Lamna Healthcare har flera distribuerade databaser med data som kräver extra skydd. De har flyttat många databaser till Azure SQL Database och vill vara säkra på att deras data krypteras i databasen. Om datafiler, loggfiler och säkerhetskopior blir stulna måste de vara säkra på att de inte kan läsas utan åtkomst till krypteringsnycklarna.

Med transparent datakryptering kan du skydda Azure SQL Database och Azure Data Warehouse mot skadlig aktivitet. TDE utför realtidskryptering och realtidsdekryptering av databasen, tillhörande säkerhetskopior och transaktionsloggfiler i vila, utan att några ändringar krävs i programmet. Som standard är TDE aktiverat för alla nyligen distribuerade Azure SQL-databaser.

TDE krypterar lagringen av en hel databas med hjälp av en symmetrisk nyckel kallad databaskrypteringsnyckeln. Som standard tillhandahåller Azure en unik krypteringsnyckel per logisk SQL-server och hanterar all information. BYOK (Bring Your Own Key) stöds även med nycklar som lagras i Azure Key Vault.

Eftersom TDE är aktiverat som standard kan Lamna Healthcare vara säkra på att de har rätt skydd för data som lagras i deras databaser.

### <a name="encrypting-secrets"></a>Kryptera hemligheter

Vi har sett att alla krypteringstjänster använder nycklar för att kryptera och dekryptera data. Men hur ser vi till att själva nycklarna är säkra? Lamna Healthcare kanske också har lösenord, anslutningssträngar eller andra känsliga uppgifter som måste lagras på ett säkert sätt.

Azure Key Vault är en molntjänst som fungerar som ett säkert lager för hemligheter. Du kan skapa flera säkra containrar, kallade valv, i Key Vault. De här valven förlitar sig på säkerhetsmoduler på maskinvarunivå (HSM:er). Med valv så minskar risken för att säkerhetsinformation förloras av misstag eftersom lagringen av hemligheter centraliseras. Key Vault kontrollerar och loggar dessutom åtkomsten till allt som lagras i valven. Azure Key Vault kan hantera förfrågningar om och förnyelser av TLS-certifikat (Transport Layer Security), och ger tillgång till alla de funktioner som krävs för en robust livscykelhantering av certifikat. Key Vault har stöd för alla typer av hemligheter. Exempel på sådana hemligheter är lösenord, databasautentiseringsuppgifter, API-nycklar och certifikat.

Eftersom Azure AD-identiteter kan beviljas behörighet att använda Azure Key Vault-hemligheter, kan program med hanterade identiteter för Azure-tjänster automatiskt och smidigt hämta de hemligheter som de behöver.

Lamna Healthcare kan använda Key Vault för att lagra all känslig programinformation, inklusive TLS-certifikaten som de använder för att skydda kommunikationen mellan system.

## <a name="encryption-at-lamna-healthcare"></a>Kryptering på Lamna Healthcare

Lamna Healthcare har gått igenom identifierings- och klassificeringsprocessen för alla data som de lagrar. De har jämfört dessa klassificeringar med tillämpliga efterlevnads- och företagskrav och har insett att de måste kryptera betydligt större mängder data. De har krypterat alla virtuella datorer som lagrar känsliga data och de krypterar all känslig patientinformation som lagras i blobblagringen. Transparent datakryptering är aktiverat på alla deras databaser, så deras relationsdatabaser uppfyller krypteringskraven oavsett klassificering. De använder även Key Vault i hela organisationen för att lagra alla certifikat och autentiseringsuppgifter som olika program kan behöva för att fungera.

## <a name="summary"></a>Sammanfattning

Kryptering är ofta den sista skyddsbarriären mot angripare och är en viktig del i en skiktbaserad strategi för att skydda din arkitektur. Azure tillhandahåller inbyggda funktioner och tjänster som krypterar och skyddar data mot oavsiktlig exponering. Skydd av kunddata som lagras i Azure-tjänster har högsta prioritet för Microsoft och ska integreras i all arkitekturdesign. Grundläggande tjänster som Azure Storage, Azure Virtual Machines, Azure SQL Database och Azure Key Vault hjälper dig att skydda din miljö genom kryptering.

Att använda en molnplattform som värd för program medför flera fördelar jämfört med traditionella lokala distributioner. Molnets ”delat ansvar”-modell innebär att kontrollen över säkerheten på nivån för fysiska nätverk, utveckling och värdhantering flyttas till molnleverantören. Angreppsförsök mot plattformen på den här nivån lönar sig sällan med tanke på de stora investeringar som leverantörerna gör för att skydda och övervaka sina infrastrukturer.

Därför är det mycket effektivare för angripare att rikta in sig på säkerhetsbrister som introduceras av molnplattformens kunder på programnivå. Kunder som väljer att hantera sina program i PaaS (Platform as a Service) kan dessutom frigöra resurser som arbetar med operativsystemssäkerhet så att de i stället kan fokusera på programkoden och identitetssäkerheten i anslutning till programmet. I den här delen går vi igenom några exempel på hur programsäkerheten kan förbättras genom design.

## <a name="scenario"></a>Scenario

Lamna Healthcares kunder behöver åtkomst till personliga läkarjournaler via en onlinewebbportal. Företaget måste uppfylla kraven i HIPAA (Health Insurance Portability and Accountability Act). Läckage av personliga data på grund av intrång i företagets system kan leda till allvarliga ekonomiska påföljder. Att skydda programdata och personliga data har därför högsta prioritet för företaget.

De viktigaste områdena som rör kundtillämpningar är:

- Säker programdesign
- Datasäkerhet
- Identitets- och åtkomsthantering
- Slutpunktssäkerhet

## <a name="security-development-lifecycle"></a>Säkerhetsutvecklingslivscykel

Microsofts [SDL-process](https://www.microsoft.com/en-us/sdl) för säkerhetsutvecklingslivscykeln (Security Development Lifecycle) kan användas under designfasen för att säkerställa att viktiga säkerhetsfrågor hanteras under programvaruutvecklingens livscykel. Säkerhets- och efterlevnadsproblem är mycket enklare att hantera när du utformar ett program och kan minimera risken för många vanliga fel som kan leda till säkerhetsbrister i den slutliga produkten. Det är också mycket mindre kostsamt att åtgärda problem i ett tidigt skede under programvaruutvecklingen. Stegen i SDL-processen för ett programvaruprojekt utförs normalt i följande ordning:

1. Utbildning

    - Grundläggande säkerhetsutbildning

1. Krav

    - Definiera krav och kvalitet
    - Analysera säkerhets- och integritetsrisker
 
1. Design

    - Analysera attackyta
    - Hotmodellering
 
1. Implementering

    - Specificera verktyg för att säkerställa att kodkvaliteten kan mätas
    - Tillämpa regler för förbjudna API:er och funktioner
    - Utför analys av statisk kod
    - Sök efter lagrade hemligheter på lagringsplatser
 
1. Verifiering

    - Dynamisk testning/fuzztestning
    - Verifiera hotmodeller/attackyta
 
1. Utgivning

    - Definiera säkerhetsåtgärdsplan
    - Genomför en slutlig säkerhetsgranskning
    - Utgivningsarkiv
 
1. Svarsåtgärder 

    - Verkställ svarsåtgärder vid hot

![Säkerhetsutvecklingslivscykel (SDL)](../media/sdl.png)

SDL är både en kulturell aspekt och en process eller uppsättning verktyg. Att skapa en kultur där säkerheten har hög prioritet inom all programutveckling är ett viktigt steg i en organisations säkerhetsoptimering.

<!-- Bear in mind that the migration of un-modified applications (especially COTS procured software systems) will not be able to perform many of the steps listed above.
 -->

## <a name="operational-security-assessment"></a>Driftsäkerhetsutvärdering

När ett program har distribuerats är det viktigt att löpande utvärdera säkerheten i programmet, att avgöra hur problem som identifieras kan åtgärdas och undvikas och att integrera denna kunskap i programvaruutvecklingscykeln. I hur hög grad detta görs är kopplat till programvaruutvecklings- och driftteamens mognadsnivå och till kraven på dataintegritet.

Programvarutjänster som söker efter säkerhetsbrister kan användas för att automatisera den här processen och för att regelbundet utvärdera säkerhetsproblem så att kostsamma manuella processer, t.ex. penetrationstester, kan undvikas.

Azure Security Center är en kostnadsfri tjänst som nu är aktiverad som standard för alla Azure-prenumerationer. Tjänsten är nära integrerad med andra tjänster på Azure-programnivå som Azure Application Gateway och Azure Web Application Firewall. Genom att analysera loggar från dessa tjänster kan ASC rapportera om kända problem i realtid och rekommendera lämpliga åtgärder, samt även konfigureras att automatiskt köra strategiböcker som svar på attacker.

<!-- SDL culture
Key Vault / MSI
CSE = App  -> DB & App Storage
Mention approach of code scanning & SDL
Scanning for passwords - Git
 -->

## <a name="identity-as-the-perimeter"></a>Identitet som skyddsmekanism

Identitetsverifiering är i allt högre utsträckning den första skyddsbarriären för program. Att begränsa åtkomsten till ett webbprogram genom autentiserings- och auktoriseringssessioner kan drastiskt minska angreppsytan. Azure AD och Azure AD B2C erbjuder ett effektivt sätt att överlåta ansvaret för identitets- och åtkomsthanteringen till en helt hanterad tjänst. Azure AD-principer för villkorlig åtkomst, Privileged Identity Management och Identity Protection-kontroller förbättrar ytterligare en kunds möjlighet att förebygga obehörig åtkomst och att granska ändringar.

## <a name="data-protection"></a>Dataskydd

Kunddata är målet för de flesta, om inte alla, attacker mot webbprogram. Säker lagring och transport av data mellan ett program och datalagringsskiktet är helt avgörande.

Lamna Healthcare lagrar och använder mycket känslig medicinsk patientinformation. HIPAA antogs av USA:s kongress 1996 och definierar, jämte andra kontroller, nationella standarder för elektroniska transaktioner av vårdgivare och arbetsgivare inom hälsovården. Lamna måste se till att patienter och behöriga parter, t.ex. deras läkare, har säker åtkomst till medicinsk information.

För att uppfylla dessa hårda krav har Lamna Healthcare ändrat sina program så att all patientinformation krypteras, både i vila och under överföring. Exempelvis används TLS (Transport Layer Security) för att kryptera data som skickas mellan webbprogrammet och SQL-databaserna på serversidan. Data krypteras också i vila i SQL Server med hjälp av TDE (Transparent Data Encryption). Det betyder att, även om miljön komprometteras, så är informationen oanvändbar för alla som saknar rätt krypteringsnycklar.

För kryptering av data som lagras i bloblagring kan kryptering på klientsidan användas så att data som finns i minnet krypterar innan de skrivs till lagringstjänsten. Bibliotek som stöder den här typen av kryptering är tillgängliga för .NET, Java och Python och gör det möjligt att integrera datakryptering direkt i program för att förbättra dataintegriteten.

### <a name="secure-key-and-secret-storage"></a>Säker lagring av nycklar och hemligheter

Det är viktigt att programhemligheter (anslutningssträngar, lösenord osv.) och krypteringsnycklar hålls åtskilda från programmet som används för att komma åt data. Krypteringsnycklar och programhemligheter bör aldrig lagras i programkoden för konfigurationsfiler. I stället bör en säker lagringsplats som Azure Key Vault användas. Åtkomst till dessa känsliga data kan sedan begränsas till programidentiteter via hanterade tjänstidentiteter, och nycklarna kan roteras med jämna mellanrum för att begränsa exponeringen och skydda mot läckage av krypteringsnycklar. Kunder kan också välja att använda sina egna krypteringsnycklar som genererats av lokala maskinvarusäkerhetsmoduler (HSM) och kan även begära att Azure Key Vault-instanser implementeras i diskreta HSM-moduler för en enskild klientorganisation.

<!-- ### Secure and immutable file storage

All Azure storage accounts are encrypted by default using Microsoft managed keys. Azure customers also have the ability to use their own encryption keys (BYOK) to encrypt blob, file and queue data so that even the hosting provider has no access to unencrypted data. Data immutability is often required for auditing purposes or when legal disputes call for data to be effectively frozen for a determined amount of time. Azure has recently introduced an [immutable data storage](https://docs.microsoft.com/en-us/azure/storage/blobs/storage-blob-immutable-storage) option known as Write-Once, Read many (WORM) for this scenario. -->

## <a name="application-security-at-lamna-healthcare"></a>Programsäkerhet på Lamna Healthcare

## <a name="summary"></a>Sammanfattning
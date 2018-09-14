Att använda en molnplattform som värd för program medför flera fördelar jämfört med traditionella lokala distributioner. Delat ansvar molnmodell flyttar säkerhet på fysiska nätverk, bygga och värden nivåer kontrolleras av molnleverantören. Angreppsförsök mot plattformen på den här nivån lönar sig sällan med tanke på de stora investeringar som leverantörerna gör för att skydda och övervaka sina infrastrukturer.

Därför är det mycket effektivare för angripare att rikta in sig på säkerhetsbrister som introduceras av molnplattformens kunder på programnivå. Kunder som väljer att hantera sina program i PaaS (Platform as a Service) kan dessutom frigöra resurser som arbetar med operativsystemssäkerhet så att de i stället kan fokusera på programkoden och identitetssäkerheten i anslutning till programmet. I den här delen går vi igenom några exempel på hur programsäkerheten kan förbättras genom design.

## <a name="scenario"></a>Scenario

Lamna Healthcares kunder behöver åtkomst till personliga läkarjournaler via en onlinewebbportal. Efterlever den Health Insurance Portability and Accountability Act (HIPAA) är obligatoriskt och placerar företaget betydande risk för ekonomiska påföljder om ett intrång av personuppgifter inträffar; Därför är att skydda program och personliga data som det interagerar med avgörande.

De viktigaste områdena som rör kundtillämpningar är:

- Säker programdesign
- Datasäkerhet
- Identitets- och åtkomsthantering
- Slutpunktssäkerhet

## <a name="security-development-lifecycle"></a>Säkerhetsutvecklingslivscykel

Microsofts [SDL-process](https://www.microsoft.com/sdl) för säkerhetsutvecklingslivscykeln (Security Development Lifecycle) kan användas under designfasen för att säkerställa att viktiga säkerhetsfrågor hanteras under programvaruutvecklingens livscykel. Säkerhets- och efterlevnadsproblem är mycket enklare att hantera när du utformar ett program och kan minimera risken för många vanliga fel som kan leda till säkerhetsbrister i den slutliga produkten. Det är också mycket mindre kostsamt att åtgärda problem i ett tidigt skede under programvaruutvecklingen. Stegen i SDL-processen för ett programvaruprojekt utförs normalt i följande ordning:

1. Utbildning

    - Grundläggande säkerhetsutbildning

1. Krav

    - Definiera krav och kvalitet
    - Analysera risker för säkerhet och sekretess
 
1. Design

    - Analysera attackyta
    - Hotmodellering
 
1. Implementering

    - Specificera verktyg för att säkerställa att kodkvaliteten kan mätas
    - Framtvinga förbjudna API: er och funktioner
    - Utföra statiska kodanalys
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

SDL är både en kulturell aspekt och en process eller uppsättning verktyg. Skapa en kultur där säkerhet är ett primärt fokus och krav för alla application development kan göra stort steg framåt i utvecklade en organisations funktionerna kring säkerhet.

<!-- Bear in mind that the migration of un-modified applications (especially COTS procured software systems) will not be able to perform many of the steps listed above.
 -->

## <a name="operational-security-assessment"></a>Driftsäkerhetsutvärdering

När ett program har distribuerats, är det viktigt att utvärdera sin säkerhetsposition, bestämma hur du ska åtgärda eventuella problem som identifieras, och feed kunskaper tillbaka till utvecklingscykeln programvara kontinuerligt. I hur hög grad detta görs är kopplat till programvaruutvecklings- och driftteamens mognadsnivå och till kraven på dataintegritet.

Programvarutjänster som söker efter säkerhetsbrister kan användas för att automatisera den här processen och för att regelbundet utvärdera säkerhetsproblem så att kostsamma manuella processer, t.ex. penetrationstester, kan undvikas.

Azure Security Center är en kostnadsfri tjänst som nu är aktiverad som standard för alla Azure-prenumerationer. Tjänsten är nära integrerad med andra tjänster på Azure-programnivå som Azure Application Gateway och Azure Web Application Firewall. Genom att analysera loggar från de här tjänsterna kan ASC kan rapportera om kända problem i realtid, rekommenderar svar att lösa dem och även konfigureras för att automatiskt köra strategiböcker som svar på attacker.

<!-- SDL culture
Key Vault / MSI
CSE = App  -> DB & App Storage
Mention approach of code scanning & SDL
Scanning for passwords - Git
 -->

## <a name="identity-as-the-perimeter"></a>Identitet som skyddsmekanism

Identitetsverifiering är i allt högre utsträckning den första skyddsbarriären för program. Att begränsa åtkomsten till ett webbprogram genom autentiserings- och auktoriseringssessioner kan drastiskt minska angreppsytan. Azure AD och Azure AD B2C erbjuder ett effektivt sätt att överlåta ansvaret för identitets- och åtkomsthanteringen till en helt hanterad tjänst. Principer för villkorlig åtkomst för Azure AD privileged identity management och identitetsskydd kontroller ytterligare förbättra en kund möjlighet att förebygga obehörig åtkomst och granska ändringar.

## <a name="data-protection"></a>Dataskydd

Kunddata är målet för de flesta, om inte alla, attacker mot webbprogram. Säker lagring och transport av data mellan ett program och datalagringsskiktet är helt avgörande.

Lamna Healthcare lagrar och använder mycket känslig medicinsk patientinformation. HIPAA antogs av USA:s kongress 1996 och definierar, jämte andra kontroller, nationella standarder för elektroniska transaktioner av vårdgivare och arbetsgivare inom hälsovården. Lamna måste patienter och auktoriserade parterna, till exempel deras läkare ha säker åtkomst till medicinska data.

Lamna Healthcare har ändrat sina program för att kryptera alla patientens data i vila och under överföring för att uppfylla dessa krav. Exempelvis används TLS (Transport Layer Security) för att kryptera data som skickas mellan webbprogrammet och SQL-databaserna på serversidan. Data krypteras också i vila i SQL Server använder Transparent datakryptering (TDE), som säkerställer att även om miljön komprometteras data effektivt oanvändbara för alla som saknar rätt krypteringsnycklar.

För att kryptera data som lagras i blob storage kan client side encryption användas för att kryptera data i minnet innan de skrivs till storage-tjänsten. Bibliotek som stöder den här typen av kryptering är tillgängliga för .NET, Java och Python och gör det möjligt att integrera datakryptering direkt i program för att förbättra dataintegriteten.

### <a name="secure-key-and-secret-storage"></a>Säker lagring av nycklar och hemligheter

Det är viktigt att programhemligheter (anslutningssträngar, lösenord osv.) och krypteringsnycklar hålls åtskilda från programmet som används för att komma åt data. Krypteringsnycklar och programhemligheter bör aldrig lagras i programkoden för konfigurationsfiler. I stället bör en säker lagringsplats som Azure Key Vault användas. Åtkomst till den här känsliga data kan sedan begränsas till programmet identiteter via hanterade tjänstidentiteter och nycklar kan roteras med jämna mellanrum att begränsa exponering när det gäller kryptering nyckeln läcker ut. Kunder kan också välja att använda sina egna krypteringsnycklar som genereras av den lokala maskinvarusäkerhetsmodul moduler (HSM) och även Kräv att Azure Key Vault-instanser är implementerade i en enda klient diskreta HSM: er.

<!-- ### Secure and immutable file storage

All Azure storage accounts are encrypted by default using Microsoft managed keys. Azure customers also have the ability to use their own encryption keys (BYOK) to encrypt blob, file and queue data so that even the hosting provider has no access to unencrypted data. Data immutability is often required for auditing purposes or when legal disputes call for data to be effectively frozen for a determined amount of time. Azure has recently introduced an [immutable data storage](https://docs.microsoft.com/azure/storage/blobs/storage-blob-immutable-storage) option known as Write-Once, Read many (WORM) for this scenario. -->

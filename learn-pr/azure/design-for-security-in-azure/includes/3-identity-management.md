Lamna Healthcare har ett äldre internt program och en webbportal som dess läkare använder för att hantera patienternas hälsodata. Organisationen har fått många begäranden om att det här programmet ska bli tillgängligt för vårdgivare som ofta är på plats med patienter och därför är utanför nätverket. 

En nyligen inträffad dataläcka från skadliga aktörer har tvingad företaget att stärka sina lösenordsprinciper. De kräver nu att användare ändrar sina lösenord oftare och att de använder längre, mer avancerade lösenord. Detta har medfört den oönskade sidoeffekten att användarna antecknar komplexa lösenord på ett osäkert sätt, eftersom de har svårt att komma ihåg flera uppsättningar med autentiseringsuppgifter som har skapats för olika administrativa roller. 

Här diskuterar vi identitet som ett säkerhetslager för interna och externa program, fördelarna med enkel inloggning (SSO) och multifaktorautentisering (MFA) för att tillhandahålla identitetssäkerhet, samt varför du bör överväga att replikera lokala identiteter till Azure Active Directory.

## <a name="identity-as-a-layer-of-security"></a>Identitet som ett säkerhetslager

Digitala identiteter är en central del av dagens verksamheter och social kommunikation lokalt och online. Tidigare var identitets-och åtkomsttjänster begränsade till att arbeta i ett företags interna nätverk med hjälp av protokoll som Kerberos och LDAP som utformats för detta ändamål. På senare tid har mobila enheter blivit det vanligaste sättet att interagera med digitala tjänster. Kunder och anställda räknar med att komma åt tjänster var och när som helst, vilket har drivit utvecklingen av identitetsprotokoll som fungerar i Internetskala på många olika enheter och operativsystem.

Lamna Healthcare utvärderar vilka funktioner deras arkitektur har vad gäller identitet och undersöker hur de kan implementera följande funktioner i sitt program:

- Tillhandahålla enkel inloggning för programanvändare
- Förbättra det äldre programmet till att använda modern autentisering med minimal arbetsinsats
- Använda multifaktorautentisering för alla inloggningar utanför företagets nätverk
- Utveckla ett program för att tillåta patienter att registrera och hantera sina kontodata på ett säkert sätt

## <a name="single-sign-on"></a>Enkel inloggning

Ju fler identiteter en användare behöver hantera, desto större risk för en säkerhetsincident relaterad till autentiseringsuppgifterna. Fler identiteter innebär flera lösenord för att komma ihåg och ändra. Lösenordsprinciper kan variera mellan program, och när kraven på komplexitet ökar blir det svårare för användare att komma ihåg dem.

Dessutom krävs omfattande hantering av alla dessa identiteter. Det leder även till ökad arbetsbörda för supportavdelningen att hantera kontolåsningar och begäranden för återställning av lösenord. Om en användare lämnar en organisation kan det vara svårt att spåra alla dessa identiteter och se till att de inaktiveras. Om en identitet är förbises möjliggör detta åtkomst när den borde ha tagits bort.

Med enkel inloggning behöver användarna bara komma ihåg ett ID och ett lösenord. Åtkomst för flera program beviljas till en enda identitet som är kopplad till en användare, vilket förenklar säkerhetsmodellen. När användare byter roller eller lämnar en organisation är åtkomständringarna knutna till en enda identitet, vilket avsevärt minskar det arbete som krävs för att ändra eller inaktivera konton. Med enkel inloggning för konton blir det enklare för användarna att hantera sina identiteter, och det ökar säkerhetsfunktionerna i din miljö.

### <a name="sso-with-azure-active-directory"></a>Enkel inloggning med Azure Active Directory

Azure Active Directory (AD) är en molnbaserad identitetstjänst. Det har inbyggt stöd för att synkronisera med din befintliga lokala Active Directory eller kan användas fristående.

Det innebär att alla dina program, oavsett om de är lokala, molnbaserade (inklusive Office 365) eller mobila kan dela samma autentiseringsuppgifter. 

Administratörer och utvecklare kan styra åtkomst till data och program med hjälp av centraliserade regler och principer som konfigureras i Azure AD.

Microsoft har dessutom en unik möjlighet att kombinera flera datakällor till en intelligent säkerhetsgraf som ger hotanalyser och identitetsskydd i realtid till alla konton i Azure Active Directory (även konton som synkroniseras från din lokala AD).

### <a name="synchronize-directories-with-ad-connect"></a>Synkronisera kataloger med AD Connect

Azure AD Connect integrerar dina lokala kataloger med Azure Active Directory. Azure AD Connect innehåller de nyaste funktionerna och ersätter äldre versioner av identitetsintegrationsverktyg som DirSync och Azure AD Sync.

Det är ett enda verktyg som innehåller allt som krävs för en enkel distributionsupplevelse för synkronisering och inloggning.

![AAD Connect](../media-draft/AADCONNECTxprs_960.jpg)

Lamna Healthcare kräver att autentisering sker främst mot lokala domänkontrollanter, men kräver även molnautentisering i ett katastrofåterställningsscenario. De har inte några krav som inte redan stöds av Azure AD.

Lamna Healthcare har bestämt sig för att gå vidare med följande konfiguration:

- Använda Azure AD Connect till att synkronisera grupper, användarkonton och lösenordshashar som lagras i deras lokala Active Directory till Azure AD
  - Detta kan användas som en reservmetod om direktautentisering inte är tillgänglig
- Konfigurera direktautentisering med hjälp av lokal autentiseringsagent som är installerad på en lokal Windows Server
- Använda funktionen för smidig enkel inloggning i Azure AD för att automatiskt logga in användare från lokala domänanslutna datorer
  - Minska friktionen för användare genom att utelämna flera autentiseringsbegäranden

## <a name="authentication--access"></a>Autentisering och åtkomst

Lamna Healthcares säkerhetsprincip kräver att alla inloggningar som sker utanför företagets perimeternätverk autentiseras med ytterligare en autentiseringsfaktor. Det här kravet kombinerar två delar av Azure AD-tjänsten: multifaktorautentisering och principer för villkorlig åtkomst.

### <a name="multi-factor-authentication"></a>Multifaktorautentisering

Multifaktorautentisering (MFA) ger ökad säkerhet för dina identiteter genom att kräva två eller flera element för fullständig autentisering. De här elementen är indelade i tre kategorier:

- *något du känner till*
- *något du har*
- *något du är*

**Något du känner** kan vara ett lösenord eller svaret på en säkerhetsfråga. **Något du har** kan vara en mobilapp som får ett meddelande eller en tokengenererande enhet. **Något du är** är vanligtvis någon form av biometrisk egenskap, till exempel ett fingeravtryck eller ansiktsskanning som används i många mobila enheter.

Användning av multifaktorautentisering ökar säkerheten för din identitet genom att begränsa effekten av exponering av autentiseringsuppgifter. En angripare som har en användares lösenord skulle även behöva ha tillgång till användarens telefon eller ansikte för att kunna autentisera fullständigt. Autentisering med bara en enda verifierad faktor är otillräckligt, och angriparen skulle inte kunna använda de autentiseringsuppgifterna för att autentisera. Detta medför stora säkerhetsfördelar, och det är mycket viktigt att multifaktorautentisering aktiveras närhelst det är möjligt.

Azure AD har funktioner för multifaktorautentisering inbyggda och kommer att integrera med andra tredjepartsleverantörer för multifaktorautentisering. Det tillhandahålls kostnadsfritt till alla användare som har rollen Global administratör i Azure AD eftersom dessa är mycket känsliga konton. Alla andra konton kan ha multifaktorautentisering aktiverat genom att köpa licenser med den här funktionen och tilldela en licens till kontot.

### <a name="conditional-access-policies"></a>Principer för villkorsstyrd åtkomst

Tillsammans med multifaktorautentisering går det att lägga till ytterligare en skyddsnivå genom att se till att ytterligare krav är uppfyllda innan åtkomst beviljas. Att blockera inloggningar från en misstänkt IP-adress eller neka åtkomst från enheter utan skydd mot skadlig kod kan begränsa åtkomst från riskfyllda inloggningar.

Azure Active Directory tillhandahåller en funktion för principer för villkorsstyrd åtkomst som har stöd för åtkomstprinciper baserat på grupp, plats eller enhetstillstånd. Platsfunktionen gör att Lamna kan särskilja IP-adresser som inte tillhör deras nätverk och uppfyller deras säkerhetsprincip, och kräva multifaktorautentisering från alla sådana platser.

Lamna Healthcare har skapat en [princip för villkorsstyrd åtkomst](https://docs.microsoft.com/azure/active-directory/conditional-access/overview) som kräver att användare som kommer åt programmet från en IP-adress utanför företagsnätverket prövas med multifaktorautentisering.

![villkorsstyrd åtkomst](../media-draft/conditional-access.png)

## <a name="securing-legacy-applications"></a>Säkra äldre program

Lamna Healthcares anställda behöver säker fjärråtkomst till sina administrativa program som finns lokalt. Användarna autentiserar för närvarande till programmet med hjälp av Windows-integrerad autentisering (WIA) från sina domänanslutna datorer bakom företagets brandvägg. Ett projekt för att införliva moderna autentiseringsmekanismer i programmet har planerats, men det finns betydande tryck på verksamheten att aktivera funktionerna för fjärråtkomst så snart som möjligt. Azure-programproxy kan snabbt, enkelt och säkert tillåta att programmet nås via en fjärranslutning utan några kodändringar.

Azure AD-programproxy är:

- Enkelt
  - Du behöver inte ändra eller uppdatera dina program för att fungera med programproxy.
  - Användarna får en konsekvent autentiseringsupplevelse. De kan använda MyApps-portalen för att få enkel inloggning till både SaaS-appar i molnet och dina appar lokalt.
- Säkerhet
  - När du publicerar dina appar med Azure AD-programproxy kan dra du nytta av de omfattande auktoriseringskontrollerna och säkerhetsanalyserna i Azure. Du får säkerhet på molnskala och Azure-säkerhetsfunktioner som villkorsstyrd åtkomst och tvåstegsverifiering.
  - Du behöver inte öppna några inkommande anslutningar via brandväggen för att ge dina användare fjärråtkomst.
- Kostnadseffektivt
  - Programproxy fungerar i molnet så att du kan spara tid och pengar. Lokala lösningar kräver normalt att du konfigurerar och underhåller DMZ:er, gränsservrar eller andra komplexa infrastrukturer.

Azure Active Directory-programproxy består av två komponenter: en anslutningsagent som finns på en Windows-server i företagsnätverket och en extern slutpunkt, antingen MyApps-portalen eller en extern URL. När en användare navigerar till slutpunkten autentiserar denne med Azure AD och dirigeras till det lokala programmet via anslutningsagenten.

## <a name="working-with-consumer-identities"></a>Arbeta med konsumentidentiteter

Sedan Lamna Healthcare integrerade modern autentisering med sitt befintliga program har de snabbt insett de möjligheter som ett system för hanterad identitet såsom Azure AD kan ge deras organisation. Ledningen vill nu utforska andra sätt som Microsofts identitetstjänster kan ge affärsvärde på. De riktar nu sin uppmärksamhet på externa kunder och hur modernisering av befintliga kundinteraktioner skulle kunna ge nära integrering med identitetsleverantörer från tredje part, till exempel Google, Facebook och LinkedIn.

Azure AD B2C är en identitetshanteringstjänst som bygger på de gedigna grunderna i Azure Active Directory och som hjälper dig att anpassa och styra hur kunderna registrerar sig, loggar in och hanterar sina profiler när de använder dina program. Detta omfattar program som har utvecklas för bland annat iOS, Android och .NET. Azure AD B2C tillhandahåller en inloggningsupplevelse för social identitet och skyddar dessutom profilinformationen för dina kundidentiteter. Azure AD B2C-kataloger skiljer sig från vanliga Azure AD-kataloger och kan skapas i Azure-portalen.

## <a name="identity-management-at-lamna-healthcare"></a>Identitetshantering på Lamna Healthcare

Vi har sett hur Lamna Healthcare har använt identitetshanteringslösningar på Azure för att förbättra säkerheten för deras miljö. De har börjat genom att ge användarna enkel inloggning för att minimera det antal konton som användarna behöver hantera och minska den driftskomplexitet som ett överdrivet antal konton medför. De har tillämpat multifaktorautentisering för åtkomst till sina program och har uppdaterat ett äldre program till att använda modern autentisering med minimal arbetsinsats. De har även lärt sig hur de kan förbättra sin förmåga att arbeta med konsumentidentiteter, vilket förbättrar programmets användbarhet för patienterna.

## <a name="summary"></a>Sammanfattning

I den här enheten har vi gått igenom hur ett antal Azure Active Directory-funktioner kan kombineras för att tillhandahålla en stabil identitetslösning för att skydda åtkomst till program, oavsett vilken plats de har. Identitet är ett kritiskt säkerhetslager. När det är väl utformat och ingår i arkitekturen kan du säkerställa att din miljö är säker.
Nätverksperimetrar, deras brandväggar samt fysiska åtkomstkontroller brukade vara det primära skyddet för företagets data. Men nätverksperimetrar har blivit allt osäkrare på grund av allt fler BYOD-enheter (Bring Your Own Device), mobilappar och molnprogram. 

Identiteten har blivit den nya primära säkerhetsgränsen. Därför är korrekt autentisering och tilldelning av behörigheter viktigt för att bibehålla kontrollen över dina data.

Ditt företag Contoso Shipping fokuserar på att hantera dessa frågor direkt. Ditt teams nya hybridmolnlösning behöver ta hänsyn till mobila appar som har åtkomst till hemliga data när en auktoriserad användare är inloggad &mdash; förutom att leveransfordon ständigt skickar en ström med telemetridata som är viktigt för att optimera verksamheten.

## <a name="single-sign-on"></a>Enkel inloggning

Ju fler identiteter en användare behöver hantera, desto större risk för en säkerhetsincident relaterad till autentiseringsuppgifterna. Fler identiteter innebär flera lösenord att komma ihåg och ändra. Lösenordsprinciper kan variera mellan program, och när kraven på komplexitet ökar blir det svårare för användare att komma ihåg dem.

Nu kan du överväga logistiken med att hantera dessa identiteter. Det leder även till ökad arbetsbörda för supportavdelningen att hantera kontolåsningar och begäranden för återställning av lösenord. Om en användare lämnar en organisation kan det vara svårt att spåra alla dessa identiteter och se till att de inaktiveras. Om en identitet är förbises möjliggör detta åtkomst när den borde ha tagits bort.

Med enkel inloggning (SSO) behöver användarna bara komma ihåg ett ID och ett lösenord. Åtkomst för flera program beviljas till en enda identitet som är kopplad till en användare, vilket förenklar säkerhetsmodellen. När användare byter roller eller lämnar en organisation är åtkomständringarna knutna till en enda identitet, vilket avsevärt minskar det arbete som krävs för att ändra eller inaktivera konton. Med enkel inloggning för konton blir det enklare för användarna att hantera sina identiteter, och det ökar säkerhetsfunktionerna i din miljö.

:::row:::
  :::column:::
    ![Ett tumavtryck som representerar Azure Active Directory](../media/3-sso-with-azure-ad.png)
  :::column-end:::
    :::column span="3"::: **Enkel inloggning med Azure Active Directory**

Azure Active Directory (AD) är en molnbaserad identitetstjänst. Det har inbyggt stöd för att synkronisera med din befintliga lokala Active Directory eller kan användas fristående. Det innebär att alla dina program, oavsett om de är lokala, molnbaserade (inklusive Office 365) eller mobila kan dela samma autentiseringsuppgifter. Administratörer och utvecklare kan styra åtkomst till data och program med hjälp av centraliserade regler och principer som konfigureras i Azure AD.

Genom att dra nytta på Azure AD för enkel inloggning har du också möjligheten att kombinera flera datakällor i ett intelligent säkerhetsdiagram. Det här säkerhetsdiagrammet låter dig erbjuda hotanalys och identitetsskydd i realtid för alla konton i Azure AD, inklusive konton som synkroniseras från din lokala AD. Med hjälp av en centraliserad identitetsprovider, kommer du att ha centraliserade säkerhetskontroller, rapportering, aviseringar och administration av din identitetsinfrastruktur.

När Contoso Shipping integrerar sin befintliga Active Directory-instans med Azure AD, kommer du att göra åtkomstkontroll konsekvent över hela organisationen. Detta förenklar också möjligheten att logga in på e-post och Office 365-dokument utan att behöva autentisera sig på nytt.
  :::column-end:::
:::row-end:::

## <a name="multi-factor-authentication"></a>Multifaktorautentisering

Multifaktorautentisering (MFA) ger ökad säkerhet för dina identiteter genom att kräva två eller flera element för fullständig autentisering. De här elementen är indelade i tre kategorier:

- *Något du känner till*
- *Något du har*
- *Något du är*

**Något du känner till** kan vara ett lösenord eller svaret på en säkerhetsfråga. **Något du har** kan vara en mobilapp som tar emot ett meddelande eller en tokenskapande enhet. **Något du är** är vanligtvis någon form av biometrisk egenskap, till exempel ett fingeravtryck eller ansiktsskanning som används i många mobila enheter.

Användning av multifaktorautentisering ökar säkerheten för din identitet genom att begränsa effekten av exponering av autentiseringsuppgifter. En angripare som har en användares lösenord skulle även behöva ha tillgång till användarens telefon eller ansikte för att kunna autentisera fullständigt. Autentisering med bara en enda verifierad faktor är otillräckligt och angriparen skulle inte kunna använda de autentiseringsuppgifterna för att autentisera. Det här innebär enorma fördelar för säkerheten och vi kan inte nog betona vikten av att aktivera MFA där det är möjligt.

Azure AD har inbyggda funktioner för MFA och kommer att integrera med andra tredjepartsleverantörer för multifaktorautentisering. Det erbjuds kostnadsfritt till alla användare som har rollen Global administratör i Azure AD eftersom dessa är mycket känsliga konton. Alla andra konton kan få MFA aktiverat genom att köpa licenser med den funktionen &mdash; samt genom att tilldela en licens till kontot.

För Contoso Shipping väljer du att aktivera MFA varje gång en användare loggar in från en dator som inte är ansluten via en domän &mdash; vilket inkluderar mobilapparna som era chaufförer använder.

## <a name="providing-identities-to-services"></a>Tilldela identiteter till tjänster

Det är ofta bra att tilldela identiteter till tjänster. Ofta och i strid med bästa praxis, bäddas autentiseringsuppgifter in i konfigurationsfiler. Om dessa konfigurationsfiler inte skyddas kan vem som helst med åtkomst till systemen eller lagringsplatserna komma åt dessa uppgifter eller öka risken för att de exponeras.

Azure AD löser det här problemet med hjälp av två metoder: tjänstens huvudnamn och hanterade identiteter för Azure-tjänster.

:::row:::
  :::column:::
    ![Bild som representerar olika roller](../media/3-service-principals.png)
  :::column-end:::
    :::column span="3"::: **Tjänstens huvudnamn**

För att förstå vad tjänstens huvudnamn är, måste du först förstå vad orden **identitet** och **huvudnamn** betyder när de används inom identitetshantering.

En **identitet** är helt enkelt något som kan autentiseras. Exempel på detta är användare med användarnamn och lösenord, men det kan även vara program eller andra servrar, som kan autentiseras med hemliga nycklar eller certifikat. Det kan även vara bra att veta att ett **konto** är data som är associerade med en identitet.

Ett **huvudnamn** är en identitet som fungerar med vissa roller eller anspråk. Istället för att tänka på identitet och huvudnamn separat är det användbart att tänka på att använda `sudo` i en Bash-prompt i Linux eller i Windows med kör som Administratör. I båda dessa fall är du fortfarande inloggad med samma identitet som innan, men du har ändrat rollen som du kör under. Även grupper betraktas ofta som huvudkonton, eftersom de kan ha en tilldelad behörighet.

**Tjänstens huvudnamn** namnges alltså rent bokstavligen. Det här är en identitet som används av en tjänst eller ett program. Precis som andra identiteter kan den tilldelas roller.
  :::column-end:::
:::row-end:::

:::row:::
  :::column:::
    ![Bild som representerar hanterade identiteter](../media/3-managed-service-identities.png)
  :::column-end:::
    :::column span="3"::: **Hanterade identiteter för Azure-tjänster**

Det kan vara tidskrävande att skapa tjänstens huvudnamn och det finns många kontaktpunkter, vilket ofta gör det svårt att underhålla dem. Hanterade identiteter för Azure-tjänster är mycket enklare och tar hand om merparten av arbetet åt dig. 

En hanterad identitet kan skapas direkt för valfri Azure-tjänst som stöder det&mdash;och listan växer hela tiden. När du skapar en hanterad identitet för en tjänst, skapar du ett konto i Azure AD-klientorganisationen. Azure-infrastrukturen tar automatiskt hand om autentiseringen av tjänsten och hanteringen av kontot. Du kan sedan använda det kontot på samma sätt som andra Azure AD-konton och på ett säkert sätt ge den autentiserade tjänsten åtkomst till andra Azure-resurser.
  :::column-end:::
:::row-end:::

## <a name="role-based-access-control"></a>Rollbaserad åtkomstkontroll

Roller är uppsättningar med behörigheter, t.ex. ”Skrivskyddad” eller ”Deltagare”, som användare kan beviljas för att få åtkomst till en Azure-tjänstinstans. 

Identiteter mappas till roller direkt eller via gruppmedlemskap. Genom att säkerhetsobjekt, åtkomstbehörigheter och resurser hålls åtskilda, blir det enklare att hantera och kontrollera åtkomsten. Administratörer kan se till att de minsta nödvändiga behörigheterna har beviljats.

Roller kan tilldelas på nivån för en enskild tjänstinstans eller spridas nedåt i Azure Resource Manager-hierarkin. Roller som har tilldelats i en högre omfattning, t.ex. en hel prenumeration, ärvs av underordnade områden, t.ex. tjänstinstanser. 

![Hanteringsgrupper](../media/3-role-assignment-scope.png)

:::row:::
  :::column:::
    ![Bild som representerar identitetsverifiering](../media/3-privileged-identity-management.png)
  :::column-end:::
    :::column span="3"::: **Privileged Identity Management**

Förutom att hantera Azure-resursåtkomst med rollbaserad åtkomstkontroll (RBAC), bör en heltäckande säkerhetsstrategi för infrastrukturen omfatta regelbunden granskning av rollmedlemmar allt eftersom organisationen förändras och utvecklas. Betaltjänsten Azure AD Privileged Identity Management (PIM) hjälper dig att övervaka rolltilldelningar, självbetjäning och JIT-rollaktivering (Just-In-Time) och innehåller även funktioner för att granska åtkomsten till Azure AD-resurser.

![Privileged Identity Management](../media/PIM_Dashboard.png)

  :::column-end:::
:::row-end:::

## <a name="summary"></a>Sammanfattning

Med identiteter kan vi behålla en säkerhetsperimeter även utanför vår fysiska kontroll. Med enkel inloggning och lämplig rollbaserad åtkomstkonfiguration kan vi alltid vara säkra på vem som har möjlighet att se och ändra våra data och vår infrastruktur.
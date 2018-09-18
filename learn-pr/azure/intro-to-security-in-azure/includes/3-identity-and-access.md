Nätverksperimetrar, deras brandväggar samt fysiska åtkomstkontroller brukade vara det primära skyddet för företagets data. Men nätverksperimetrar har blivit allt osäkrare på grund av allt fler BYOD-enheter (Bring Your Own Device), mobilappar och molnprogram. 

Identiteten har blivit den nya primära säkerhetsgränsen. Korrekt autentisering och tilldelning av behörigheter är viktigt för att bibehålla kontrollen över dina data.

Contoso hanterar dessa problem direkt. Deras nya hybridmolnlösning behöver ett konto för mobilappar som har åtkomst till hemliga data när en behörig användare är inloggad. Deras fraktfordon skickar också en konstant ström med telemetridata som är viktig för att de ska kunna optimera sin verksamhet.

## <a name="single-sign-on"></a>Enkel inloggning

Ju fler identiteter en användare behöver hantera, desto större risk för en säkerhetsincident relaterad till autentiseringsuppgifterna. Fler identiteter innebär flera lösenord för att komma ihåg och ändra. Lösenordsprinciper kan variera mellan program, och när kraven på komplexitet ökar blir det svårare för användare att komma ihåg dem.

Dessutom krävs omfattande hantering av alla dessa identiteter. Det leder även till ökad arbetsbörda för supportavdelningen att hantera kontolåsningar och begäranden för återställning av lösenord. Om en användare lämnar en organisation kan det vara svårt att spåra alla dessa identiteter och se till att de inaktiveras. Om en identitet är förbises möjliggör detta åtkomst när den borde ha tagits bort.

Med enkel inloggning behöver användarna bara komma ihåg ett ID och ett lösenord. Åtkomst för flera program beviljas till en enda identitet som är kopplad till en användare, vilket förenklar säkerhetsmodellen. När användare byter roller eller lämnar en organisation är åtkomständringarna knutna till en enda identitet, vilket avsevärt minskar det arbete som krävs för att ändra eller inaktivera konton. Med enkel inloggning för konton blir det enklare för användarna att hantera sina identiteter, och det ökar säkerhetsfunktionerna i din miljö.

### <a name="sso-with-azure-active-directory"></a>Enkel inloggning med Azure Active Directory

Azure Active Directory (AD) är en molnbaserad identitetstjänst. Det har inbyggt stöd för att synkronisera med din befintliga lokala Active Directory eller kan användas fristående.

Det innebär att alla dina program, oavsett om de är lokala, molnbaserade (inklusive Office 365) eller mobila kan dela samma autentiseringsuppgifter. 

Administratörer och utvecklare kan styra åtkomst till data och program med hjälp av centraliserade regler och principer som konfigureras i Azure AD.

Microsoft har dessutom en unik möjlighet att kombinera flera datakällor till en intelligent säkerhetsgraf som visar hotanalyser och identitetsskydd i realtid för alla konton i Azure Active Directory (även konton som synkroniseras från din lokala AD).

Contoso integrerar sin befintliga Active Directory med Azure AD, eftersom de då får en konsekvent åtkomstkontroll i hela organisationen. Det gör det också enkelt för användarna att komma åt sin e-post och sina Office 365-dokument utan att behöva autentisera på nytt.

## <a name="multi-factor-authentication"></a>Multifaktorautentisering

Multifaktorautentisering (MFA) ger ökad säkerhet för dina identiteter genom att kräva två eller flera element för fullständig autentisering. De här elementen är indelade i tre kategorier:

- *något du känner till*
- *något du har*
- *något du är*

**Något du känner** kan vara ett lösenord eller svaret på en säkerhetsfråga. **Något du har** kan vara en mobilapp som får ett meddelande eller en tokengenererande enhet. **Något du är** är vanligtvis någon form av biometrisk egenskap, till exempel ett fingeravtryck eller ansiktsskanning som används i många mobila enheter.

Användning av multifaktorautentisering ökar säkerheten för din identitet genom att begränsa effekten av exponering av autentiseringsuppgifter. En angripare som har en användares lösenord skulle även behöva ha tillgång till användarens telefon eller ansikte för att kunna autentisera fullständigt. Autentisering med bara en enda verifierad faktor är otillräckligt, och angriparen skulle inte kunna använda de autentiseringsuppgifterna för att autentisera. Detta medför stora säkerhetsfördelar, och det är mycket viktigt att multifaktorautentisering aktiveras närhelst det är möjligt.

Azure AD har funktioner för multifaktorautentisering inbyggda och kommer att integrera med andra tredjepartsleverantörer för multifaktorautentisering. Det tillhandahålls kostnadsfritt till alla användare som har rollen Global administratör i Azure AD eftersom dessa är mycket känsliga konton. Alla andra konton kan ha MFA aktiverat genom att köpa licenser med den här funktionen och tilldela en licens till kontot.

För Contoso Shipping väljer du att aktivera MFA varje gång en användare loggar in från en dator som inte är ansluten via en domän. Det innefattar mobilappar.

## <a name="providing-identities-to-services"></a>Tilldela identiteter till tjänster

Det är ofta bra att tilldela identiteter till tjänster. Ofta, och i strid med bästa praxis, bäddas autentiseringsuppgifter in i konfigurationsfiler. Om dessa konfigurationsfiler inte skyddas kan vem som helst med åtkomst till systemen eller lagringsplatserna komma åt dessa uppgifter eller öka risken för att de exponeras.

Azure AD löser det här problemet med hjälp av två metoder: tjänstens huvudnamn och hanterade tjänstidentiteter.

### <a name="service-principals"></a>Tjänstens huvudnamn

För att förstå vad tjänstens huvudnamn är, måste du först förstå vad orden **identitet** och **huvudnamn** betyder när de används inom identitetshantering.

En **identitet** är helt enkelt något som kan autentiseras. Exempel på detta är användare med användarnamn och lösenord, men det kan även vara program eller andra servrar, som kan autentiseras med hemliga nycklar eller certifikat. Det kan även vara bra att veta att ett **konto** är data som är associerade med en identitet.

Ett **huvudnamn** är en identitet som fungerar med vissa roller eller anspråk. I stället för att tänka på identitet och huvudnamn som separata företeelser kan du likna dem vid att använda ”sudo” i en bash-kommandotolk eller i Windows med ”Kör som administratör”. I båda dessa fall är du fortfarande inloggad med samma identitet som innan, men du har ändrat rollen som du kör under. Även grupper betraktas ofta som huvudkonton, eftersom de kan ha en tilldelad behörighet.

**Tjänstens huvudnamn** namnges alltså rent bokstavligen. Det här är en identitet som används av en tjänst eller ett program. Precis som andra identiteter kan den tilldelas roller. 

### <a name="managed-service-identities"></a>Hanterade tjänstidentiteter

Det kan vara tidskrävande att skapa tjänstens huvudnamn och det finns många kontaktpunkter, vilket ofta gör det svårt att underhålla dem. Hanterade tjänstidentiteter (MSI) är mycket enklare och tar hand om merparten av arbetet åt dig. 

En MSI kan skapas direkt för valfri Azure-tjänst som stöder den (listan växer hela tiden). När du skapar en MSI för en tjänst, skapar du ett konto i Azure Active Directory-klientorganisationen. Azure-infrastrukturen tar automatiskt hand om autentiseringen av tjänsten och hanteringen av kontot. Du kan sedan använda det kontot på samma sätt som andra AD-konton, och på ett säkert sätt ge den autentiserade tjänsten åtkomst till andra Azure-resurser.

## <a name="role-based-access-control"></a>Rollbaserad åtkomstkontroll

Roller är uppsättningar med behörigheter, t.ex. ”Skrivskyddad” eller ”Deltagare”, som användare kan beviljas för att få åtkomst till en Azure-tjänstinstans. 

Identiteter mappas till roller direkt eller via gruppmedlemskap. Genom att säkerhetsobjekt, åtkomstbehörigheter och resurser hålls åtskilda, blir det enklare att hantera och kontrollera åtkomsten. Administratörer kan se till att de minsta nödvändiga behörigheterna har beviljats.

Roller kan tilldelas på nivån för en enskild tjänstinstans eller spridas nedåt i Azure Resource Manager-hierarkin. Roller som har tilldelats för ett högre område, t.ex. en hel prenumeration, ärvs av underordnade områden, t.ex. tjänstinstanser. 

<!--TODO: replace with final media which was submitted for Design-for-security-in-azure -->
![Hanteringsgrupper](../media-draft/3-role-assignment-scope.png)

### <a name="privileged-identity-management"></a>Privileged Identity Management

Förutom att hantera åtkomsten till Azure-resurser med RBAC, bör en heltäckande säkerhetsstrategi för infrastrukturen omfatta regelbunden granskning av rollmedlemmar allt eftersom organisationen förändras och utvecklas. Betaltjänsten Azure AD Privileged Identity Management (PIM) hjälper dig att övervaka rolltilldelningar, självbetjäning och JIT-rollaktivering (Just-In-Time), samt innehåller även funktioner för att granska åtkomsten till Azure AD-resurser.

<!--TODO: replace with final media which was submitted for Design-for-security-in-azure -->
![Privileged Identity Management](../media-COPIED-FROM-DESIGNFORSECURITY/PIM_Dashboard.PNG)

Med identiteter kan vi behålla en säkerhetsperimeter även utanför vårt fysiska kontroll. Med enkel inloggning och lämplig rollbaserad åtkomstkonfiguration kan vi alltid vara säkra på vem som har möjlighet att se och ändra våra data och vår infrastruktur.
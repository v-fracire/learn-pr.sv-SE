Nätverket perimetrar och deras brandväggar och fysiska åtkomstkontroller som används för att vara det primära skyddet för företagets data. Men nätverk perimetrar har blivit allt högre grad porös med alltfler ta med din egen enhet (BYOD), mobila appar, program och molnprogram. 

Identiteten har blivit nya primära säkerhetsgräns. Korrekt autentisering och tilldelning av behörigheter som är viktigt att bibehålla kontrollen över dina data.

Contoso leverans arbetar med dessa problem direkt. Sina nya hybridmolnlösning måste kontot för mobila appar som har åtkomst till hemliga data när en behörig användare är inloggad. De har också produkter skickar en konstant ström av telemetridata som är viktigt att optimera sin verksamhet.

## <a name="single-sign-on"></a>Enkel inloggning

Ju fler identiteter en användare behöver hantera, desto större risk för en säkerhetsincident relaterad till autentiseringsuppgifterna. Fler identiteter innebär flera lösenord för att komma ihåg och ändra. Lösenordsprinciper kan variera mellan program och när kraven på komplexitet ökar, det gör det svårare för användare att komma ihåg dem.

Dessutom krävs omfattande hantering av alla dessa identiteter. Det leder även till ökad arbetsbörda för supportavdelningen att hantera kontolåsningar och begäranden för återställning av lösenord. Om en användare lämnar en organisation kan det vara svårt att spåra alla dessa identiteter och se till att de inaktiveras. Om en identitet är förbises möjliggör detta åtkomst när den borde ha tagits bort.

Med enkel inloggning (SSO) måste användare att komma ihåg endast ett ID och ett lösenord. Åtkomst för flera program beviljas till en enda identitet som är kopplad till en användare, vilket förenklar säkerhetsmodellen. När användare byter roller eller lämnar en organisation är åtkomständringarna knutna till en enda identitet, vilket avsevärt minskar det arbete som krävs för att ändra eller inaktivera konton. Med hjälp av enkel inloggning för konton som gör det enklare för användarna att hantera identiteter och ökar säkerhetsfunktioner i din miljö.

### <a name="sso-with-azure-active-directory"></a>Enkel inloggning med Azure Active Directory

Azure Active Directory (Azure AD) är en molnbaserad identitetstjänst. Det har inbyggt stöd för att synkronisera med din befintliga lokala Active Directory-instans, eller den kan användas fristående.

Det innebär att alla dina program, oavsett om de är lokala, molnbaserade (inklusive Office 365) eller mobila kan dela samma autentiseringsuppgifter. 

Administratörer och utvecklare kan styra åtkomst till data och program med hjälp av centraliserade regler och principer som konfigureras i Azure AD.

Microsoft är dessutom vara placerat om du vill kombinera flera datakällor i en intelligent security graph som ger hotanalyser och i realtid identity protection till alla konton i Azure Active Directory (även konton som synkroniseras från din lokala Active Directory-instans).

Contoso leverans integrerar sina befintliga Active Directory-instans med Azure AD. Det gör åtkomstkontroll konsekvent i organisationen. Det gör det också enkelt för användare att få sin e-post och Office 365-dokument utan att autentiseras på nytt.

## <a name="multi-factor-authentication"></a>Multi-Factor Authentication

Multifaktorautentisering (MFA) ger ökad säkerhet för dina identiteter genom att kräva två eller flera element för fullständig autentisering. De här elementen är indelade i tre kategorier:

- *något du känner till*
- *något du har*
- *något du är*

**Något du känner** skulle vara ett lösenord eller svaret på en säkerhetsfråga. **Något du har** kan vara en mobil app som tar emot ett meddelande eller en enhet som genererar token. **Något du är** är vanligtvis någon form av biometriska egenskap, till exempel ett fingeravtryck eller ansiktslista genomsökning som används i många mobila enheter.

Med hjälp av MFA ökar säkerheten för din identitet genom att begränsa effekten av exponering av autentiseringsuppgifter. En angripare som har en användares lösenord skulle även behöva ha tillgång till användarens telefon eller ansikte för att kunna autentisera fullständigt. Angriparen skulle kunna använda dessa autentiseringsuppgifter för att autentisera autentisering med bara en enda faktor som verifierats är otillräcklig. Detta medför stora säkerhetsfördelar, och det är mycket viktigt att multifaktorautentisering aktiveras närhelst det är möjligt.

Azure AD har MFA-funktioner som är inbyggda och kommer att integrera med andra tredje parts MFA-leverantörer. Den tillhandahålls kostnadsfritt till alla användare som har rollen Global administratör i Azure AD, eftersom de är mycket känsliga konton. Alla andra konton kan ha multifaktorautentisering aktiverat genom att köpa licenser med den här funktionen och tilldela en licens till kontot.

För Contoso leverans, kan du välja att aktivera MFA varje gång en användare loggar in från en dator med icke-domain-anslutna. Det omfattar mobila appar.

## <a name="providing-identities-to-services"></a>Tilldela identiteter till tjänster

Det är ofta bra att tilldela identiteter till tjänster. Ofta och mot bästa praxis är autentiseringsuppgifter inbäddad i konfigurationsfiler. Om dessa konfigurationsfiler inte skyddas kan vem som helst med åtkomst till systemen eller lagringsplatserna komma åt dessa uppgifter eller öka risken för att de exponeras.

Azure AD löser det här problemet med hjälp av två metoder: tjänstens huvudnamn och hanterade tjänstidentiteter.

### <a name="service-principals"></a>Tjänstens huvudnamn

För att förstå tjänstens huvudnamn, det är bra att först förstå orden **identitet** och **huvudnamn**, eftersom de används i identity management världen.

En **identitet** är bara något som kan autentiseras. Naturligtvis är detta omfattar även användare med ett användarnamn och lösenord, men den kan även innehålla program eller andra servrar som kan autentiseras med hemliga nycklar eller certifikat. I detta sammanhang kan det även vara bra att veta att **konto** definieras som data som är associerade med en identitet.

Ett **huvudnamn** är en identitet som fungerar med vissa roller eller anspråk. Ofta är det inte praktiskt att tänka på identitets- och huvudnamn separat, men tänk på hur du använder `sudo` på en bash-kommandotolk eller på Windows med hjälp av ”kör som administratör”. I dessa båda fallen kan du fortfarande är inloggad som samma identitet som innan, men du har ändrat rollen som du kör. Grupper betraktas ofta även huvudkonton eftersom de kan ha behörigheten.

Därför en **tjänstens huvudnamn** heter bokstavligt. Det här är en identitet som används av en tjänst eller ett program. Det kan tilldelas roller som andra identiteter. 

### <a name="managed-service-identities"></a>Hanterade tjänstidentiteter

Skapa tjänstens huvudnamn kan vara en tidsödande process och det finns en massa kontaktpunkter som kan göra att underhålla dem. svårt. Hanterade tjänstidentiteter är mycket enklare och gör det mesta av arbetet åt dig. 

En hanterad tjänstidentitet kan skapas direkt för alla Azure-tjänster som stöder den (listan växer hela tiden). När du skapar en hanterad tjänstidentitet för en tjänst, skapar du ett konto på Azure Active Directory-klient. Azure-infrastrukturen hand automatiskt tar om autentisering av tjänsten och hantera kontot. Du kan sedan använda det kontot som alla andra Azure AD-konto, inklusive på ett säkert sätt att låta tjänsten autentiserad åtkomst till andra Azure-resurser.

## <a name="role-based-access-control"></a>Rollbaserad åtkomstkontroll

Roller är en uppsättning behörigheter, som ”skrivskyddad” eller ”bidragsgivare” att användare kan få åtkomst till en Azure-tjänstinstans. 

Identiteter är mappade till roller direkt eller via gruppmedlemskap. Att ange säkerhetsobjekt, behörigheter och resurser ger enkel hantering och detaljerad kontroll. Administratörer kan se till att de minsta nödvändiga behörigheterna har beviljats.

Roller kan tilldelas på instansnivå enskild tjänst, men de också flödar nedåt i Azure Resource Manager-hierarkin. Roller som tilldelats för ett högre omfång, t.ex. en hel prenumeration, ärvs av underordnade omfång, t.ex. tjänstinstanser. 

<!--TODO: replace with final media which was submitted for Design-for-security-in-azure -->
![Hanteringsgrupper](../media-draft/3-role-assignment-scope.png)

### <a name="privileged-identity-management"></a>Privileged Identity Management

Utöver att hantera Azure-resursåtkomst med rollbaserad åtkomst kontroll (RBAC), en omfattande metod för att infrastrukturen bör inklusive pågående granskning av rollmedlemmar som deras organisation ändringar och utvecklas. Azure AD Privileged Identity Management (PIM) är ett ytterligare, betalat för erbjudande som ger översyn av rolltilldelningar och självbetjäning just-in-time rollaktivering och Azure AD och Azure-resurs-åtkomstgranskningar.

<!--TODO: replace with final media which was submitted for Design-for-security-in-azure -->
![Privileged identity management](../media-COPIED-FROM-DESIGNFORSECURITY/PIM_Dashboard.png)

Identitet kan vi underhåller en säkerhetsperimeter även utanför vårt fysisk kontroll. Med enkel inloggning och konfiguration för rätt rollbaserad åtkomst vara vi alltid säker på vem som har möjlighet att se och ändra våra data och infrastruktur.
Lamna Healthcare hade nyligen ett allvarligt avbrott i en kundinriktad webbapp. En tekniker hade beviljats fullständig åtkomst till en resursgrupp som innehöll själva produktionswebbprogrammet. Teknikern råkade av misstag ta bort hela resursgruppen och alla underordnade resurser, inklusive databasen med alla kunddata. 

Lyckligtvis var programmets källkod och resurser tillgängliga i källkontrollen och regelbundna säkerhetskopieringar kördes automatiskt enligt ett schema. Tjänsten kunde därför återställas relativt enkelt. Här ska vi utforska hur det här avbrottet hade kunnat undvikas med hjälp av funktioner i Azure som skyddar åtkomsten till infrastrukturen.

## <a name="criticality-of-infrastructure"></a>Den viktiga infrastrukturen

Molninfrastrukturen blir en allt viktigare komponent på många företag. Det är viktigt att användare och processer endast beviljas de behörigheter som de behöver för att utföra sina jobb. Tilldelar felaktig åtkomst kan resultera i dataförlust, data sprids, eller tjänster blir otillgänglig. 

Systemadministratörer kan ansvara för ett stort antal användare, system, och behörighetsuppsättningar. Därför kan det snabbt bli svårt att bevilja åtkomst på ett optimalt sätt, vilket ofta resulterar i en alltför generell standardtillämpning. Den här metoden kan visserligen förenkla administrationen, men gör det betydligt lättare att oavsiktligt bevilja mer omfattande behörigheter än vad som egentligen krävs.

## <a name="role-based-access-control"></a>Rollbaserad åtkomstkontroll

Rollbaserad åtkomstkontroll (RBAC) fungerar på ett lite annorlunda sätt. Roller definieras som samlingar med åtkomstbehörigheter. Säkerhetsobjekt mappas till roller direkt eller via gruppmedlemskap. Genom att säkerhetsobjekt, åtkomstbehörigheter och resurser hålls åtskilda blir det enklare att hantera och kontrollera åtkomsten.

I Azure lagras användare, grupper och roller i Azure Active Directory (AD Azure). Azure Resource Manager-API:et använder rollbaserad åtkomstkontroll för att skydda all åtkomsthantering för resurser i Azure.

![ACL-baserad åtkomst](../media-draft/ACL_Based_Access.png)

<!-- ![Role-based access control](../media-draft/Role_Based_Access.png)
 -->

### <a name="roles-and-management-groups"></a>Roller och hanteringsgrupper

Roller är en uppsättning behörigheter, som ”skrivskyddad” eller ”bidragsgivare” att användare kan få åtkomst till en Azure-tjänstinstans. Roller kan tilldelas på instansnivå enskild tjänst, men de också flödar nedåt i Azure Resource Manager-hierarkin. Roller som tilldelats för ett högre omfång, t.ex. en hel prenumeration, ärvs av underordnade omfång, t.ex. tjänstinstanser. 

Hanteringsgrupper är en ytterligare hierarkisk nivå som nyligen introducerades i RBAC-modellen. Hanteringsgrupper gör det möjligt att gruppera prenumerationer och att tillämpa principen på en ännu högre nivå.

Möjligheten att sprida roller via en godtyckligt definierad prenumerationshierarki gör dessutom att administratörer kan bevilja tillfällig åtkomst till en hel miljö för autentiserade användare. En granskare kan exempelvis behöva tillfällig skrivskyddad åtkomst till alla prenumerationer.

![Hanteringsgrupper](../media-draft/management_groups.png)

### <a name="privileged-identity-management"></a>Privileged Identity Management

Förutom att hantera åtkomsten till Azure-resurser med RBAC, bör en heltäckande säkerhetsstrategi för infrastrukturen omfatta regelbunden granskning av rollmedlemmar allt eftersom organisationen förändras och utvecklas. Betaltjänsten Azure AD Privileged Identity Management (PIM) hjälper dig att övervaka rolltilldelningar, självbetjäning och JIT-rollaktivering (Just-In-Time) och innehåller även funktioner för att granska åtkomsten till Azure AD-resurser.

![Privileged Identity Management](../media-draft/PIM_Dashboard.png)

## <a name="providing-identities-to-services"></a>Tilldela identiteter till tjänster

Det är ofta bra att tilldela identiteter till tjänster. Ofta, och i strid med bästa praxis, bäddas autentiseringsuppgifter in i konfigurationsfiler. Om dessa konfigurationsfiler inte skyddas kan vem som helst med åtkomst till systemen eller lagringsplatserna komma åt dessa uppgifter eller öka risken för att de exponeras.

Azure AD åtgärdar problemet med två metoder: tjänsten huvudnamn och hanterade identiteter för Azure-tjänster.

### <a name="service-principals"></a>Tjänstens huvudnamn

För att förstå tjänstens huvudnamn, det är bra att först förstå orden **identitet** och **huvudnamn** när de används i Identity management världen.

En **identitet** är bara något som kan autentiseras. Naturligtvis det omfattar även användare med användarnamn och lösenord, men den kan även innehålla program eller andra servrar som kan autentiseras med hemliga nycklar eller certifikat. I detta sammanhang kan det även vara bra att veta att **konto** definieras som data som är associerade med en identitet.

Ett **huvudnamn** är en identitet som fungerar med vissa roller eller anspråk. I stället för att tänka på identitet och huvudnamn som separata företeelser kan du likna dem vid att använda ”sudo” i en bash-kommandotolk eller i Windows med ”Kör som administratör”. I båda dessa fall kan du fortfarande är inloggad som samma identitet som innan, men du har ändrat rollen som du kör.

**Tjänstens huvudnamn** namnges alltså rent bokstavligen. Det här är en identitet som används av en tjänst eller ett program. Det kan tilldelas roller som andra identiteter. 

Lamna Healthcare kan till exempel tilldela sina distributionsskript så att de körs autentiserade som ett huvudnamn för tjänsten. Om det är den enda identitet som har behörighet att utföra skadliga åtgärder gått Lamna långt för att se till att de inte har en upprepning av oavsiktliga resurs togs bort.

### <a name="managed-identities-for-azure-resources"></a>Hanterade identiteter för Azure-resurser

Skapa tjänstens huvudnamn kan vara en tidsödande process och det finns en massa kontaktpunkter som kan göra att underhålla dem. svårt. Hantera identiteter för Azure-resurser är mycket enklare och gör det mesta av arbetet åt dig.

En hanterad identitet kan skapas direkt för alla Azure-tjänster som stöder den (listan växer hela tiden). När du skapar en hanterad identitet för en tjänst, skapar du ett konto på Azure AD-klient. Azure-infrastrukturen tar automatiskt hand om autentiseringen av tjänsten och hanteringen av kontot. Du kan sedan använda det kontot på samma sätt som andra AD-konton, och på ett säkert sätt ge den autentiserade tjänsten åtkomst till andra Azure-resurser.

Lamna Healthcare sina Identitetshantering ett steg ytterligare och använder hanterade identiteter för alla tjänster som stöds som måste ha möjlighet att utföra hantering av infrastruktur och distributioner.

## <a name="infrastructure-protection-at-lamna-healthcare"></a>Skyddad infrastruktur på Lamna Healthcare

Vi har sett hur Lamna Healthcare har åtgärdat det tidigare misstaget då företagets infrastruktur oavsiktligt raderades. De har använt rollbaserad åtkomstkontroll för att bättre hantera säkerhet i sin infrastruktur och använder hanterade identiteter för att hålla sina autentiseringsuppgifter från kod och enkel administration av identiteter som behövs för sina tjänster.

## <a name="summary"></a>Sammanfattning

För att säkerställa infrastrukturens tillgänglighet och integritet är det viktigt att den skyddas på rätt sätt. Med funktioner som RBAC och hanterade identiteter hjälper att skydda din Azure-miljö från obehöriga eller oavsiktliga åtkomst och förbättra säkerhetsfunktioner identitet i din arkitektur.
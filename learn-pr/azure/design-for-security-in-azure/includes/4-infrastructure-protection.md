Lamna Healthcare hade nyligen ett allvarligt avbrott i en kundinriktad webbapp. En tekniker hade beviljats fullständig åtkomst till en resursgrupp som innehöll själva produktionswebbprogrammet. Teknikern råkade av misstag ta bort hela resursgruppen och alla underordnade resurser, inklusive databasen med alla kunddata. 

Lyckligtvis var programmets källkod och resurser tillgängliga i källkontrollen och regelbundna säkerhetskopieringar kördes automatiskt enligt ett schema. Tjänsten kunde därför återställas relativt enkelt. Här ska vi utforska hur det här avbrottet hade kunnat undvikas med hjälp av funktioner i Azure som skyddar åtkomsten till infrastrukturen.

## <a name="criticality-of-infrastructure"></a>Den viktiga infrastrukturen

Molninfrastrukturen blir en allt viktigare komponent på många företag. Det är viktigt att användare och processer endast beviljas de behörigheter som de behöver för att utföra sina jobb. Om fel åtkomst beviljas kan det resultera i dataförlust, dataläckage eller att tjänster blir otillgängliga. 

Systemadministratörer kan ansvara för ett stort antal användare, system och behörighetsuppsättningar. Därför kan det snabbt bli svårt att bevilja åtkomst på ett optimalt sätt, vilket ofta resulterar i en alltför generell standardtillämpning. Den här metoden kan visserligen förenkla administrationen, men gör det betydligt lättare att oavsiktligt bevilja mer omfattande behörigheter än vad som egentligen krävs.

## <a name="role-based-access-control"></a>Rollbaserad åtkomstkontroll

Rollbaserad åtkomstkontroll (RBAC) fungerar på ett lite annorlunda sätt. Roller definieras som samlingar med åtkomstbehörigheter. Säkerhetsobjekt mappas till roller direkt eller via gruppmedlemskap. Genom att säkerhetsobjekt, åtkomstbehörigheter och resurser hålls åtskilda blir det enklare att hantera och kontrollera åtkomsten.

I Azure lagras användare, grupper och roller i Azure Active Directory (AD Azure). Azure Resource Manager-API:et använder rollbaserad åtkomstkontroll för att skydda all åtkomsthantering för resurser i Azure.

![En bild som visar implementeringen av en rollbaserad åtkomstkontroll.](../media/ACL_Based_Access.png)

<!-- ![Role-based access control](../media/Role_Based_Access.png)
 -->

### <a name="roles-and-management-groups"></a>Roller och hanteringsgrupper

Roller är uppsättningar med behörigheter, t.ex. ”Skrivskyddad” eller ”Deltagare”, som användare kan beviljas för att få åtkomst till en Azure-tjänstinstans. Roller kan tilldelas på nivån för en enskild tjänstinstans eller spridas nedåt i Azure Resource Manager-hierarkin. Roller som har tilldelats i en högre omfattning, t.ex. en hel prenumeration, ärvs av underordnade områden, t.ex. tjänstinstanser. 

Hanteringsgrupper är en ytterligare hierarkisk nivå som nyligen introducerades i RBAC-modellen. Hanteringsgrupper gör det möjligt att gruppera prenumerationer och att tillämpa principen på en ännu högre nivå.

Möjligheten att sprida roller via en godtyckligt definierad prenumerationshierarki gör dessutom att administratörer kan bevilja tillfällig åtkomst till en hel miljö för autentiserade användare. En granskare kan exempelvis behöva tillfällig skrivskyddad åtkomst till alla prenumerationer.

![En illustration som visar den hierarkiska representationen av rollbaserad åtkomst i en hanteringsgrupp.](../media/management_groups.png)

### <a name="privileged-identity-management"></a>Privileged Identity Management

Förutom att hantera åtkomsten till Azure-resurser med RBAC, bör en heltäckande säkerhetsstrategi för infrastrukturen omfatta regelbunden granskning av rollmedlemmar allt eftersom organisationen förändras och utvecklas. Betaltjänsten Azure AD Privileged Identity Management (PIM) hjälper dig att övervaka rolltilldelningar, självbetjäning och JIT-rollaktivering (Just-In-Time) och innehåller även funktioner för att granska åtkomsten till Azure AD-resurser.

![Skärmbild av Privileged Identity Management-instrumentpanelen](../media/PIM_Dashboard.png)

## <a name="providing-identities-to-services"></a>Tilldela tjänster identiteter

Det är ofta bra att tilldela identiteter till tjänster. Ofta, och i strid med bästa praxis, bäddas autentiseringsuppgifter in i konfigurationsfiler. Om dessa konfigurationsfiler inte skyddas kan vem som helst med åtkomst till systemen eller lagringsplatserna komma åt dessa uppgifter eller öka risken för att de exponeras.

Azure AD löser det här problemet med hjälp av två metoder: tjänstens huvudnamn och hanterade identiteter för Azure-tjänster.

### <a name="service-principals"></a>Tjänstens huvudnamn

För att förstå vad tjänstens huvudnamn är, måste du först förstå vad orden **identitet** och **huvudnamn** betyder när de används inom identitetshantering.

En **identitet** är helt enkelt något som kan autentiseras. Exempel på detta är användare med användarnamn och lösenord, men det kan även vara program eller andra servrar, som kan autentiseras med hemliga nycklar eller certifikat. Det kan även vara bra att veta att ett **konto** är data som är associerade med en identitet.

Ett **huvudnamn** är en identitet som fungerar med vissa roller eller anspråk. Överväg att använda sudo i en bash-kommandotolk eller Windows med hjälp av kör som administratör. I båda dessa fall är du fortfarande inloggad som samma identitet som innan, men du har ändrat rollen som du kör under.

**Tjänstens huvudnamn** namnges alltså rent bokstavligen. Det här är en identitet som används av en tjänst eller ett program. Precis som andra identiteter kan den tilldelas roller. 

Lamna Healthcare kan till exempel tilldela sina distributionsskript så att de körs autentiserade som ett huvudnamn för tjänsten. Om det här är den enda identiteten som har behörighet att utföra skadliga åtgärder, är Lamna en bra bit på väg att förhindra att företagets data raderas av misstag igen.

### <a name="managed-identities-for-azure-resources"></a>Hanterade identiteter för Azure-resurser

Det kan vara tidskrävande att skapa tjänstens huvudnamn och det finns många kontaktpunkter, vilket ofta gör det svårt att underhålla dem. Hanterade identiteter för Azure-resurser är enklare och tar hand om merparten av arbetet åt dig.

En hanterad identitet kan skapas direkt för valfri Azure-tjänst som stöder det (listan växer hela tiden). När du skapar en hanterad identitet för en tjänst, skapar du ett konto i Azure AD-klientorganisationen. Azure-infrastrukturen tar automatiskt hand om autentiseringen av tjänsten och hanteringen av kontot. Du kan sedan använda det kontot på samma sätt som andra AD-konton, och på ett säkert sätt ge den autentiserade tjänsten åtkomst till andra Azure-resurser.

Lamna Healthcare tar identitetshanteringen ett steg längre och använder hanterade identiteter för alla kompatibla tjänster som måste kunna hantera och distribuera infrastruktur.

## <a name="infrastructure-protection-at-lamna-healthcare"></a>Skyddad infrastruktur på Lamna Healthcare

Vi har sett hur Lamna Healthcare har åtgärdat det tidigare misstaget då företagets infrastruktur oavsiktligt raderades. De använder rollbaserad åtkomstkontroll för att bättre hantera säkerheten i infrastrukturen och använder hanterade identiteter för att hålla autentiseringsuppgifterna åtskilda från koden och för att förenkla administrationen av de identiteter som behövs för deras tjänster.

## <a name="summary"></a>Sammanfattning

För att säkerställa infrastrukturens tillgänglighet och integritet är det viktigt att den skyddas på rätt sätt. Funktioner som RBAC och hanterade identiteter hjälper dig att skydda din Azure-miljö mot obehörig eller oavsiktlig åtkomst, samtidigt som de förbättrar säkerhetsfunktionerna relaterade till identiteter i din arkitektur.
Den vanligaste säkerhetsbrist program idag är inte korrekt bearbeta indata som tagits emot från externa källor, särskilt _indata från användaren_. Du bör alltid ta en titt några indata för att kontrollera att den har verifierats innan det används. Om du inte gör detta kan resultera i dataförlust eller exponering, eskalering av privilegier eller även körning av skadlig kod på andra användares datorer.

Tragic sak är att det här är ett enkelt problemet att lösa. Här beskriver vi hur du behandlar data. När den tas emot, när den visas på skärmen och när den är sparad för senare användning.

## <a name="why-do-we-need-to-validate-our-input"></a>Varför behöver vi verifiera vår indata?

Anta att du skapar ett gränssnitt för att tillåta en användare att skapa ett konto på din webbplats. Vår profildata inkluderar ett namn, e-post och ett smeknamn som vi visas för alla som besöker webbplatsen. Vad händer om en ny användare skapar en profil och anger ett smeknamn som innehåller vissa SQL-kommandon? Till exempel – vad händer om våra felaktiga användaren anger något som liknar:

```output
Eve'); DROP TABLE Users;--
```

Om vi bara blint infoga det här värdet i en databas, kan det potentiellt alter SQL-instruktionen för att köra kommandon som vi inte vill absolut kör! Detta kallas en ”SQL Injection”-attack och är en av de _många_ typer av kod som potentiellt kan göras när du inte hanterar korrekt indata. Så vad kan vi göra för att åtgärda detta? Den här enheten Lär dig när att verifiera indata, koda utdata och hur du skapar parametriserade frågor (som löser ovan utnyttja). Det här är de tre huvudsakliga defense teknikerna mot skadliga indata som anges i dina program.

## <a name="when-do-i-need-to-validate-input"></a>När behöver jag verifiera indata?

Svaret är _alltid_. Du måste verifiera **varje** in i ditt program. Detta inkluderar parametrar i Webbadressen indata från användare, data från databasen, data från ett API och något som skickas i klartext som en användare kan potentiellt manipulera. Använd alltid en lista över tillåtna metod, vilket innebär att du bara godkänna ”fungerande” indata, i stället för en Svartlistat (där du specifikt leta efter felaktiga indata) eftersom det är omöjligt att tänka på en fullständig lista över potentiellt skadliga indata.  Utför arbetet på servern, inte klientsidan (eller förutom klientsidan), så att dina försvar kan inte kringgås. Behandla **alla** data som inte är betrodd och du kan skydda dig mot de flesta av app vanliga säkerhetshot.

Om du använder ASP.NET, ramverket ger [bra stöd för att verifiera indata](https://docs.microsoft.com/aspnet/web-pages/overview/ui-layouts-and-themes/validating-user-input-in-aspnet-web-pages-sites) på både klienten och servern sida.

Om du använder en annan webbramverk, det finns vissa bra metoder för att utföra verifiering av indata på den [OWASP inkommande verifiering fusklapp](https://www.owasp.org/index.php/Input_Validation_Cheat_Sheet).


## <a name="always-use-parameterized-queries"></a>Använd alltid frågor med parametrar

SQL-databaser används ofta för att lagra data, till exempel profilinformation.  Skapa aldrig infogad SQL eller andra databasfrågor ”i farten” i din kod och skicka den direkt till databasen, det här är ett recept för haveriberedskap, som vi såg ovan.

Till exempel **inte gör detta** (kallas även infogade SQL):

```csharp
string userName = ... // receive input from the user BEWARE!
...
string query = "SELECT *  FROM  [dbo].[users] WHERE userName = '" + userName + "'";
```

Här sammanfoga vi textsträngar ihop för att skapa frågan, tar emot indata från användaren och genererar en dynamisk SQL-fråga för att leta upp användaren. Igen, om en obehörig användare insåg att vi har detta, eller bara _försökte_ olika indata format för att se om det uppstod ett problem, vi kan få en större katastrof. Använd i stället SQL-instruktioner eller lagrade procedurer som detta:

```sql
-- Lookup a user
CREATE PROCEDURE sp_findUser
(
@UserName varchar(50)
)

SELECT *  FROM  [dbo].[users] WHERE userName = @UserName
```

Med den här metoden du kan anropa proceduren från din kod på ett säkert sätt, skicka den `userName` sträng utan att behöva bekymra dig om den som behandlas som en del av SQL-instruktionen.

## <a name="always-encode-your-output"></a>Alltid koda dina utdata

Inga utdata du presentera visuellt eller i ett dokument ska alltid kodade och undantaget. Detta kan skydda dig om något inte uppfylldes i gemensamt passet eller koden genererar något som kan användas för vissa ändringar av misstag. Kontrollerar att allt är då _utdata_ och inte oavsiktligt tolkas som något som ska köras. Det här är en annan mycket vanligt angrepp metod som kallas ”Cross Site Scripting” (XSS).

Eftersom det är till exempel vanligt krav, är det här ett annat område där ASP.NET kommer att göra arbetet åt dig. Som standard [alla utdata redan är kodat](https://docs.microsoft.com/aspnet/core/security/cross-site-scripting?view=aspnetcore-2.1). Om du använder en annan webbramverk, kan du kontrollera dina alternativ för utdata encoding på webbplatser med den [OWASP XSS Dataförlustskydd fusklapp](https://www.owasp.org/index.php/XSS_(Cross_Site_Scripting)_Prevention_Cheat_Sheet).

## <a name="summary"></a>Sammanfattning

Är ett nödvändigt krav på att kontrollera dina indata är giltig och säkert att använda och lagra Santizing och verifiera dina indata. De flesta moderna webbramverk erbjuder inbyggda funktioner som kan automatisera vissa av arbetet. Du kan kontrollera din önskade framework-dokumentationen och se vilka funktioner erbjudanden. Webbprogram är den vanligaste plats där det här inträffar, Kom ihåg att andra typer av program som bara sårbara. Tror inte att du är säker det nya programmet är en skrivbordsapp. Du behöver fortfarande kunna hantera indata från användaren för att säkerställa att någon inte använda din app för att skada dina data eller skada företagets rykte.
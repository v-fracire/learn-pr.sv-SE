Den vanligaste säkerhetsbristen i program i dag är att inte korrekt bearbeta indata som tas emot från externa källor, särskilt _användarinmatning_. Du bör alltid kontrollera indata noggrant för att se till att de verifieras innan de används. Om du inte gör detta kan det leda till förlust eller exponering av data, eskalering av privilegier eller till och med körning av skadlig kod på andra användares datorer.

Det som är så onödigt med dessa problem att de enkelt kan lösas. Här går vi igenom hur du behandlar data: när de tas emot, när de visas på skärmen och när de är sparas för senare användning.

## <a name="why-do-we-need-to-validate-our-input"></a>Varför behöver vi verifiera våra indata?

Anta att du skapar ett gränssnitt för att användare ska kunna skapa ett konto på din webbplats. Våra profildata inkluderar ett namn, en e-postadress och ett smeknamn som vi visar för alla som besöker webbplatsen. Vad händer om en ny användare skapar en profil och anger ett smeknamn som innehåller vissa SQL-kommandon? Vad händer om våra illasinnade användare anger något i still med följande:

```sql
Eve'); DROP TABLE Users;--
```

Om vi utan vidare infogar det här värdet i en databas kan det potentiellt ändra SQL-instruktionen till att köra kommandon som vi absolut inte vill! Det här kallas en ”SQL-inmatningsattack” och är en av de _många_ typer av kryphål som potentiellt kan utföras när indata inte hanteras på rätt sätt. Vad kan vi då göra för att åtgärda detta? I den här enheten får du lära dig när indata ska valideras, hur utdata ska kodas samt hur du skapar parametriserade frågor (vilket löser kryphålet ovan!). Det här är de tre huvudsakliga försvarsteknikerna mot skadliga indata som försöker inmatas i dina program.

## <a name="when-do-i-need-to-validate-input"></a>När behöver jag verifiera indata?

Svaret är _alltid_. Du måste verifiera **alla** indata till ditt program. Detta inkluderar parametrar i URL:en, indata från användare, data från databasen, data från ett API och allt som skickas i klartext som en användare potentiellt kan manipulera. Använd alltid metoden med en vitlista, vilket innebär att du bara godkänner indata som är ”bekräftat goda” i stället för en svartlista (där du specifikt letar efter felaktiga indata) eftersom det är omöjligt att skapa en fullständig lista över potentiellt skadliga indata.  Utför det här arbetet på serversidan, inte på klientsidan (eller utöver klientsidan) så att dina försvar inte kan kringgås. Om du behandlar **ALLA** data som ej betrodda skyddar du mot de flesta vanliga säkerhetshoten för webbappar.

Om du använder ASP.NET ger ramverket [utmärkt stöd för att verifiera indata](https://docs.microsoft.com/aspnet/web-pages/overview/ui-layouts-and-themes/validating-user-input-in-aspnet-web-pages-sites) på både klient- och servernsidan.

Om du använder ett annat webbramverk finns det vissa bra metoder för att utföra verifiering av indata tillgängliga på [OWASP:s översiktsblad om verifiering av indata](https://www.owasp.org/index.php/Input_Validation_Cheat_Sheet).


## <a name="always-use-parameterized-queries"></a>Använd alltid parametriserade frågor

SQL-databaser används ofta för att lagra data, till exempel profilinformation.  Skapa aldrig infogat SQL eller andra databasfrågor ”i farten” i din kod och skicka den direkt till databasen – som vi såg ovan kan resultatet bli katastrofalt.

Till exempel ska du **inte göra detta** (kallas även infogat SQL):

```csharp
string userName = ... // receive input from the user BEWARE!
...
string query = "SELECT *  FROM  [dbo].[users] WHERE userName = '" + userName + "'";
```

Här sammanfogar vi textsträngar för att skapa frågan. Vi använder indata från användaren och genererar en dynamisk SQL-fråga för att leta upp användaren. Om en illasinnad användare skulle inse att vi gör detta eller helt enkelt _prova_ olika indataformat för att se om det finns en sårbarhet kan det leda till en stor katastrof. Använd istället parametriserade SQL-instruktioner eller lagrade procedurer som den här:

```sql
-- Lookup a user
CREATE PROCEDURE sp_findUser
(
@UserName varchar(50)
)

SELECT *  FROM  [dbo].[users] WHERE userName = @UserName
```

Med den här metoden kan du anropa proceduren från din kod på ett säkert sätt och skicka strängen `userName` utan att behöva bekymra dig om den behandlas som en del av SQL-instruktionen.

## <a name="always-encode-your-output"></a>Koda alltid dina utdata

Alla utdata som du presenterar antingen visuellt eller inom ett dokument ska alltid kodas och undantas. Detta kan skydda dig om något förbises i saneringsomgången eller om koden av misstag genererar något som kan användas i skadligt syfte. På så sätt ser du till att allt visas som _utdata_ och inte oavsiktligt tolkas som något som ska köras. Det här är en annan mycket vanlig angreppsmetod som kallas ”skriptkörning över flera webbplatser” (XSS, Cross-Site Scripting).

Eftersom det här är ett vanligt krav utgör det här ytterligare ett område där ASP.NET gör arbetet åt dig. Som standard är alla utdata redan kodade. Om du använder ett annat webbramverk kan du kontrollera dina alternativ för utdatakodning på webbplatser med [OWASP:s översiktsblad om XSS-skydd](https://www.owasp.org/index.php/XSS_(Cross_Site_Scripting)_Prevention_Cheat_Sheet).

## <a name="summary"></a>Sammanfattning

Att sanera och verifiera dina indata är ett nödvändigt krav för att se till att dina indata är giltiga och säkra att använda och lagra. De flesta moderna webbramverk erbjuder inbyggda funktioner som kan automatisera en del av arbetet. Du kan kontrollera din önskade framework-dokumentation och se vilka funktioner som erbjuds. Även om webbprogram är den vanligaste plats där det här inträffar så bör du tänka på att andra typer av program kan vara precis lika sårbara. Tro inte att du är säker för att ditt nya program är en skrivbordsapp. Du behöver fortfarande kunna hantera indata från användare för att säkerställa att ingen använder din app för att skada dina data eller företagets rykte.
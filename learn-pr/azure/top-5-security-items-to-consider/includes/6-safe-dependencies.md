En stor del av koden som finns i moderna program är de bibliotek och beroenden som du har valt: utvecklaren. Det här är en vanlig metod som sparar tid och pengar. Nackdelen är dock att du är ansvarig för den här koden även om andra skrev det, eftersom du har använt den i projektet. Om en forskare (eller ännu värre hackare) identifierar en sårbarhet i ett av dessa bibliotek med 3 part samma fel troligen kommer också finnas i din app.

Med hjälp av komponenter med kända problem är stora problem i vår bransch. Det är så problematiska som har gjort den [OWASP topp tio](https://www.owasp.org/index.php/Category:OWASP_Top_Ten_Project) sämsta web sårbarheter i program, hålls vid #9 i flera år.

## <a name="track-known-security-vulnerabilities"></a>Spåra kända säkerhetsproblem

Vi har problemet är att veta när ett problem har identifierats. Att hålla våra bibliotek och beroenden kan naturligtvis uppdaterade (#4 i vår lista!), men det är en bra idé att hålla reda på identifierade säkerhetsrisker som kan påverka ditt program.

> [!IMPORTANT]
> När ett system har en känd svaghet, är det mycket mer sannolikt också kryphål tillgänglig, kod som kan användas för att attackera dessa system. Om en exploatering offentliggörs, är det viktigt att de aktuella systemen uppdateras direkt.

**Mitre** är en ideell organisation som underhåller den [Common Vulnerabilities and Exposures lista](https://cve.mitre.org). Den här listan är en offentligt sökbara uppsättning kända cybersäkerhet säkerhetsproblem i appar, bibliotek och ramverk. **Om du hittar ett bibliotek eller en komponent i CVE databasen har det kända säkerhetsrisker**.

Problem med skickas av säkerhets-communityn när ett security fel hittas i en produkt eller komponent. Varje publicerad problemet tilldelas ett ID och innehåller datum som identifierats och en beskrivning av säkerhetsproblem, och referenser till publicerade lösningar eller leverantörens instruktioner om problemet.

### <a name="how-to-verify-if-you-have-known-vulnerabilities-in-your-3rd-party-components"></a>Så här verifierar du om du har kända säkerhetsrisker i dina 3 part-komponenter

Du kan placera en daglig aktivitet i din telefon och gå och kontrollera den här listan, men som tur är för oss, många verktyg finns om du vill att vi kan kontrollera om våra beroenden är sårbara. Du kan köra dessa verktyg mot din kodbas eller bättre än, lägga till dem i dina CI/CD-pipeline för att automatiskt söka efter problem som en del av utvecklingsprocessen.

- [Kontrollera om OWASP beroende](https://www.owasp.org/index.php/OWASP_Dependency_Check), som har en [plugin-programmet Jenkins](https://wiki.jenkins.io/display/JENKINS/OWASP+Dependency-Check+Plugin)
- [OWASP SonarQube](https://www.owasp.org/index.php/OWASP_SonarQube_Project)
- [Synk](https://snyk.io), som är kostnadsfritt för lagringsplatser med öppen källkod i GitHub
- [Svart ankor](https://www.blackducksoftware.com) som används av många företag
- [RubySec](https://rubysec.com) en rådgivande databas bara för Ruby
- [Retire.js](https://github.com/retirejs/retire.js/) ett verktyg för att verifiera om JavaScript-bibliotek är inaktuella; kan användas som ett plugin-program för olika verktyg, bland annat [Burp Suite](https://www.portswigger.net)

Vissa verktyg som är särskilt utformade för statiska kodanalys kan användas för det här också.

- [Roslyn Kontrollidentifiering](https://dotnet-security-guard.github.io)
- [Puma genomsökning](https://pumascan.com)
- [PT Application Inspector](https://www.ptsecurity.com/ww-en/products/ai/)
- [Apache Maven Dependency Plugin](http://maven.apache.org/plugins/maven-dependency-plugin/)
- [Käll-rensning](https://www.sourceclear.com)
- [Sonatype](https://ossindex.sonatype.org)
- [Plattform för nod-säkerhet](https://nodesecurity.io)
- [WhiteSource](https://www.whitesourcesoftware.com/what-is-whitesource/)
- [Hdiv](https://hdivsecurity.com)
- [Och många fler...](https://www.owasp.org/index.php/Source_Code_Analysis_Tools)

Mer information om riskerna som använder sårbara komponenter finns på [OWASP sidan](https://www.owasp.org/index.php/Top_10-2017_A9-Using_Components_with_Known_Vulnerabilities) specifikt för det här avsnittet.

## <a name="summary"></a>Sammanfattning

När du använder bibliotek eller andra 3 part-komponenter som en del av ditt program som du också ta med på eventuella risker kan ha. Det bästa sättet att minska denna risk är att se till att du endast använder komponenter som har inga kända säkerhetsrisker kopplade.

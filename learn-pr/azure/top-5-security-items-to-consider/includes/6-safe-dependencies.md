En stor del av koden som finns i moderna program är de bibliotek och beroenden som du som utvecklare har valt. Det här är en vanlig metod som sparar tid och pengar. Nackdelen är dock att du är ansvarig för den här koden även om andra skrev den, eftersom du har använt den i ditt projekt. Om en forskare (eller ännu värre, en hackare) upptäcker en säkerhetsrisk i något av dessa bibliotek från tredje part, kommer samma fel troligen också finnas i din app.

Att komponenter med kända säkerhetsrisker används är ett stort problem i vår bransch. Det är så problematiskt att det har hamnat på [OWASP:s topp 10-lista](https://www.owasp.org/index.php/Category:OWASP_Top_Ten_Project) över de värsta säkerhetsriskerna i webbprogram, där det har legat på plats 9 i flera år.

## <a name="track-known-security-vulnerabilities"></a>Hitta kända säkerhetsrisker

Problemet är att veta när ett problem har identifierats. Att hålla våra bibliotek och beroenden uppdaterade (nr 4 i vår lista!) hjälper förstås, men det är också en bra idé att hålla reda på identifierade säkerhetsrisker som kan påverka ditt program.

> [!IMPORTANT]
> När ett system har en känd säkerhetsrisk är det mycket mer sannolikt att det också finns kryphål, dvs. kod som kan användas för att attackera dessa system. Om ett kryphål offentliggörs är det viktigt att de berörda systemen uppdateras omedelbart.

**Mitre** är en ideell organisation som sammanställer [listan Common Vulnerabilities and Exposures](https://cve.mitre.org). Den här listan är en offentligt sökbar uppsättning av kända cybersäkerhetsproblem i appar, bibliotek och ramverk. **Om du hittar ett bibliotek eller en komponent i CVE-databasen har det kända säkerhetsrisker**.

Problem skickas in av säkerhetscommunityn när ett säkerhetsfel hittas i en produkt eller komponent. Varje publicerat problem tilldelas ett ID och visar det datum då det upptäcktes, en beskrivning av säkerhetsrisken och referenser till publicerade lösningar eller leverantörens anvisningar för problemet.

### <a name="how-to-verify-if-you-have-known-vulnerabilities-in-your-3rd-party-components"></a>Så här kontrollerar du om du har kända säkerhetsrisker i dina komponenter från tredje part

Du kan ha ett larm i din telefon om att kontrollera den här listan, men som tur är för oss finns det många verktyg som i stället hjälper oss att se om våra beroenden är sårbara. Du kan köra verktygen mot din kodbas eller (vilket är ännu bättre) lägga till dem i din CI/CD-rörledning för att automatiskt söka efter problem som en del av utvecklingsprocessen.

- [OWASP Dependency Check](https://www.owasp.org/index.php/OWASP_Dependency_Check) med ett [plugin-program för Jenkins](https://wiki.jenkins.io/display/JENKINS/OWASP+Dependency-Check+Plugin)
- [OWASP SonarQube](https://www.owasp.org/index.php/OWASP_SonarQube_Project)
- [Synk](https://snyk.io) som är kostnadsfritt för lagringsplatser med öppen källkod i GitHub
- [Black Duck](https://www.blackducksoftware.com) som används av många företag
- [RubySec](https://rubysec.com) som är en rådgivande databas bara för Ruby
- [Retire.js](https://github.com/retirejs/retire.js/) som är ett verktyg för att verifiera om JavaScript-bibliotek är inaktuella. Det kan användas som ett plugin-program för olika verktyg, bland annat [Burp Suite](https://www.portswigger.net)

Vissa verktyg som är särskilt utformade för analys av statisk kod kan också användas för det här.

- [Roslyn Security Guard](https://dotnet-security-guard.github.io)
- [Puma Scan](https://pumascan.com)
- [PT Application Inspector](https://www.ptsecurity.com/ww-en/products/ai/)
- [Apache Maven Dependency Plugin](http://maven.apache.org/plugins/maven-dependency-plugin/)
- [Source Clear](https://www.sourceclear.com)
- [Sonatype](https://ossindex.sonatype.org)
- [Node Security Platform](https://nodesecurity.io)
- [WhiteSource](https://www.whitesourcesoftware.com/what-is-whitesource/)
- [Hdiv](https://hdivsecurity.com)
- [Och mycket mer!](https://www.owasp.org/index.php/Source_Code_Analysis_Tools)

Mer information om riskerna med användningen av sårbara komponenter finns på [OWASP-sidan](https://www.owasp.org/index.php/Top_10-2017_A9-Using_Components_with_Known_Vulnerabilities) specifikt för det här ämnet.

## <a name="summary"></a>Sammanfattning

När du använder bibliotek eller andra komponenter från tredje part som en del av ditt program, får du också de eventuella risker de kan innehålla. Det bästa sättet att minska denna risk är att se till att du endast använder komponenter utan några kända säkerhetsrisker.

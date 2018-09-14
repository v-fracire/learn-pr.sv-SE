Anta att du får en avisering från ditt företags säkerhetsadministratör att en potentiell säkerhetsrisk har identifierats i nätverket. För att skydda dina database-servrar, som du vill lägga till granskning och övervakning.

I den här enheten, kan vi titta på hur granskning konfigureras mot en databas och hur du använder dessa granskningar.

## <a name="configure-auditing"></a>Konfigurera granskning

Aktiverar du granskning att lagra de åtgärder som inträffar på databasen för senare granskning eller har automatiserade verktyg analysera dem. Granskning används också för hantering av regelefterlevnad och förstå hur databasen används. Granskning krävs också om du vill använda Azure hotidentifiering i Azure SQL database.

Om du vill lagra granskningar av databasen, krävs ett Azure storage-konto för att lagra granskningshistoriken.

Nu ska vi titta på de steg du utför för att konfigurera granskning på datorn.

1. Välj Azure SQL-Server i portalen.

2. Navigera till objektet granskning i de vänstra konfigurationsalternativ. Du hittar den i säkerhetskategorin.

3. Granskning är inaktiverat som standard. Om du vill aktivera det på din databasserver, trycker du på knappen vidare.

4. När knappen på är markerad, klicka på Konfigurera definiera storage-konto. Du kan välja ett befintligt lagringskonto eller skapa ett nytt lagringskonto för att lagra dina granskningar. Lagringskontot måste konfigureras för att använda samma region som din server.

5. Klicka på knappen ”Spara” i verktygsfältet för att spara dina ändringar.

De här åtgärderna konfigurera revisioner på databasnivå för servern. Du kan också konfigurera granskning ska inträffa vid en databasnivå.

Nu får du konfigurera Avancerat skydd.

## <a name="configure-advanced-threat-protection"></a>Konfigurera Avancerat skydd

Avancerat skydd analyseras granskningsloggar för att söka efter potentiella problem och hot mot din databas.

Nu ska vi titta på de steg du utför för att konfigurera Avancerat skydd på datorn.

1. Välj Azure SQL-Server i portalen.

2. Navigera till avancerat skydd-objekt i de vänstra konfigurationsalternativ. Du hittar den i säkerhetskategorin.

3. Klicka på knappen ”Aktivera Avancerat skydd på servern”.

4. Ändra Advanced Threat Protection-växel till ON.

5. Välj vyn Avancerat skydd-serverinställningar Visa alternativ för databassystem.

6. Definiera där avgränsade meddelandet som e-postmeddelanden levereras som en lista över semikolonavgränsade e-postadresser. Välj e-posttjänst och medadministratörer att skicka hot för tjänstadministratörer.

7. Nu ska du ange den prenumerationen och storage-konto som ska analyseras för några hot i systemet. Detta bör vara den prenumerationen och Azure storage-konto som konfigurerades för granskning. Du måste också ange antalet dagar att behålla granskningshistoriken. Ställer in värdet på noll innebär det att granskningen kommer att lagras alltid.

Välj sedan lagringsåtkomstnyckel för att ansluta till revisioner. Tryck på OK när du har konfigurerat alternativen.

Slutligen ange vilka typer av identifiering av hot. Önskat Kontaktalternativ är allt.

Alla representerar följande värden:

- SQL-inmatning rapporter där SQL-inmatningar-angrepp har inträffat;
- SQL-inmatning säkerhetsproblem rapporter där sannolikheten för en SQL-inmatning är sannolikt; och
- Avvikande klientinloggning tittar på inloggningar som oregelbunden och kan orsaka problem, till exempel en potentiella angripare får åtkomst.

Klicka på den **spara** knappen för att tillämpa ändringarna.

Du får e-postmeddelanden som sårbarheter upptäcks. E-postmeddelandet beskriver vad som hänt och åtgärder som kan vidtas.

## <a name="enable-advanced-threat-protection"></a>Aktivera Avancerat skydd

När du har konfigurerat servern för avancerat skydd kan aktivera du alternativet för varje enskild databas. Navigera till de enskilda databaserna och aktivera Avancerat skydd genom att välja ”Aktivera Avancerat skydd på servern”.

Du kan aktivera periodiskt återkommande sökningar som genomsöker systemet var sjunde dag för att söka efter säkerhetsrisker.

När du väljer alternativet periodiskt återkommande genomsökning körs en genomsökning omedelbart när du har sparat inställningarna.

Klicka på knappen **Spara** för att spara ändringarna.

Du får ett e-postmeddelande som meddelar dig om eventuella säkerhetsproblem. Se till att åtgärda hotet omedelbart. Du kan få ett meddelande för en rad orsaker:

![En exempel-meddelande varning från Avancerat skydd](../media-draft/5-email-with-warning.png)

Välj Avancerat skydd om Advanced Threat Protection är igång, visas en lista över problem som visas. Den här listan kan innehålla Dataidentifiering och klassificering problem, till exempel känsliga data, en lista över sårbarheter på system och potentiella hot.

![Dataidentifiering och klassificering](../media-draft/5-data-discovery-and-classification.png)

Panelen Dataidentifiering och klassificering visar kolumner i dina tabeller som behöver skyddas. Några av kolumnerna som kan ha känslig information eller kan betraktas som klassificeras i olika länder eller regioner.

Klicka på panelen Dataidentifiering och klassificering.

Ett meddelande visas om några kolumner behöver skydd som har konfigurerats. Det här meddelandet skickas i form av *”vi har hittat 10 kolumner med klassificeringsrekommendationer”*. Du kan klicka på texten som ska visa rekommendationer.

Markera de kolumner som du vill klassificera genom att klicka på bockmarkeringen bredvid kolumnen eller markera kryssrutan till vänster om schema-huvudet. Välja Godkänn valda rekommendationer att använder rekommendationerna för klassificering.

Du redigera kolumner och definiera informationstypen och känslighetsetikett för databasen. Klicka på knappen Spara för att spara ändringarna.

Inga aktiva rekommendationer bör visas när du har lyckats rekommendationerna har.

![Vulnerability Assessment instrumentpanel](../media-draft/5-vulnerability-assessment-dashboard.png)

Utvärdering av säkerhetsrisker listar konfigurationsproblem på din databas och den associera risken. Till exempel i bilden ovan ser du brandväggen på servernivå måste ställas in.

Klicka på panelen Vulnerability Assessment att granska en fullständig lista över sårbarheter. Härifrån kan ska du klicka på varje enskild säkerhetsproblem.

På sidan visas information, till exempel risknivå, vilken databas som gäller för en beskrivning av problemet och de rekommenderade åtgärderna för att åtgärda problemet. Du ska använda åtgärder för att åtgärda problemet eller problem. Se till att åtgärda alla sårbarheter.

![Identifiering av hot](../media-draft/5-threat-detection-dashboard.png)

Senaste diagrammet visar en lista över threat identifieringar. Till exempel i den här listan visas ett antal potentiell SQL-inmatningsattacker.

Klicka på panelen identifiering av hot för att gå till listan över poster att se det hotet som sårbarheter. Sedan åtgärda problemet genom att följa rekommendationerna.  För problem med till exempel SQL-inmatning varningar kommer du att kunna titta på frågan och arbetar bakåt för att där frågan körs i kod. Förr, du måste skriva om koden så att den inte längre har problemet.

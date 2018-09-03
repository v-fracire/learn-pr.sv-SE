Om du vill att förstå vad som kan gå fel med hanteringen av ett programs konfigurationshemligheter kan du läsa historien om utvecklaren Steve.

Steve hade arbetat på ett företag som levererade foder för sällskapsdjur i några veckor. Han läste information om företagets webbapp, &mdash;, ett .NET Core-webbprogram som använde en Azure SQL-databas för att lagra beställningsinformation och tredjeparts-API:er för kreditkortsfakturering och för att lagra kundadresser &mdash; när han av misstag klistrade in anslutningssträngen till beställningsdatabasen i ett öppet forum.

Några dagar senare upptäckte ekonomiavdelningen att företaget levererade mycket foder som ingen hade betalat för. Någon hade använt anslutningssträngen för att få åtkomst till databasen, bakåtkompilerat schemat och skickat beställningar utan att gå via webbplatsen.

När Steve upptäckte sitt misstag skyndade han att byta databaslösenordet för att låsa ute angriparen, vilket gjorde att webbplatsen kraschade. Han loggade direkt in programservern och ändrade appkonfigurationen i stället för att omdistribuera, men servern visade fortfarande misslyckade begäranden.

Steve hade glömt att flera instanser av appen kördes på olika servrar. Han hade bara ändrat konfigurationen för en. En fullständig omdistribution behövdes, vilket orsakade ett avbrott på 30 minuter.

Att läcka en databasanslutningssträng, API-nyckel eller tjänstlösenord kan resultera i en katastrof. Stulna eller borttagna data, finansiell skada, stilleståndstid och oåterkallelig skada för företagets tillgångar och rykte är alla möjliga resultat. Tyvärr behöver hemliga värden ofta distribueras på flera platser samtidigt och ändras ibland. Och *någonstans* måste du lagra dem! Nu ska vi se hur vi kan göra allt detta med Azure KeyVault.

## <a name="learning-objectives"></a>Utbildningsmål
I den här modulen kommer du att göra följande:

- Förstå vilka typer av information som kan lagras i Azure Key Vault.
- Skapa ett Azure Key Vault-lager.
- Autentisera med Azure Key Vault.
- Få tillgång till hemligheter i ett Azure Key Vault.
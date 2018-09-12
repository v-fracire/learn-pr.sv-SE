Moderna program består ofta av flera delar som körs på separata datorer och enheter som finns på platser över hela världen. Komplexa nätverk med olika tillförlitlighet och hastighet ligger ofta mellan dessa komponenter. En grundläggande utmaning med dessa distribuerade program är att kommunicera på ett tillförlitligt sätt mellan komponenterna.

Anta att du är en molnutvecklare för Contoso sektorer, en global pizzakedja. Din arbetsgivare håller på att uppgradera sin teknik så att användarna kan beställa från webben eller sina mobilappar. Beställningarna skickas till användarnas favoritrestaurang där pizzan lagas. Efter hand som degen rullas, pizzan läggs in ugnen, förpackas och levereras skickas uppdateringar till användarens mobilapp. Användaren får till och med platsuppdateringar när föraren kommer närmare. 

Contoso Slice har tidigare skapat ett onlinesystem för att beställa pizza som lagrar beställningsdata i en SQL Server-databas. Varje butik var tvungen att komma ihåg att uppdatera ”webbeställningarna” manuellt för att se om de hade nya beställningar. Dessutom, när efterfrågan på pizza var stor, till exempel under sportevenemang, råkade systemet ofta ut för systemavbrott och fel. Slutligen saknade systemet ett centralt betalningssystem och stöd för statusuppdateringar för användaren.

För det här nya, mer ambitiösa projektet anlitade Contoso en molnarkitekt och planerar att använda en fristående arkitektur. 

I den här modulen får vi lära dig hur Azure Service Bus kan hjälpa dig att skapa ett program som är tillförlitligt under belastning. Vi ska också se hur Azure Service Bus gör det enkelt att lägga till funktioner i våra program. Under resans gång kommer vi att använda C# för att omsätta lektionerna i praktiken. Här får du se hur du kan använda ämnen och köer för Azure Service Bus i en distribuerad arkitektur för att säkerställa tillförlitlig kommunikation vid hög efterfrågan. Du får också skriva C#-kod som kommunicerar via Service Bus.

## <a name="learning-objectives"></a>Utbildningsmål

I den här modulen får du:
- Välja om du vill använda Service Bus-köer, -ämnen eller -reläer för att kommunicera i ett distribuerat program
- Konfigurera ett Azure Service Bus-namnområde i en Azure-prenumeration
- Skapa ett Service Bus **-ämne** och använd det för att skicka och ta emot meddelanden
- Skapa en Service Bus **-kö** och använd den för att skicka och ta emot meddelanden

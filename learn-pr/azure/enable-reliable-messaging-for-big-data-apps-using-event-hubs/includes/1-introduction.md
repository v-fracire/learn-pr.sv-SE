Stordataprogram bör ha möjlighet att bearbeta ökat genomflöde genom att skala ut för att möta ökade transaktionsvolymer.

Anta att du jobbar med kreditkort på en bank. Du är en del av ett team som hanterar systemet som ansvarar för bedrägeritestning för att avgöra om varje transaktion ska godkännas eller nekas. Systemet tar emot en dataström med transaktioner och måste bearbeta dem i realtid.

Belastningen på systemet är som högst under helger och helgdagar. Det ska kunna hantera ökat genomflöde effektivt och korrekt. Transaktionerna är känsliga, så även det minsta felet kan ha stor inverkan.

Azure Event Hubs kan ta emot och bearbeta en stor mängd transaktioner. Det kan också konfigureras att skala dynamiskt när det behövs, för att hantera ökat genomflöde.
I den här modulen lär du dig att ansluta händelsehubbar till din app och bearbeta stora transaktionsvolymer med hög tillförlitlighet.

## <a name="learning-objectives"></a>Utbildningsmål

I den här modulen kommer du att:

- Skapa en händelsehubb med hjälp av Azure CLI.
- Konfigurera appar för att skicka eller ta emot meddelanden via en händelsehubb.
- Utvärdera händelsehubbens prestanda med hjälp av Azure Portal.
Anta att du skapar ett mobilt program för snabbmeddelanden. Programmet tillåter användare att skicka meddelanden till alla medlemmar i en viss användardefinierad grupp. Det finns vissa data som behöver lagras om varje användare som sitt användarnamn, e-post och lösenord. Så väljer du att använda SQL Server för att skapa en relationsdatabas för den här informationen. Men själva meddelandena måste skickas och nås snabbt och en relationsdatabas är för långsamma för detta.

Du vill skapa en Azure Redis Cache på grund av hur många fördelar. Exempelvis kan du använder transaktioner för att se till att ett meddelande med en bild och texten skickas tillsammans. Du använder data upphör att gälla kan återställa namnet på gruppen chatten efter en timme. Slutligen ska du använda avlägsningsprinciper för att kontrollera att de äldsta meddelandena tas bort först när du slut på minne.

## <a name="learning-objectives"></a>Utbildningsmål

I den här modulen kommer du att göra följande:

- Gruppera flera åtgärder i en transaktion
- Ställer in en förfallotid på dina data
- Hantera minnet är slut villkor
- Använda cache-aside-mönstret

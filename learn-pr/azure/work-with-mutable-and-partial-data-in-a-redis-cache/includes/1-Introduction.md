Anta att du skapar ett mobilt program för snabbmeddelanden. Programmet tillåter användare att skicka meddelanden till alla medlemmar i en viss användardefinierad grupp. Det finns vissa data som behöver lagras om varje användare som användarnamn, e-post och lösenord. Så du väljer att använda SQL Server för att skapa en relationsdatabas för den här informationen. Men själva meddelandena måste skickas och nås snabbt och en relationsdatabas är för långsam för detta.

Du bestämmer dig för att skapa en Azure Redis Cache på grund av alla de fördelar detta innebär. Kom ihåg från modulen **optimera dina webbprogram genom att cachelagra skrivskyddade data med Redis** att en Redis Cache är ett minnesinternt datastrukturlager som kan användas som en databas, cache eller koordinator.

Med Azure Redis Cache skulle du kunna använda transaktioner för att se till att ett meddelande med en bild och text skickas tillsammans. Använd dataförfallotider för att återställa namnet på gruppchatten efter en timme. Slutligen använder du borttagningsprinciper för att kontrollera att de äldsta meddelandena tas bort först när det är slut på minne.

## <a name="learning-objectives"></a>Utbildningsmål

I den här modulen kommer du att göra följande:

- Gruppera flera åtgärder i en transaktion
- Ställa in en förfallotid för dina data
- Hantera förhållanden där minnet tar slut
- Använd cache-aside-mönstret
- Använda ServiceStack.Redis-paketet i ett .NET Core-konsolprogram
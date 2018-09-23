När du skapar ett program vill du ge en bättre användarupplevelse och en del av detta är snabb datahämtning. Att hämta data från en databas tar vanligtvis lång tid och om dessa data läses ofta kan detta göra att användarupplevelsen försämras. Cache-aside-mönstret beskriver hur du kan implementera ett cacheminne tillsammans med en databas för att returnera de data som oftast används så snabbt som möjligt.

Här kan lära du dig hur cache-aside-mönstret kan användas för att säkerställa att du snabbt kan komma åt dina viktiga data.

## <a name="what-is-the-cache-aside-pattern"></a>Vad är ett cache aside-mönster?

Cache-aside-mönstret dikterar att när man behöver hämta data från en datakälla, exempelvis en relationsdatabas, bör man först kontrollera om det finns data i det egna cacheminnet. Använd de eventuella data som finns i cacheminnet. Om data inte finns i ditt cacheminne frågar du databasen och när du returnerar data tillbaka till användaren lägger du till dem i ditt cacheminne. Detta gör sedan att du kan komma åt data från ditt cacheminne nästa gång de behövs.

## <a name="when-to-implement-the-cache-aside-pattern"></a>När ska du implementera cache-aside-mönstret?

Läsning av data från en databas tar vanligtvis lång tid. Det omfattar kompilering av en komplex fråga, förberedelse av en plan för frågekörning, körning av frågan och sedan förberedelse av resultatet. I vissa fall kan den här processen även läsa data från disken. Det finns optimeringar som kan göras på databasnivå, exempelvis att sammanställa frågorna i förväg, och läsa in en del data i minnet. Körningen av frågan och förberedelsen av resultatet inträffar emellertid alltid när data hämtas från en databas.

Vi kan lösa problemet med hjälp av cache-aside-mönstret. Vi har fortfarande programmet och databasen i cache-aside-mönstret, men nu har vi även en cache. En cache lagrar data i minnet så att den inte behöver interagera med filsystemet. Cacheminnen lagrar också data i mycket enkla datastrukturer, som nyckelvärdespar, så att de inte behöver köra komplexa frågor för att samla in data eller underhålla index när du skriver data. Ett cacheminne är därför vanligtvis bättre än en databas. När du använder ett program görs ett försök att läsa data från cachen först. Om begärda data inte finns i cacheminnet hämtar programmet den från databasen, som den alltid har gjort. Därefter lagras dock data i cacheminnet för efterföljande begäranden. Nästa gång en användare begär data returneras de från cachen direkt.

![Diagram för att läsa in data som ska cachelagras](../media/8-cache-aside-set-cache.png)

### <a name="how-to-manage-updating-data"></a>Så här hanterar du datauppdateringar

När du implementerar mönstret cache-aside uppstår ett litet problem. Eftersom dina data nu lagras nu i ett cacheminne och ett datalager kan du stöta på problem när du försöker göra en uppdatering. Om du vill uppdatera dina data, skulle du exempelvis behöva uppdatera både cacheminnet och datalagret.

Lösning på problemet i cache-aside-mönstret är att ogiltigförklara data i cacheminnet. När du uppdaterar data i ditt program bör du först ta bort data i cacheminnet och sedan göra ändringarna i datakällan direkt. Om du gör det kommer den inte att finnas i cacheminnet nästa gång data begärs, och processen upprepar sig. 

![Diagram över att ogiltigförklara cachelagrade data](../media/8-cache-aside-invalidate.png)

## <a name="considerations-for-using-the-cache-aside-pattern"></a>Att tänka på när cache-aside-mönstret används

Överväg noga vilka data som vi ska placera i cacheminnet. Alla data lämpar sig inte för cachelagring.

- **Omfång**: För att cache-aside ska vara effektivt måste du kontrollera att förfallopolicyn matchar åtkomstfrekvensen för data. Gör inte förfalloperioden för kort, då det kan leda till att programmen hämtar data regelbundet från datalagret och lägger till dem i cachen.

- **Borttagning**: Cacheminnen har en begränsad storlek jämfört med typiska datalager och de tar bort data när det behövs. Se till att välja en lämplig borttagningsprincip för dina data.

- **Inställningar**: I syfte att göra cache aside-mönstret effektivt kommer många lösningar att fylla i cachen med data som bedöms kommer till användning ofta.

- **Konsekvens**: Konsekvens mellan datalagret och cachen garanteras inte av att cache-aside-mönstret implementeras. Data i ett datalager kan ändras utan att cachen meddelas. Detta kan leda till allvarliga synkroniseringsproblem.

Cache-aside-mönstret är användbart när man måste komma åt data ofta från en datakälla som använder en disk. Med hjälp av cache-aside-mönstret kommer du att lagra viktiga data i ett cacheminne för att öka hämtningshastigheten. 
När du skapar ett program som du vill ge en bättre användarupplevelse och en del av som är snabb datahämtning. Hämta data från en databas är vanligtvis lång tid och om dessa data läses ofta, kan detta ge en dålig användarupplevelse. Cache-aside-mönstret beskriver hur du kan implementera ett cacheminne tillsammans med en-databas att returnera de ofta använda data så snabbt som möjligt.

Här kan lära du dig hur cache-aside-mönstret kan användas för att kontrollera dina viktiga data kan snabbt komma åt.

## <a name="what-is-the-cache-aside-pattern"></a>Vad är cache-aside-mönstret?

Cache-aside-mönstret avgör att när du behöver hämta data från en datakälla som en relationsdatabas, bör du först kontrollera för data i cacheminnet. Om data finns i cacheminnet, använda den. Om data är inte i din cache och sedan fråga databasen och när du returnerar data tillbaka till användaren kan du lägga till den till ditt cacheminne. Detta kommer sedan att du kan komma åt data från ditt cacheminne nästa gång det behövs.

## <a name="when-to-implement-the-cache-aside-pattern"></a>När du ska implementera mönstret cache-aside?

Läsning av data från en databas är vanligtvis lång tid. Det innebär att kompileringen av en komplex fråga, förbereda en plan för körning av fråga, körning av frågan och förberedelsen av resultatet. I vissa fall kan kan den här processen läsa data från disken samt. Det finns optimeringar som kan göras på databasnivå som förväg kompilera frågorna och inläsning av data i minnet. Körningen av frågan och förberedelsen av resultatet inträffar alltid emellertid att hämta data från en databas.

Vi kan lösa problemet med hjälp av cache-aside-mönstret. Vi har fortfarande programmet och databasen i cache-aside-mönstret, men nu ger vi även team en cache. En cache lagras data i minnet, så att den inte behöver interagera med filsystemet. Cacheminnen lagrar också data i mycket enkelt datastrukturer som nyckelvärdepar, så att de inte behöver köra komplexa frågor för att samla in data eller underhålla index när du skriver data. Ett cacheminne är därför vanligtvis bättre än en databas. När du använder ett program, görs ett försök att läsa data från cachen först. Om begärda data inte är i cacheminnet, hämtar programmet den från databasen, som alltid är klar. Men sedan lagras data i cacheminnet för efterföljande förfrågningar. Nästa gång när en användare begär data, returneras det från cachen direkt.

![Diagram för att läsa in data som ska cachelagras](../media-draft/cache-aside-set-cache.png)

### <a name="how-to-manage-updating-data"></a>Så här hanterar du uppdaterar data

När du implementerar mönstret cache-aside lägger du till ett litet problem. Eftersom dina data lagras nu i ett cacheminne och ett datalager, kan du köra får problem när du försöker göra en uppdatering. Om du vill uppdatera dina data, skulle du behöva uppdatera både cacheminnet och datalagret.

Lösning på problemet i cache-aside-mönstret är att ogiltigförklara data i cacheminnet. När du uppdaterar data i ditt program bör du först ta bort data i cacheminnet och gör ändringar i datakällan direkt. Den kommer inte vara närvarande i cacheminnet genom att göra detta, nästa gång data begärs, och processen upprepas. 

![Diagram över ogiltigförklara cachelagrade data](../media-draft/cache-aside-invalidate.png)

## <a name="considerations-for-using-the-cache-aside-pattern"></a>Att tänka på när cache-aside-mönstret

Noga överväga vilka data du kan placera i minnet. Inte alla data är lämpligt att cachelagras.

- **Livslängd**: för cache-aside ska vara effektivt, se till att förfallopolicyn matchar åtkomstfrekvensen av data. Gör förfalloperioden för kort kan orsaken program att hämta data kontinuerligt från data lagra och lägga till det i cacheminnet.

- **Ta bort**: cacheminnen har en begränsad storlek jämfört med vanliga datalager och de tar bort data om det behövs. Kontrollera att du väljer en lämplig Borttagningsprincip för dina data.

- **Prima**: Om du vill göra cache-aside-mönstret effektiva många lösningar ska fylla i cachen med data som de tycker sker ofta.

- **Konsekvens**: Implementera cache-aside-mönstret inte garantera konsekvens mellan datalagret och cachen. Data i ett datalager kan ändras utan att meddela cachen. Detta kan leda till allvarliga synkroniseringsproblem.

Cache-aside-mönstret är användbart när du är obligatoriskt att komma åt data ofta från en datakälla som använder en disk. Med hjälp av cache-aside-mönstret ska du lagra viktiga data i ett cacheminne för att öka hastigheten för att hämta den. 
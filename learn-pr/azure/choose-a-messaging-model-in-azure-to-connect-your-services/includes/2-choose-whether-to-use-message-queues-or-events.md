Anta att du planerar arkitekturen till ett distribuerat program för delning av musik. Du vill att programmet ska vara så tillförlitligt och skalbart som möjligt och du planerar att använda Azure-tekniker till att bygga en robust kommunikationsinfrastruktur.

Innan du kan välja rätt Azure-teknik, måste du förstå varje enskild kommunikation som komponenterna i programmet utbyter. Du kan välja olika Azure-tekniker för olika sorters kommunikation.

Det första du ska ta reda på är om en kommunikation skickar meddelanden eller händelser. Vissa Azure-tekniker skickar händelser och andra skickar meddelanden, så detta är ett viktigt val.

## <a name="what-is-a-message"></a>Vad är ett meddelande?

I terminologin för distribuerade program har **meddelanden** följande egenskaper:

- Meddelandet innehåller rådata som genereras av en komponent och som ska användas av en annan komponent.
- Ett meddelande innehåller själva informationen, inte bara en referens till dessa data.
- Den sändande komponenten förväntar sig att meddelandeinnehållet ska bearbetas på ett visst sätt av målkomponenten. Det övergripande systemets integritet kan vara beroende av att både avsändaren och mottagaren utför ett specifikt jobb.

Anta exempelvis att en användare laddar upp en ny låt med hjälp av mobilappen för musikdelning. Mobilappen måste skicka låten till den webb-API som körs i Azure. Själva mediefilen måste skickas, inte bara en avisering som anger att en ny låt har lagts till. Mobilappen förväntar sig att webb-API:n sparar den nya låten i databasen och gör den tillgänglig för andra användare. Detta är ett exempel på ett meddelande.

## <a name="what-is-an-event"></a>Vad är en händelse?

**Händelser** är enklare än meddelanden och används oftast för sändningskommunikation. De komponenter som skickar händelsen kallas för **utgivare** och mottagare kallas för **prenumeranter**.

Med händelser avgör mottagande komponenter i allmänhet vilken kommunikation man är intresserad av och därefter ”prenumererar” man på den. Prenumerationen hanteras vanligtvis av en mellanhand som Azure Event Grid eller Azure Event Hub. När utgivaren skickar en händelse kommer mellanhanden att dirigera händelsen till intresserade prenumeranter. Detta kallas för en ”publicera/prenumerera-arkitektur”. Det är inte det enda sättet att hantera händelser på, men det är det vanligaste.

Händelser har följande egenskaper:

- En händelse är ett förenklat meddelande som anger att något har inträffat.
- Händelsen kan skickas till flera mottagare eller inte till någon alls.
- Händelser är ofta avsedda att ”förgrenas” eller har ett stort antal prenumeranter för varje utgivare.
- Utgivaren av händelsen har ingen förväntan på åtgärder från den mottagande komponenten.
- Vissa händelser är diskreta enheter som inte är relaterade till andra händelser. 
- Vissa händelser är en del av en relaterad och ordnad serie.  

Anta exempelvis att filöverföringen av musik har slutförts och att en ny låt har lagts till i databasen. För att kunna informera användarna om den nya filen måste webb-API:n meddela webbklientdelen och mobilappanvändarna om detta. Användarna kan välja om de vill lyssna på den nya låten. Därför innehåller den första aviseringen inte musikfilen, utan meddelar bara användarna om att låten finns. Avsändaren har inte någon specifik förväntan på att händelsemottagarna ska göra något särskilt som svar på mottagandet av händelsen.

Detta är ett exempel på en diskret händelse.

## <a name="how-to-choose-messages-or-events"></a>Att välja meddelanden eller händelser

Ett program använder troligen händelser för vissa syften och meddelanden för andra. Innan du väljer måste du analysera din programarkitektur och dess användning för att identifiera de olika syften som dess komponenter har för att kommunicera med varandra. 

Händelser används troligen för sändningar och är ofta tillfälliga, vilket innebär att ett meddelande kanske inte hanteras av någon mottagare om ingen prenumererar för närvarande. Det är sannolikt att meddelanden används där det distribuerade programmet kräver en garanti för att kommunikationen kommer att bearbetas.

För varje typ av kommunikation bör du ställa dig frågan: Förväntar den sändande komponenten att kommunikationen ska bearbetas på ett visst sätt av målkomponenten?

Om svaret är Ja bör du välja att använda ett meddelande. Om svaret är Nej kan du använda händelser.

## <a name="summary"></a>Sammanfattning

Komponenterna i ett distribuerat program kommunicerar med varandra i många olika syften. För vart och ett av dessa syften måste du välja om du vill använda händelser eller meddelanden, för att du ska kunna använda en Azure-teknik som har utformats för ändamålet. 

När du har förstått varför dina komponenter kommunicerar och om de använder händelser eller meddelanden, kan du fortsätta med att välja mellan Azure Storage-köer, Event Hubs, Event Grid eller Service Bus för att utbyta informationen.
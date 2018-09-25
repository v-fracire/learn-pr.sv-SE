Anta att du är en fotograf och att du har en webbplats där du visar upp dina bilder. Eftersom du är upptagen kan du har inte ett konsekvent uppladdningsschema, men du vill meddela dina följare när du laddar upp en ny bild. Du vill skapa en Azure-funktion för att automatiskt skicka en tweet när du laddar upp en bild till din Azure Storage blob-container.

Här får du lära dig hur du skapar en blob-utlösare och instruerar den att övervaka en specifik plats i din Azure Storage blob-container.

## <a name="what-is-azure-storage"></a>Vad är Azure Storage?

Azure Storage är Microsofts molnlagringslösning som har stöd för alla typer av data, inklusive: blobar, köer och NoSQL. Målet med Azure Storage är att tillhandahålla lagring av data som kännetecknas av:

- Högt tillgänglighet
- Säkerhet
- Skalbarhet
- Hanterad

Vi kommer inte att fokusera på Azure Storage för mycket. I stället använder vi den för att skapa blobar som ska utlösa vår funktion.

## <a name="what-is-azure-blob-storage"></a>Vad är Azure Blob Storage?

Azure Blob Storage en lagringslösning för objekt som är utformad för att lagra stora mängder ostrukturerade data. 

Azure Blob Storage är till exempel bra för att göra saker som att:

- Lagra filer
- Tillhandahålla filer
- Direktuppspela video och ljud
- Logga data

Det finns tre typer av blobbar: **Blockblobbar**, **bilageblobbar** och **sidblobbar**. Blockblobar är de vanligaste. De gör det möjligt att lagra text eller binära data på ett effektivt sätt. Tilläggsblobar är som blockblobar, men de är mer utformade för tilläggsåtgärder som att skapa en loggfil som uppdateras kontinuerligt. Slutligen består sidblobar av sidor och är utformade för frekventa icke-sekventiella läs- och skrivåtgärder.

## <a name="what-is-a-blob-trigger"></a>Vad är en blob-utlösare?

En blob-utlösare är en utlösare som kör en funktion när en fil laddas upp eller uppdateras i Azure Blob Storage. Om du vill skapa en blob-utlösare skapar du ett Azure Storage-konto och anger en plats som utlösaren ska övervaka.

## <a name="how-to-create-a-blob-trigger"></a>Så här skapar du en blob-utlösare

Precis som de andra utlösarna vi har sett hittills skapar vi en blob-utlösare i Azure-portalen. I din Azure-funktion väljer du **Blob-utlösare** från listan över fördefinierade utlösartyper. Sedan anger du logiken som ska köras när en blob skapas eller uppdateras.

En inställning som du bör titta på är **Sökväg**. **Sökvägen** informerar blob-utlösaren om vad den ska övervaka för att se om en blob laddas upp eller uppdateras. Som standard är värdet för **Sökväg**: 

> samples-workitems/{name}

Vi kan dela upp det här begreppet i två delar: *samples-workitems* och *{name}*. Den första delen, *samples-workitems*, representerar den blobcontainer som utlösaren övervakar. Den andra delen *{name}, innebär att alla filtyper får utlösaren att anropa funktionen. Funktionen anropas eftersom det inte finns något filter. Jag skulle till exempel kunna göra så att utlösaren endast anropar funktionen när en PNG-fil läggs till med hjälp av en syntax som:

> samples-workitems/{name}.png

Den sista viktiga informationen om det här konceptet är texten *namn*. *Namn* representerar en parameter i din Azure-funktion som tar emot namnet på den tillagda filen. Om jag till exempel laddar upp en fil med namnet *resume.txt* tar min Azure-funktion emot det värdet som en sträng via en parameter som kallas *namn*.

En blob-utlösare anropar en Azure-funktion när den ser aktivitet på en viss plats i Azure Storage blob-kontot. Du kan ange platsen som ska övervakas genom att ändra värdet **Sökväg** i Azure-portalen.
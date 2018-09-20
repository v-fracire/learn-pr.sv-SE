Blobar är ”filer för molnet”. Appar fungerar med blobar på ungefär samma sätt som med filer på en disk, där de läser och skriver data. Men till skillnad från en lokal fil är blobar tillgängliga överallt via en Internetanslutning.

Azure Blob Storage är *ostrukturerad*, vilket innebär att det inte finns några begränsningar för vilken typ av data som tjänsten kan lagra. En blob kan till exempel innehålla ett PDF-dokument, en JPG-bild, en JSON-fil, videoinnehåll osv. Blobar är inte begränsade till vanliga filformat &mdash; en blob kan innehålla flera gigabyte binära data som strömmas från ett vetenskapligt instrument, ett krypterat meddelande för ett annat program eller data i ett anpassat format för en app som du utvecklar.

Blobar är vanligtvis inte lämpliga för strukturerade data som används ofta. De har längre svarstid än minne och lokala diskar och saknar de indexeringsfunktioner som gör databaser så effektiva för frågekörning. Blobar används dock ofta i *kombination* med databaser för att lagra data som det inte går att köra frågor mot. En app med en databas för användarprofiler kan till exempel lagra profilbilder i blobar. Varje användarpost i databasen skulle i så fall innehålla namnet eller URL:en för bloben som innehåller användarens bild.

Blobar används för att lagra data på många olika sätt i alla typer av program och arkitekturer:

- Appar som behöver överföra stora mängder data med meddelandesystem som bara har stöd för små meddelanden. Dessa appar kan lagra data i blobar och skicka blob-URL:er i meddelanden.
- Blob Storage kan användas som ett filsystem för att lagra och dela dokument och andra personliga data.
- Statiska webbresurser, till exempel bilder, kan lagras i blobar och göras tillgängliga för offentlig nedladdning som om de vore filer på en webbserver.
- Många Azure-komponenter använder blobar i bakgrunden. Exempelvis lagrar Azure Cloud Shell dina filer och konfigurationer i blobar, och Azure Virtual Machines använder blobar för hårddisklagring.

Vissa appar skapar, uppdaterar och tar bort blobar kontinuerligt som en del av deras arbete. Andra använder en liten uppsättning blobar och ändrar dem sällan.

## <a name="storage-accounts-containers-and-metadata"></a>Lagringskonton, containrar och metadata

I Blob Storage finns varje blob i en *blobcontainer*. Du kan lagra ett obegränsat antal blobar i en container och ett obegränsat antal containrar i ett lagringskonto. Containrar är ”platta” &mdash; de kan endast lagra blobar, inte andra containrar.

Blobar och containrar stöder metadata i form av namn/värde-strängpar. Dina appar kan använda metadata för vad du vill: en läsbar beskrivning av en blobs innehåll som ska visas av programmet, en sträng som din app använder för att avgöra hur blobens data ska bearbetas osv.

> [!TIP]
> Blob Storage tillhandahåller ingen mekanism för att söka efter eller sortera blobar baserat på metadata. Information om hur du använder Azure Search för att göra det finns i avsnittet Ytterligare läsning i slutet av den här modulen.

## <a name="the-blob-storage-api-and-client-libraries"></a>Blob Storage-API och klientbibliotek

API:et för Blob Storage är REST-baserat och stöds av klientbibliotek i många populära språk. Med API:et kan du skriva appar som skapar och tar bort blobar och containrar, som laddar upp och laddar ned blobdata och som visar en lista över blobarna i en container.

Du har planerat nätverkets infrastruktur och identifierat några virtuella datorer som du vill migrera till molnet. Du har flera valmöjligheter när du skapar dina virtuella datorer. Vad du väljer beror på vilken miljö du känner dig bekväm med. Azure har stöd för en webbaserad portal där du kan skapa och administrera resurser. Du kan också välja att använda kommandoradsverktyg som körs i MacOS, Windows och Linux.

> [!VIDEO https://www.microsoft.com/videoplayer/embed/RE2yJKx]

Vi ska börja med att utforska Azure Portal – det enklaste sättet att börja använda Azure.

[!include[](../../../includes/azure-sandbox-activate.md)]

## <a name="azure-portal"></a>Azure Portal

Den **Azure-portalen** ger en enkel att använda webbläsarbaserat användargränssnitt där du kan skapa och hantera alla dina Azure-resurser. Du kan till exempel konfigurera en ny databas, öka beräkningskraften i dina virtuella datorer och hålla koll på din månadskostnad. Det är också ett bra inlärningsverktyg eftersom du alla tillgängliga resurser en undersökning och använda guidad guider för att skapa de som du behöver.

När du har loggat in ser du två huvudområden. Först är en meny med alternativ för att skapa resurser, övervaka resurser och hantera fakturering. Det andra området är en anpassningsbar instrumentpanel som ger en snabb överblick över viktiga tjänster som du har distribuerat till Azure. Portalen brukar vara det enklaste sättet att komma igång med Azure.

> [!TIP]
> Vyerna som visas när du gör val på portalen kallas för _blad_. Ett blad kan fungera både som en menystruktur eller som en konfigurationspanel. När du navigerar i hela Azure-portalen Gränssnittet är stående från vänster till höger och web visningsområdet dras över för att visa bladet för aktuella. Du kan använda skjutreglaget längst ned på sidan för att snabbt gå tillbaka till överordnad vyer.

### <a name="create-an-azure-vm-with-the-azure-portal"></a>Skapa en virtuell Azure-dator med Azure Portal

Anta att du vill skapa en virtuell dator som kör en WordPress-webbplats. Det är inte svårt att konfigurera en plats, men det finns några saker som du måste tänka på. Du måste installera och konfigurera ett operativsystem, konfigurera en webbplats, installera en databas och tänka igenom hur du ska installera och konfigurera brandväggar. I de kommande modulerna beskriver vi mer i detalj hur du skapar virtuella datorer, men låt oss skapa en redan nu så att du ser hur enkelt det är. Vi går inte igenom alla alternativ här. Om du vill ha fullständig information om respektive alternativ rekommenderar vi modulerna i serien **Skapa en virtuell dator**.

#### <a name="select-a-location"></a>Välj en plats

[!include[](../../../includes/azure-sandbox-regions-first-mention-note.md)]

1. Logga in på [Azure Portal](https://portal.azure.com?azure-portal=true). Azure-resurs-menyn för skapande och hantering visas på vänster och instrumentpanelen fylla resten av skärmen.

    ![Huvudinstrumentpanelen på Azure Portal](../media-draft/3-dashboard-page.png)

1. Klicka på alternativet **Skapa en resurs** längst upp till vänster på portalsidan. Bladet Azure Marketplace öppnas. Om sidofältet till vänster är dolt visas ett grönt ”plustecken”. Du kan expandera sidopanelen genom att klicka på pilikonen för att visa allt innehåll, som du ser i bilden ovan.

    ![Azure Marketplace](../media-draft/3-create-new-resource.png)

    Som du ser finns det många alternativ som du kan välja mellan. Kom ihåg att vi vill skapa en virtuell dator som kör en WordPress-webbplats. Eftersom virtuella datorer är Azure-beräkningsresurser väljer du alternativet **Beräkna** i listan och söker sedan efter VM-avbildningar för WordPress. Du kan klicka på **finns i alla** att hämta den fullständiga listan.

1. Använd den **Sök på Marketplace** sökfältet och leta efter ”WordPress”. Du bör se en lista med alternativ. Välj alternativet **WordPress Certified by Bitnami**.

    ![Söka på Azure Marketplace](../media-draft/3-search-vm-image.png)

    På bladet som öppnas visas licensinformation för avbildningen som vi ska använda. Klicka på **Skapa**.

    ![Välj och skapa WordPress-webbplats](../media-draft/3-create-vm-image.png)

1. Visas den **Skapa virtuell dator** bladet. Notera den guidebaserade metoden som vi kan använda för att konfigurera den virtuella datorn.

    ![Konfigurationssteg 1](../media-draft/3-create-vm-1.png)

    Vi måste konfigurera de grundläggande parametrarna för vår virtuella WordPress-dator. Om några av alternativen i det här läget inte känner till dig, är det OK. Vi kommer att gå igenom alla dessa alternativ i detalj i en modul längre fram. Du kan kopiera värdena som används här.

1. Använd följande värden på fliken **Grunder**.
    - Välj den kostnadsfria prenumerationen och <rgn>[Sandbox resursgruppens namn]</rgn> resursgrupp.
    - Ange ett **namn** för den virtuella datorn: här har vi använt `test-wp1-eus-vm`.
    - Välj en **region** nära dig. Se till att välja en plats i listan som anges ovan från den nedrullningsbara listan.
    - Välj **Inga** för tillgänglighetsalternativen. Detta är för hög tillgänglighet, vilket vi upp i en annan modul.
    - Den **bild** bör vara den **WordPress bitnami** vi valt från Marketplace.
    - Lämna den **storlek** som standard – den ger dig en enda kärna och 3,5 GB minne, vilket är tillräckliga för en enkel webbplats.
    - Växla till **lösenord** för autentisering skriver och anger ett användarnamn och lösenord.
    - Välj **valda portar**, i listrutan, väljer du **http**.

    ![Öppna HTTP-porten](../media-draft/3-open-http-port.png)

1. Det finns flera andra flikar som du kan utforska om du vill se vilka inställningar du kan påverka när du skapar den virtuella datorn. När du är klar klickar du på **Granska + skapa** för att granska och verifiera inställningarna.

    ![Konfigurationssteg 2](../media-draft/3-review-create-vm.png)

1. Azure verifierar dina inställningar på granskningsskärmen. Kontrollera alla inställningar är inställda på sätt som du vill och klicka sedan på **skapa** att distribuera och skapa den virtuella datorn.

1. Du kan övervaka distributionen på panelen **Meddelanden**. Klicka på ikonen i det övre verktygsfältet för att visa panelen.

    ![Övervaka distributionsförloppet](../media-draft/3-deploying.png)

1. Det tar några minuter att distribuera den virtuella datorn. Ett meddelande visas när distributionen har slutförts. Klicka på meddelandet för att gå till resursgruppen med alla element som skapades för den virtuella datorn.

    ![Den virtuella datorn har distribuerats](../media-draft/3-deployment-succeeded.png)

1. Välj VM-posten – det bör vara det första och den har det namn du angav.

    ![Välj den virtuella datorn i resursgruppen](../media-draft/3-open-vm-properties.png)

1. När du gör det kommer du till **översikten** för den virtuella dator som du har skapat. Här ser du all information och alla konfigurationsalternativ för den nya virtuella WordPress-datorn. Du hittar bland annat **den offentliga IP-adressen** här.

    ![Hämta din offentliga IP-adress till den virtuella datorn](../media-draft/3-public-ip-address.png)

11. Kopiera IP-adressen, öppna en ny flik i webbläsaren och klistra in den. Det ska gå till en helt ny WordPress-webbplats.

    ![Ny WordPress-webbplats är aktiv](../media-draft/3-my-new-blog.png)

Grattis! Du har distribuerat en virtuell dator som kör Linux, har en databas som är installerad och har en funktionell webbplats med några steg. Nu ska vi titta på några andra metoder som du kan använda när du skapar en virtuell dator.

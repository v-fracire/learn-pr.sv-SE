Du har planerat nätverkets infrastruktur och identifierat några virtuella datorer som du vill migrera till molnet. Du har flera valmöjligheter när du skapar dina virtuella datorer. Vad du väljer beror på vilken miljö du känner dig bekväm med. Azure har stöd för en webbaserad portal där du kan skapa och administrera resurser. Du kan också välja att använda kommandoradsverktyg som körs i MacOS, Windows och Linux.

> [!TIP]
> Alla övningar i Microsoft Learn är kostnadsfria. När du börjar utforska på egen hand behöver du dock en Azure-prenumeration. Om du inte har någon än kan du skapa ett [kostnadsfritt konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F). Det tar bara några minuter.

Vi ska börja med att utforska Azure Portal – det enklaste sättet att börja använda Azure.

## <a name="azure-portal"></a>Azure Portal

**Azure Portal** har ett användarvänligt webbläsarbaserat användargränssnitt där du kan skapa och hantera alla dina Azure-resurser. Du kan till exempel konfigurera en ny databas, öka beräkningskraften i dina virtuella datorer och hålla koll på din månadskostnad. Portalen är också ett bra inlärningsverktyg eftersom du kan se alla tillgängliga resurser och använda guider för att skapa de resurser du behöver.

När du har loggat in ser du två huvudområden. Det första är en meny med alternativ som du kan använda för att skapa resurser, övervaka resurser och hantera fakturering. Det andra området är en anpassningsbar instrumentpanel där du ser en överblick över de viktiga tjänster du har distribuerat till Azure. Portalen brukar vara det enklaste sättet att komma igång med Azure.

> [!NOTE]
> Vyerna som visas när du gör val på portalen kallas för _blad_. Ett blad kan fungera både som en menystruktur och som en konfigurationspanel. Användargränssnittet i Azure Portal är strukturerat från vänster till höger och webbvisningsområdet skjuts in i vyn för att visa det aktiva bladet. Du kan använda skjutreglaget längst ned på sidan för att snabbt gå tillbaka till överordnade vyer.

### <a name="create-an-azure-vm-with-the-azure-portal"></a>Skapa en virtuell Azure-dator med Azure Portal

Anta att du vill skapa en virtuell dator som kör en WordPress-webbplats. Det är inte svårt att konfigurera en plats, men det finns några saker som du måste tänka på. Du måste installera och konfigurera ett operativsystem, konfigurera en webbplats, installera en databas och tänka igenom hur du ska installera och konfigurera brandväggar. I de kommande modulerna beskriver vi mer i detalj hur du skapar virtuella datorer, men låt oss skapa en redan nu så att du ser hur enkelt det är. Vi går inte igenom alla alternativ här. Om du vill ha fullständig information om respektive alternativ rekommenderar vi modulerna i serien **Skapa en virtuell dator**.

1. Logga in på [Azure Portal](https://portal.azure.com?azure-portal=true). Till vänster ser du menyn som du kan använda för att skapa och hantera Azure-resurser. Instrumentpanelen tar upp resten av skärmen.

    ![Huvudinstrumentpanelen i Azure Portal](../media-draft/3-dashboard-page.png)

1. Klicka på alternativet **Skapa en resurs** längst upp till vänster på portalsidan. Bladet Azure Marketplace öppnas. Om sidofältet till vänster är dolt visas ett grönt ”plustecken”. Du kan expandera sidopanelen genom att klicka på pilikonen för att visa allt innehåll, som du ser i bilden ovan.

    ![Azure Marketplace](../media-draft/3-create-new-resource.png)

    Som du ser finns det många alternativ att välja mellan. Kom ihåg att vi vill skapa en virtuell dator som ska köra en WordPress-webbplats. Eftersom virtuella datorer är Azure-beräkningsresurser väljer du alternativet **Compute** i listan och söker sedan efter VM-avbildningar för WordPress.

1. Använd sökfältet **Sök på Marketplace** och sök efter ”WordPress”. Du bör se en lista med alternativ. Välj alternativet **WordPress Certified by Bitnami**.

    ![Söka på Azure Marketplace](../media-draft/3-search-vm-image.png)

    På bladet som öppnas visas licensinformation för avbildningen som vi ska använda. Klicka på **Skapa**.

    ![Välja och skapa WordPress-webbplatsen](../media-draft/3-create-vm-image.png)

1. Bladet **Skapa virtuell dator** visas. Notera den guidebaserade metoden vi kan använda till att konfigurera den virtuella datorn.

    ![Konfigurationssteg 1](../media-draft/3-create-vm-1.png)

    Vi måste konfigurera de grundläggande parametrarna för vår virtuella WordPress-dator. Om du inte känner igen alla alternativ här så är det okej. Vi kommer att gå igenom alla dessa alternativ i detalj i en modul längre fram. Du kan kopiera värdena som används här.

<!-- TODO: fix subscription + resource group -->
1. Använd följande värden på fliken **Grunder**.
    - Välj den kostnadsfria prenumerationen och en resursgrupp.
    - Ange ett **namn** för den virtuella datorn: här har vi använt `test-wp1-eus-vm`.
    - Välj en **region** nära dig. Du kan använda den nedrullningsbara listan för att välja platsen.
    - Välj **Inga** för tillgänglighetsalternativen. Den här inställningen används för hög tillgänglighet, vilket beskrivs i en annan modul.
    - **Avbildning** bör vara alternativet **WordPress by Bitnami** som vi valde på Marketplace.
    - Lämna standardvärdet för **Storlek**. Med den här storleken får du en enda kärna och 3,5 GB minne, vilket bör vara tillräckligt för en enkel webbplats.
    - Växla till **Lösenord** som autentiseringstyp och ange ett användarnamn och lösenord.
    - Öppna listrutan **Välj offentliga inkommande portar** och välj **http** som du ser i bilden nedan.

    ![Öppna HTTP-porten](../media-draft/3-open-http-port.png)

1. Det finns flera andra flikar som du kan utforska om du vill se vilka inställningar du kan påverka när du skapar den virtuella datorn. När du är klar klickar du på **Granska + skapa** för att granska och verifiera inställningarna.

    ![Konfigurationssteg 2](../media-draft/3-review-create-vm.png)

1. Azure verifierar dina inställningar på granskningsskärmen. Kontrollera att alla inställningar är rätt konfigurerade och klicka sedan på **Skapa** för att distribuera och skapa den virtuella datorn.

1. Du kan övervaka distributionen på panelen **Meddelanden**. Klicka på ikonen i det övre verktygsfältet för att visa panelen.

    ![Övervaka distributionsförloppet](../media-draft/3-deploying.png)

1. Det tar några minuter att distribuera den virtuella datorn. Ett meddelande visas när distributionen har slutförts. Klicka på meddelandet för att gå till resursgruppen med alla element som skapades för den virtuella datorn.

    ![Den virtuella datorn har distribuerats](../media-draft/3-deployment-succeeded.png)

1. Välj posten för den virtuella datorn. Det bör vara den första posten och ha det namn du angav.

    ![Välj den virtuella datorn i resursgruppen](../media-draft/3-open-vm-properties.png)

1. När du gör det kommer du till **översikten** för den virtuella dator som du har skapat. Här ser du all information och alla konfigurationsalternativ för den nya virtuella WordPress-datorn. Du hittar bland annat **den offentliga IP-adressen** här.

    ![Hämta din offentliga IP-adress till den virtuella datorn](../media-draft/3-public-ip-address.png)

11. Kopiera IP-adressen, öppna en ny flik i webbläsaren och klistra in den. Den bör öppna en helt ny WordPress-webbplats.

    ![Den nya WordPress-webbplatsen är igång](../media-draft/3-my-new-blog.png)

Grattis! I några få steg har du distribuerat en virtuell Linux-dator som har en installerad databas och en fungerande webbplats. Nu ska vi titta på några andra metoder som du kan använda när du skapar en virtuell dator.

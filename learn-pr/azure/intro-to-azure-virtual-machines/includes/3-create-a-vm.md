Du har planerat nätverkets infrastruktur och identifierat några virtuella datorer som du vill migrera till molnet. Du har flera valmöjligheter när du skapar dina virtuella datorer. Vad du väljer beror på vilken miljö du känner dig bekväm med. Azure har stöd för en webbaserad portal där du kan skapa och administrera resurser. Du kan också välja att använda kommandoradsverktyg som körs i MacOS, Windows och Linux.

[!include[](../../../includes/azure-sandbox-activate.md)]

#### <a name="options-to-create-and-manage-vms"></a>Alternativ för att skapa och hantera virtuella datorer

> [!VIDEO https://www.microsoft.com/videoplayer/embed/RE2yJKx]

Vi ska börja med att utforska Azure-portalen – det enklaste sättet att börja använda Azure.

## <a name="azure-portal"></a>Azure Portal

**Azure Portal** har ett användarvänligt webbläsarbaserat användargränssnitt där du kan skapa och hantera alla dina Azure-resurser. Du kan till exempel konfigurera en ny databas, öka beräkningskraften på dina virtuella datorer och hålla koll på din månadskostnad. Portalen är också ett bra inlärningsverktyg eftersom du kan se alla tillgängliga resurser och använda guider för att skapa de resurser du behöver.

När du har loggat in ser du två huvudområden. Det första är en meny med alternativ som du kan använda för att skapa resurser, övervaka resurser och hantera fakturering. Det andra området är en anpassningsbar instrumentpanel där du ser en överblick över de viktiga tjänster du har distribuerat till Azure. Portalen brukar vara det enklaste sättet att komma igång med Azure.

> [!TIP]
> Vyerna som visas när du gör val på portalen kallas för _blad_. Ett blad kan fungera både som en menystruktur och som en konfigurationspanel. Användargränssnittet i Azure Portal är strukturerat från vänster till höger och webbvisningsområdet skjuts in i vyn för att visa det aktiva bladet. Du kan använda skjutreglaget längst ned på sidan för att snabbt gå tillbaka till överordnade vyer.

### <a name="create-an-azure-vm-with-the-azure-portal"></a>Skapa en virtuell Azure-dator med Azure Portal

Anta att du vill skapa en virtuell dator som kör en WordPress-webbplats. Det är inte svårt att konfigurera en plats, men det finns några saker som du måste tänka på. Du måste installera och konfigurera ett operativsystem, konfigurera en webbplats, installera en databas och tänka igenom hur du ska installera och konfigurera brandväggar. I de kommande modulerna beskriver vi mer i detalj hur du skapar virtuella datorer, men låt oss skapa en redan nu så att du ser hur enkelt det är. Vi går inte igenom alla alternativ här. Om du vill ha fullständig information om respektive alternativ rekommenderar vi modulerna i serien **Skapa en virtuell dator**.

1. Logga in på [Azure-portalen](https://portal.azure.com/triplecrownlabs.onmicrosoft.com?azure-portal=true) med samma konto som du använde för att aktivera sandbox-miljön. 

1. Till vänster ser du menyn som du kan använda för att skapa och hantera Azure-resurser. Instrumentpanelen tar upp resten av skärmen.

    ![En skärmbild som visar huvudinstrumentpanelen i Azure Portal](../media/3-dashboard-page.png)

1. Klicka på alternativet **Skapa en resurs** längst upp till vänster på portalsidan. Bladet Azure Marketplace öppnas. Om sidofältet till vänster är dolt visas ett grönt ”plustecken”. Du kan expandera sidopanelen genom att klicka på pilikonen, så visas all text så som visas på bilden ovan.

    ![En skärmbild som visar Microsoft Azure Marketplace med skapandet av en resurs markerat.](../media/3-create-new-resource.png)

    Som du ser finns det många alternativ att välja mellan. Kom ihåg att vi vill skapa en virtuell dator som ska köra en WordPress-webbplats. Eftersom virtuella datorer är Azure-beräkningsresurser väljer du alternativet **Compute** i listan och söker sedan efter VM-avbildningar för WordPress. Du kan klicka på **Visa alla** för att få den fullständiga listan.

1. Använd sökfältet **Sök på Marketplace** och sök efter ”WordPress”. Du bör se en lista med alternativ. Välj alternativet **WordPress 4.9.7** så som visas nedan.

    ![En skärmbild som visar Sök på Marketplace med WordPress 4.9.7 markerat.](../media/3-search-vm-image.png)

    På bladet som öppnas visas licensinformationen för den avbildning som vi ska använda. Klicka på **Skapa**.

    ![En skärmbild som visar bladet Välj och skapa WordPress med WordPress 4.9.7 markerat.](../media/3-create-vm-image.png)

1. Bladet **Skapa virtuell dator** visas. Notera den guidebaserade metoden vi kan använda till att konfigurera den virtuella datorn.

### <a name="configure-the-vm"></a>Konfigurera den virtuella datorn

Vi måste konfigurera de grundläggande parametrarna för vår virtuella WordPress-dator. Om du inte känner igen alla alternativ här så är det okej. Vi kommer att gå igenom alla dessa alternativ i detalj i en framtida modul. Du kan kopiera de värden som används här.

1. Använd följande värden på fliken **Grunder**.
    - Prenumerationen ska vara inställd på _Concierge-prenumeration_.
    - Välj **Använd befintlig** för region och välj sedan <rgn>[Sandbox resursgruppens namn]</rgn> från listrutan.
    - Ange ett **Namn** för aviseringen. Använd _test-wp1-eus-vm_.
    - Välj en **Region** nära till dig från följande lista.
        [!include[](../../../includes/azure-sandbox-regions-note-friendly.md)]
    - Välj _Inga_ som **Tillgänglighetsalternativ**. Det här är för hög tillgänglighet.
    - **Avbildning** bör vara alternativet _WordPress 4.9.7_ som vi valde på Marketplace.
    - Lämna **Storlek** som standarden _Standard A1_ – det ger dig en enda kärna och 1,75 GB minne, vilket borde räcka för en enkel webbplats.
    - Växla till **Lösenord** som autentiseringstyp och ange ett användarnamn och lösenord.

    ![Skärmbild som visar skärmen Skapa en virtuell dator med informationen ifylld](../media/3-create-vm-1.png)

1. Det finns flera andra flikar som du kan utforska om du vill se vilka inställningar du kan påverka när du skapar den virtuella datorn. När du är klar klickar du på **Granska + skapa** för att granska och verifiera inställningarna.

1. Azure verifierar dina inställningar på granskningsskärmen. Du kan behöva ange ytterligare information baserat på kraven för avbildningens skapare. Kontrollera att alla inställningar är rätt konfigurerade och klicka sedan på **Skapa** för att distribuera och skapa den virtuella datorn.

1. Du kan övervaka distributionen på panelen **Meddelanden**. Visa eller dölj panelen genom att klicka på ikonen i det övre verktygsfältet.

    ![En skärmbild som visar övervakningen av distributionsförloppet](../media/3-deploying.png)

1. Det tar några minuter att distribuera den virtuella datorn. Ett meddelande visas när distributionen har slutförts. Klicka på knappen **Gå till resurs** för att gå till översiktssidan för den virtuella datorn.

    ![Skärmbild som visar en lyckad distribution av Wordpress-avbildningen](../media/3-deployment-succeeded.png)

1. Här ser du all information och alla konfigurationsalternativ för den nya virtuella WordPress-datorn. En del av informationen är **den offentliga IP-adressen**.

    ![Skärmbilden visar översiktssidan för den virtuella datorn med dess offentliga IP-adress markerad.](../media/3-public-ip-address.png)

11. Kopiera IP-adressen, öppna en ny flik i webbläsaren och klistra in den. Den bör öppna en helt ny WordPress-webbplats.

    ![Skärmbilden visar den nya WordPress-webbplatsen](../media/3-my-new-blog.png)

Grattis! I några få steg har du distribuerat en virtuell Linux-dator som har en installerad databas och en fungerande webbplats. Nu ska vi titta på några andra metoder som du kan använda när du skapar en virtuell dator.

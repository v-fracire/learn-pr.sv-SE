Anta att du har en webbplats för fotodelning med data som lagras på virtuella Azure-datorer (VM) som kör SQL Server och anpassade program. Du vill göra följande justeringar.

- Du behöver ändra inställningarna för diskcache på en virtuell dator
- Du vill lägga till en ny datadisk till den virtuella datorn med cachelagring aktiverat

Du har valt att göra dessa ändringar via Azure-portalen.

I den här övningen går vi igenom hur du genomför de ändringar av en virtuell dator som beskrevs ovan. Först loggar vi in på portalen och skapar en virtuell dator.

## <a name="sign-in-to-the-azure-portal"></a>Logga in på Azure-portalen
<!---TODO: Update for sandbox?--->

1. Logga in på [Azure-portalen](https://portal.azure.com/?azure-portal=true).

## <a name="create-a-virtual-machine"></a>Skapa en virtuell dator

I det här steget skapar vi en virtuell dator med följande egenskaper.

|Egenskap  |Värde  |
|---------|---------|
|Avbildning     |   **Windows Server 2016 Datacenter**      |
|Namn     |   **fotoshareVM**     |
|Resursgrupp     |   **fotoshare-rg**      |


1. I menyn till vänster på portalen väljer du **Virtuella datorer**

1. Välj nu **+ Lägg till** uppe till vänster på skärmen **Virtuella datorer**. Den här åtgärden startar skapandeprocessen.

1. På panelen Compute (Beräkning) som visar en lista över tillgängliga VM-avbildningar skriver du *Windows Server 2016 Datacenter* i sökrutan.

1. Välj **Windows Server 2016 Datacenter** bland sökresultatet och välj sedan **Skapa** för att starta VM-skapandeprocessen.

1. På panelen **Basics** (Grundläggande) går du till rutan **Namn** och anger **fotoshareVM**

1. I rutorna **Användarnamn** och **Lösenord** anger du ett namn och lösenord för ett administratörskonto på den här servern.

1. I listrutan **Prenumeration** väljer du din Azure-prenumeration.

1. Under **Resursgrupp** väljer du **Skapa ny** och skriver in **fotoshare-rg**.

1. Välj en region nära dig i listrutan **Plats**.

Följande är ett exempel på hur den **Grundläggande** konfigurationen ser ut när den har fyllts i.

![Skärmbild på Grundläggande för en virtuell dator som har fyllts i.](../media-draft/vm-basics-settings.PNG)

1. Välj **OK** för att gå vidare till nästa steg.

Nu behöver du välja en storlek för den virtuella datorn och sedan starta distributionen:

> [!IMPORTANT]
> Kom ihåg att diskcachelagring inte kan ändras för virtuella datorer i L-serien och B-serien. Vi väljer en annan storlek.

1. I avsnittet **Välj storlek** väljer du **Standard** SKU, till exempel **F1s** och väljer sedan **Välj**.

1. Rulla nedåt i fönsterrutan **Inställningar** och notera att det inte finns något alternativ för att konfigurera diskcachelagring. Vi accepterar standardinställningarna på det här steget och väljer **OK** längst ned i fönsterrutan.

1. På panelen **Skapa** granskar du sammanfattningen och väljer sedan **Skapa**.

1. Det kan ta en stund att skapa en virtuell dator. Vänta tills den virtuella datorn har distribuerats innan du fortsätter med den här övningen. Du får ett meddelande i meddelandehubben när processen är klar.

> [!IMPORTANT]
> Vi använder den här virtuella datorn i nästa kurs, så behåll den ett tag.

## <a name="view-os-disk-cache-status-in-the-portal"></a>Visa status för OS-diskcache i portalen

När vår virtuella dator har distribuerats kan vi bekräfta cachelagringsstatus för OS-disken med följande steg.

1. I den vänstra menyn klickar du på **Alla resurser** och väljer sedan din virtuella dator **fotoshareVM**.

1. På skärmen **Virtuella datorer** går du till **INSTÄLLNINGAR** och väljer **Diskar**.

1. På fönsterrutan **Diskar** har den virtuella datorn en disk, OS-disken. Dess cachetyp är för närvarande inställd på standardvärdet **Läsa/skriva**.

![Skärmbild på våra OS- och datadiskar som båda är inställda på skrivskyddad cachelagring.](../media-draft/os-disk-rw.PNG)

## <a name="change-the-cache-settings-of-the-os-disk-in-the-portal"></a>Ändra inställningarna för cachelagring för OS-disken i portalen

1. På fönsterrutan **Diskar** väljer du **Redigera** i det övre vänstra hörnet på skärmen.

1. Ändra värdet **HOST CACHING** (Cachelagring för värd) för OS-disken till **Read-only** (Skrivskyddad) med hjälp av listrutan, och välj sedan **Spara** i det övre vänstra hörnet på skärmen.

1. Den här uppdateringen tar en stund. Anledningen är att när du ändrar cacheinställningen för en Azure-disk så frånkopplas och återansluts måldisken. Om det är operativsystemdisken startas även den virtuella datorn om. Du får ett meddelande som liknar följande meddelande när uppdateringen har slutförts.

![Exempel på meddelande som du får när uppdateringen av cacheinställningen har slutförts.](../media-draft/vm-disk-update-complete.PNG)

4. När det är klart ställs cachetypen för OS-disken in på **Read-only** (Skrivskyddad).

Vi går vidare till konfigurationen av cachelagring för datadisk. För att konfigurera en disk behöver vi först skapa en.

## <a name="add-a-data-disk-to-the-vm-and-set-caching-type"></a>Lägga till en datadisk till den virtuella datorn och ange cachelagringstyp

1. När du är tillbaka i vyn **Diskar** för den virtuella datorn i portalen väljer du **Lägg till datadisk**. Ett fel visas omedelbart i fältet **Namn**, vilket anger att fältet inte får vara tomt. Vi har inte någon datadisk ännu, så vi tar och skapar en.

1. Klicka på listan **Namn** och klicka sedan på **Skapa disk**.

1. I fönsterrutan **Skapa hanterad disk** går du till rutan **Namn** och skriver **fotosharesVM-data**.

1. Under **Resursgrupp** väljer du **Använd befintlig** och väljer **fotoshare-rg** från listrutan.

1. Välj **Skapa** längst ned på skärmen.

1. Vänta tills disken har skapats innan du fortsätter.

1. Ändra värdet **HOST CACHING** (Cachelagring för värd) för den nya datadisken till **Read-only** (Skrivskyddad) med hjälp av listrutan, och välj sedan **Spara** i det övre vänstra hörnet på skärmen.

1. Vänta tills den virtuella datorn har uppdaterat. Uppdateringen tar en stund eftersom Azure frånkopplar och återansluter datadisken för att ändra den här inställningen.

1. När det är klart ställs cachetypen för datadisken in på **Read-only** (Skrivskyddad).

I den här övningen använde vi Azure-portalen för att konfigurera cachelagring på en ny virtuell dator, ändra inställningarna för cachelagring på en befintlig disk och konfigurera cachelagring på en ny datadisk. Följande skärmbild visar den slutgiltiga konfigurationen. 

![Skärmbild på våra OS- och datadiskar som båda är inställda på skrivskyddad cachelagring.](../media-draft/disks-final-config-portal.PNG)
Som du kanske minns sysslar vårt företag med bearbetning av videoinnehåll på virtuella Windows-datorer. En ny stad har anlitat oss för att bearbeta video från deras trafikkameror, men det är en modell som vi inte har arbetat med tidigare. Vi måste skapa en ny virtuell Windows-dator och installera några tillverkarspecifika codec-rutiner så att vi kan börja bearbeta och analysera kundens bilder.

## <a name="create-a-new-windows-virtual-machine"></a>Skapa en ny virtuell Windows-dator

Vi kan skapa en virtuell Windows-dator med Azure Portal, Azure CLI eller Azure PowerShell. Den enklaste metoden är portalen eftersom den går igenom den information som krävs samt visar tips och användbara meddelanden när vi skapar den virtuella datorn.

1. Logga in till [Azure Portal](https://portal.azure.com?azure-portal=true).

1. Klicka på **Skapa en resurs** uppe till vänster på Azure Portal.

1. Ange **Windows Server 2016 Datacenter** i sökrutan och klicka sedan på länken med samma namn i listan som visas.

1. Klicka på knappen **Skapa** för att börja konfigurera den virtuella datorn.

## <a name="configure-the-vm-settings"></a>Konfigurera inställningarna för den virtuella datorn

När du skapar en virtuell dator på portalen går du igenom en guide som vägleder dig genom alla konfigurationsområden. När du klickar på ”Nästa” kommer du till nästa konfigurerbara avsnitt. Du kan dock förflytta dig mellan avsnitten när du vill med hjälp av flikarna som visas längs överkanten och som identifierar varje avsnitt.

![Skapa en virtuell dator på Azure Portal](../media-drafts/3-azure-portal-create-vm.png)

När du har fyllt i alla obligatoriska alternativ (som visas med röda stjärnor) kan du hoppa över resten av guiden och börja skapa den virtuella datorn med hjälp av knappen **Granska + skapa** längst ned.

Vi börjar med avsnittet **Grundläggande inställningar**.

### <a name="configure-basic-vm-settings"></a>Konfigurera grundläggande inställningar för en virtuell dator

1. Välj den **prenumeration** som den virtuella datorns körningstimmar ska debiteras mot.

1. Välj **Skapa ny** för **Resursgrupp** och ge resursgruppen namnet **ExerciseResources**.

> [!NOTE]
> När du ändrar inställningen för ett fält och sedan flyttar från fältet valideras det nya värdet automatiskt av Azure och en grön bock visas bredvid värdet om det är giltigt. Du kan hovra över felindikatorerna om du vill ha mer information om problem som identifieras.

1. I avsnittet **INSTANSINFORMATION** anger du ett namn på den virtuella datorn, till exempel ”test-vp-vm2” (för Test Video Processor VM #2).
    - Det är bästa praxis att standardisera resursnamnen så att du enkelt kan identifiera deras syfte. Namnen på virtuella Windows-datorer är en aning begränsade – de måste vara mellan 2 och 15 tecken och måste bestå av siffror, bokstäver och bindestreck.

1. Välj en region nära dig.

1. Lämna **Tillgänglighetsalternativ** som ”Inga”. Det här alternativet används för att se till att den virtuella datorn har hög tillgänglighet. Detta görs genom att flera virtuella datorer grupperas för att underlätta hanteringen av planerade eller oplanerade underhållsåtgärder eller avbrott.

1. Kontrollera att avbildningen är inställd på ”Windows Server 2016 Datacenter”. Du kan öppna listrutan för att visa alla tillgängliga alternativ.

1. I fältet **Typ av virtuell datordisk** klickar du på listrutan så att du ser alternativen. Kontrollera att **SSD** är valt.

1. Fältet **Storlek** kan inte ändras direkt och har standardstorleken DS1. Klicka på länken **Ändra storlek** om du vill utforska andra storlekar på virtuella datorer. I dialogrutan som visas kan du filtrera baserat på antal processorer, namn och disktyp. Välj ”Standard DS1 v2” (vanligtvis standardinställningen) när du är klar. Med det här alternativet tilldelas den virtuella datorn 1 processor och 3,5 GB minne.

    > [!TIP]
    > Du kan också dra vyn åt vänster för att gå tillbaka till inställningarna för den virtuella datorn eftersom ett nytt fönster öppnades till höger och placerades ovanpå det.

1. I fältet **Användarnamn** i avsnittet **ADMINISTRATÖRSÅTKOMST** anger du ett användarnamn som du ska använda för att logga in på den virtuella datorn.

1. I fältet **Lösenord** anger du ett lösenord som är minst 12 tecken långt. Det måste innehålla tre av följande typer av tecken: en gemen, en versal, en siffra och ett specialtecken som inte är ”\'” eller ”-”. Använd något som är enkelt att komma ihåg eller skriv ned det eftersom du behöver det senare.

1. Bekräfta **lösenordet**.

1. I avsnittet **REGLER FÖR INKOMMANDE PORTAR** öppnar du listan och _avmarkerar_ ”Inga”. Eftersom det här är en virtuell Windows-dator vill vi kunna komma åt skrivbordet via RDP. Bläddra i listan om det behövs tills du hittar RDP (3389) och välj det. Som meddelandet i användargränssnittet indikerar kan vi även ändra nätverksportarna efter att vi har skapat den virtuella datorn.

    ![Öppna porten för RDP-åtkomst på den virtuella Windows-datorn](../media-drafts/3-open-ports.png)

## <a name="configure-disks-for-the-vm"></a>Konfigurera diskar för den virtuella datorn

1. Gå till avsnittet Diskar genom att klicka på **Nästa**.

    ![Konfigurera diskar för den virtuella datorn](../media-drafts/3-configure-disks.png)

1. Välj ”Premium SSD” för **OS-disktypen**.

1. Använd hanterade diskar så att vi inte behöver arbeta med lagringskonton. Om du vill kan du växla inställningen i det grafiska användargränssnittet för att se skillnaden i vilken typ av information Azure behöver.

### <a name="create-a-data-disk"></a>Skapa en datadisk

Som du kanske minns får vi en operativsystemdisk (C:) och en temporär disk (D:). Nu ska vi lägga till en datadisk också.

1. Klicka på länken **Create and attach a new disk** (Skapa och koppla en ny disk) i avsnittet **DATADISKAR**.

    ![Skapa en datadisk för den virtuella datorn på portalen](../media-drafts/3-add-data-disk.png)

1. Du kan använda alla standardvärden: Premium SSD, 1 023 GB och Inget (tom disk). Observera att det är här vi skulle kunna använda en ögonblicksbild eller Storage Blob för att skapa en virtuell hårddisk.

1. Klicka på **OK** för att skapa disken och gå tillbaka till avsnittet **DATADISKAR**.

1. En ny disk bör visas på den första raden.

    ![Ny disk på den virtuella datorn](../media-drafts/3-new-disk.png)

## <a name="configure-the-network"></a>Konfigurera nätverket

1. Gå till avsnittet Nätverk genom att klicka på **Nästa**.

1. I ett produktionssystem där vi redan har andra komponenter vill vi använda ett _befintligt_ virtuellt nätverk. På så sätt kan vår virtuella dator kommunicera med de andra molntjänsterna i vår lösning. Om ingenting har definierats på den här platsen än, kan vi skapa det här och konfigurera:
    - **Adressutrymme**: Hela IPV4-utrymmet som är tillgängligt för det här nätverket.
    - **Intervall för undernät**: Det första undernätet som delar in adressutrymmet i underordnade nät – det måste ligga inom det definierade adressutrymmet. När det virtuella nätverket har skapats kan du lägga till fler undernät.

1. Nu ska vi ändra standardintervallen och i stället använda IP-adressutrymmet `172.xxx`.
    - Ändra fältet **Adressutrymme** till `172.16.0.0/16` så att det tilldelas hela adressintervallet.
    - Ändra fältet **Subnet Range** (Intervall för undernät) till `172.16.1.0/24` och tilldela 256 IP-adresser av utrymmet.

> [!NOTE]
> Som standard skapar Azure ett virtuellt nätverk, ett nätverksgränssnitt och en offentlig IP-adress för din virtuella dator. Det är inte helt okomplicerat att ändra nätverksalternativen i efterhand, när den virtuella datorn har skapats. Därför bör du alltid dubbelkolla nätverkstilldelningarna för tjänster som du skapar i Azure.

## <a name="finish-configuring-the-vm-and-create-the-image"></a>Slutför konfigurationen av den virtuella datorn och skapa avbildningen

Resten av alternativen har redan lämpliga standardinställningar och du behöver inte ändra dem. Utforska de andra flikarna om du vill. Bredvid respektive alternativ visas en `(i)`-ikon som öppnar en hjälpbubbla med en beskrivning av alternativet. Beskrivningen är ett bra sätt att lära dig mer om de olika alternativen som du kan använda för att konfigurera den virtuella datorn.

1. Klicka på knappen **Granska + skapa** längst ned på panelen.

1. Alternativen valideras och visar information om den virtuella datorn som skapas.

1. Skapa och distribuera den virtuella datorn genom att klicka på **Skapa**. På instrumentpanelen i Azure ser du den virtuella dator som distribueras. Det här kan ta flera minuter.

Medan den virtuella datorn distribueras ska vi titta närmare på vad vi kan göra med den.
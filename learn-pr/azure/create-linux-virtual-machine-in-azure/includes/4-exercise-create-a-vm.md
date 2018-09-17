Vårt mål är alltså att flytta en befintlig Linux-server som kör Apache till Azure. Vi börjar med att skapa en Ubuntu Linux-server.

## <a name="create-a-new-linux-virtual-machine"></a>Skapa en ny virtuell Linux-dator

Vi kan skapa en virtuell Linux-dator via Microsoft Azure-portalen, Azure CLI eller Azure PowerShell. Den enklaste metoden när du börjar med Azure är att använda portalen, eftersom den går igenom den information som krävs samt visar tips och användbara meddelanden medan du skapar den virtuella datorn.

1. Logga in på [Microsoft Azure-portalen](https://portal.azure.com?azure-portal=true).

1. Klicka på **Skapa en resurs** längst upp till vänster i Azure Portal.

1. I sökrutan anger du **Ubuntu Server** för att se de olika versionerna som finns tillgängliga. Välj **Ubuntu Server 18.04** från den visade listan.

1. Klicka på knappen **Skapa** för att börja konfigurera den virtuella datorn.

## <a name="configure-the-vm-settings"></a>Konfigurera inställningar för den virtuella datorn

När du skapar en virtuell dator via portalen går du igenom en **guide** där du vägleds genom alla konfigurationsområden. När du klickar på **Nästa** kommer du till nästa konfigurerbara avsnitt. Du kan dock förflytta dig mellan avsnitten när du vill med hjälp av flikarna som visas längs överkanten och som identifierar varje del.

![Skärmbild av Microsoft Azure-portalen som visar första bladet för att skapa en virtuell dator för en Ubuntu Server-dator.](../media/3-azure-portal-create-vm.png)

När du har fyllt i alla obligatoriska alternativ (som anges med röda stjärnor) kan du hoppa över resten av guiden och börja skapa den virtuella datorn med hjälp av knappen **Granska + skapa**, som visas längst ned.

Vi börjar med avsnittet **Grundläggande inställningar**.

### <a name="configure-basic-vm-settings"></a>Konfigurera grundläggande inställningar för en virtuell dator

1. Välj den **prenumeration** som den virtuella datorns körningstimmar ska debiteras mot.

1. Välj **Skapa ny** under **Resursgrupp** och ge resursgruppen namnet **ExerciseResources**.

> [!NOTE]  
> När du ändrar inställningar och flyttar från fälten valideras värdena automatiskt av Azure, och en grön bock visas bredvid dem om de är giltiga. Du kan hålla muspekaren över felindikatorerna om du vill visa mer information om upptäckta problem.

1. I avsnittet **INSTANSINFORMATION** anger du ett namn på den virtuella webbserverdatorn, till exempel **test-web-eus-vm1**. Detta anger miljön (**test**), rollen (**web**), platsen (**East US**), tjänsten (**vm**) och instansnumret (**1**).
    - Det är bästa praxis att standardisera resursnamnen så att du snabbt kan identifiera deras syfte. Namn på virtuella Linux-datorer måste vara mellan 1 och 64 tecken långa, och måste bestå av siffror, bokstäver och bindestreck.

1. Välj en region nära dig; vi har valt **USA, östra**.

1. Låt **Inga** stå kvar under **Tillgänglighetsalternativ**. Det här alternativet säkerställer att den virtuella datorn har hög tillgänglighet genom att gruppera flera virtuella datorer, vilket underlättar hanteringen av planerade eller oplanerade underhållsåtgärder eller avbrott.

1. Se till att avbildningen är inställd på **Ubuntu Server 18.04 LTS**. Du kan öppna listrutan om du vill visa alla tillgängliga alternativ.

1. Fältet **Storlek** kan inte redigeras direkt och har en standardstorlek på **DS2_v3**, vilket är ett av alternativen för generella beräkningar. Det här valet är perfekt för en offentlig webbserver, men klicka gärna på länken **Ändra storlek** om du vill utforska andra storlekar för virtuella datorer. I dialogrutan som visas kan du filtrera baserat på **antal processorer**, **namn** och **disktyp**. Välj samma **DS2_v3**-alternativ, så får du två virtuella processorer med 8 GB RAM.

    > [!TIP]
    > Du kan även dra vyn åt vänster om du vill gå tillbaka till inställningarna för den virtuella datorn, eftersom ett nytt fönster öppnades till höger och hamnade ovanpå inställningsfönstret.

1. I avsnittet **ADMINISTRATÖRSÅTKOMST** väljer du alternativet offentlig SSH-nyckel för **Autentiseringstyp**.

1. Ange ett **användarnamn** som du kommer att använda för att logga in med SSH.

1. Kopiera SSH-nyckeln från filen med din offentliga nyckel och klistra in den i fältet **Offentlig SSH-nyckel**.

> [!IMPORTANT]
> När du kopierar den offentliga nyckeln till Microsoft Azure-portalen ska du se till att inte lägga till några extra blanksteg eller radmatningstecken.

1. I avsnittet **REGLER FÖR INKOMMANDE PORTAR** öppnar du listan och avmarkerar **Inga**. Eftersom det här är en virtuell Linux-dator vill vi kunna komma åt den virtuella datorn via SSH med fjärranslutning. Vid behov kan du bläddra i listan tills du hittar alternativet **ssh (22)**, som du sedan väljer. Som anmärkningen i användargränssnittet anger kan vi även ändra nätverksportarna efter att vi har skapat den virtuella datorn.

    ![Skärmbild av Microsoft Azure-portalen som visar administratörskontot och inställningarna för inkommande port som används vid öppnande av en virtuell Linux-dator för SSH-åtkomst.](../media/3-open-ports.png) 

## <a name="configure-disks-for-the-vm"></a>Konfigurera diskar för den virtuella datorn

1. Gå till avsnittet **Diskar** genom att klicka på **Nästa**.

    ![Skärmbild av Microsoft Azure-portalen som visar avsnittet Diskar i bladet Skapa en virtuell dator.](../media/3-configure-disks.png) 

1. Ange **Premium SSD** som **OS-disktyp**.

1. Använd hanterade diskar så att vi inte behöver arbeta med lagringskonton. Om du vill kan du växla inställningen i det grafiska användargränssnittet för att se skillnaden mellan vilken information Azure behöver.

### <a name="create-a-data-disk"></a>Skapa en datadisk

Som du kanske minns har vi en operativsystemdisk (/dev/sda) och en tillfällig disk (/dev/sdb). Nu ska vi lägga till en datadisk också:

1. Klicka på länken **Create and attach a new disk** (Skapa och koppla en ny disk) i avsnittet **DATADISKAR**.

    ![Skärmbild av Microsoft Azure-portalen som visar bladet Skapa en ny disk.](../media/3-add-data-disk.png) 

1. Du kan använda alla standardvärden: Premium SSD, 1023 GB och **Ingen** (tom disk). Observera att det är här vi skulle kunna använda en ögonblicksbild eller Storage Blob för att skapa en virtuell hårddisk.

1. Klicka på **OK** så att skapa disken skapas och gå sedan tillbaka till avsnittet **DATADISKAR**.

1. En ny disk bör visas på den första raden.

    ![Skärmbild av Microsoft Azure-portalen som visar den nya raden för datadisken vid skapande av en virtuell dator.](../media/3-new-disk.png) 

## <a name="configure-the-network"></a>Konfigurera nätverket

1. Gå till avsnittet **Nätverk** genom att klicka på **Nästa**.

1. I ett produktionssystem där vi redan har andra komponenter vill vi använda ett _befintligt_ virtuellt nätverk. På så sätt kan vår virtuella dator kommunicera med de andra molntjänsterna i vår lösning. Om ingenting har definierats på den här platsen än kan vi skapa det här och konfigurera följande:
    - **Adressutrymme**: Hela det IPV4-utrymme som är tillgängligt för nätverket.
    - **Intervall för undernät**: Det första undernätet som delar in adressutrymmet i underordnade nät – det måste ligga inom det definierade adressutrymmet. När det virtuella nätverket har skapats kan du lägga till fler undernät.

> [!NOTE]  
> Som standard skapar Azure ett virtuellt nätverk, ett nätverksgränssnitt och en offentlig IP-adress för din virtuella dator. Det är inte helt okomplicerat att ändra nätverksalternativen i efterhand när den virtuella datorn redan har skapats. Därför bör du alltid dubbelkolla nätverkstilldelningarna för de tjänster som du skapar i Azure.

## <a name="finish-configuring-the-vm-and-create-the-image"></a>Slutför konfigurationen av den virtuella datorn och skapa avbildningen.

Resten av alternativen har redan lämpliga standardinställningar och du behöver inte ändra dem. Utforska de andra flikarna om du vill. Bredvid respektive alternativ visas en `(i)`-ikon som kan användas för att visa en hjälpbubbla med en beskrivning av alternativet. Beskrivningen är ett bra sätt att lära dig mer om de olika alternativen som du kan använda för att konfigurera den virtuella datorn.

1. Klicka på knappen **Granska + skapa** längst ned på panelen.

1. Systemet validerar alternativen och visar information om den virtuella datorn som skapas.

1. Skapa och distribuera den virtuella datorn genom att klicka på **Skapa**. På instrumentpanelen i Azure ser du den virtuella dator som distribueras. Det här kan ta flera minuter.

Medan den virtuella datorn distribueras ska vi titta närmare på vad vi kan göra med den.
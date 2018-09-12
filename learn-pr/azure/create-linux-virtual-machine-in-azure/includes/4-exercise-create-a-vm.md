Vårt mål är alltså att flytta en befintlig Linux-server som kör Apache till Azure. Vi börjar med att skapa en Ubuntu Linux-server.

## <a name="create-a-new-linux-virtual-machine"></a>Skapa en ny virtuell Linux-dator

Vi kan skapa en virtuell Linux-dator med Azure-portalen, Azure CLI eller Azure PowerShell. Den enklaste metoden när du börjar med Azure är att använda portalen eftersom den går igenom den information som krävs samt visar tips och användbara meddelanden när vi skapar den virtuella datorn.

1. Logga in på [Azure-portalen](https://portal.azure.com?azure-portal=true).

1. Klicka på **Skapa en resurs** uppe till vänster i Azure Portal.

1. I sökrutan anger du **Ubuntu Server** för att se de olika versionerna som är tillgängliga. Välj **Ubuntu Server 18.04** från den visade listan.

1. Klicka på knappen **Skapa** för att börja konfigurera den virtuella datorn.

## <a name="configure-the-vm-settings"></a>Konfigurera inställningarna för den virtuella datorn

När du skapar en virtuell dator på portalen går du igenom en guide som vägleder dig genom alla konfigurationsområden. När du klickar på ”Nästa” kommer du till nästa konfigurerbara avsnitt. Du kan dock förflytta dig mellan avsnitten när du vill med hjälp av flikarna som visas längs överkanten och som identifierar varje del.

![Skapa en virtuell dator på Azure Portal](../media-drafts/3-azure-portal-create-vm.png)

När du har fyllt i alla obligatoriska alternativ (som visas med röd stjärnor) kan du hoppa över resten av guiden och börja skapa den virtuella datorn via knappen **Granska + Skapa** längst ned.

Vi börjar med avsnittet **Grundläggande inställningar**.

### <a name="configure-basic-vm-settings"></a>Konfigurera grundläggande inställningar för en virtuell dator

1. Välj den **prenumeration** som VM-timmar ska debiteras mot.

1. Välj **Skapa nytt** för **Resursgrupp** och ge resursgruppen namnet **ExerciseResources**.

> [!NOTE]
> När du ändrar inställningar och flyttar från fälten valideras värdena automatiskt av Azure och en grön bock visas bredvid dem om de är giltiga. Du kan hovra över felindikatorerna om du vill ha mer information om problem som identifieras.

1. I avsnittet **INSTANSINFORMATION** anger du ett namn på den virtuella webbserverdatorn, ”test-web-eus-vm1”. Detta anger miljön (”test”), rollen (”webb”), platsen (”USA, östra”), tjänsten (”vm”) och instansnummer (”1”).
    - Det är bästa praxis att standardisera resursnamnen så att du snabbt kan identifiera deras syfte. Namnen på virtuella Linux-datorer måste vara mellan 1 och 64 tecken och måste bestå av siffror, bokstäver och bindestreck.

1. Välj en region nära dig; vi har valt USA, östra.

1. Lämna **Tillgänglighetsalternativ** som ”Inga”. Det här alternativet säkerställer att den virtuella datorn har hög tillgänglighet genom att gruppera flera virtuella datorer för att underlätta hanteringen av planerade eller oplanerade underhållsåtgärder eller avbrott.

1. Se till att avbildningen är inställd på ”Ubuntu Server 18.04 LTS”. Du kan öppna listrutan för att visa alla tillgängliga alternativ.

1. Fältet **Storlek** kan inte redigeras direkt och har en standardstorlek på **DS2_v3**, vilket är ett av valen för beräkning för generell användning. Det här valet är perfekt för en offentlig webbserver, men klicka på länken **Ändra storlek** om du vill utforska andra storlekar på virtuella datorer. I dialogrutan som visas kan du filtrera baserat på antal processorer, namn och disktyp. Välj samma **DS2_v3**-val, vilket ger dig två virtuella processorer med 8 GB RAM.

    > [!TIP]
    > Du kan även dra vyn åt vänster för att gå tillbaka till inställningarna för den virtuella datorn eftersom ett nytt fönster öppnades till höger och placerades ovanpå det.

1. I avsnittet **ADMINISTRATÖRSÅTKOMST** väljer du alternativet offentlig SSH-nyckel för **Autentiseringstyp**.

1. Ange ett **användarnamn** som du kommer att använda för att logga in med SSH.

1. Kopiera SSH-nyckeln från filen med din offentliga nyckel och klistra in den i fältet **Offentlig SSH-nyckel**.

> [!IMPORTANT]
> När du kopierar den offentliga nyckeln till Azure-portalen ska du se till att inte lägga till några extra blanksteg eller radmatningstecken.

1. I avsnittet **REGLER FÖR INKOMMANDE PORTAR** öppnar du listan och _avmarkerar_ ”Inga”. Eftersom det här är en virtuell Linux-dator vill vi kunna komma åt den virtuella datorn med hjälp av SSH via en fjärranslutning. Bläddra i listan om det behövs tills du hittar ssh (22) och välj det. Som anmärkningen i användargränssnittet indikerar kan vi även ändra nätverksportarna efter att vi har skapat den virtuella datorn.

    ![Öppna porten för SSH-åtkomst på den virtuella Linux-datorn](../media-drafts/3-open-ports.png)

## <a name="configure-disks-for-the-vm"></a>Konfigurera diskar för den virtuella datorn

1. Gå till avsnittet Diskar genom att klicka på **Nästa**.

    ![Konfigurera diskar för den virtuella datorn](../media-drafts/3-configure-disks.png)

1. Välj ”Premium SSD” för **OS-disktypen**.

1. Använd hanterade diskar så att vi inte behöver arbeta med lagringskonton. Om du vill kan du växla inställningen i det grafiska användargränssnittet för att se skillnaden mellan vilken information Azure behöver.

### <a name="create-a-data-disk"></a>Skapa en datadisk

Som du kanske minns får vi en operativsystemdisk (/dev/sda) och en temporär disk (/dev/sdb). Nu ska vi lägga till en datadisk också.

1. Klicka på länken **Create and attach a new disk** (Skapa och koppla en ny disk) i avsnittet **DATADISKAR**.

    ![Skapa en datadisk för den virtuella datorn på portalen](../media-drafts/3-add-data-disk.png)

1. Du kan använda alla standardvärden: Premium SSD, 1 023 GB och Inget (tom disk). Observera dock att det är här vi skulle kunna använda en ögonblicksbild eller Storage Blob för att skapa en virtuell hårddisk.

1. Klicka på **OK** för att skapa disken och gå tillbaka till avsnittet **DATADISKAR**.

1. En ny disk bör visas på den första raden.

    ![Ny disk på den virtuella datorn](../media-drafts/3-new-disk.png)

## <a name="configure-the-network"></a>Konfigurera nätverket

1. Gå till avsnittet Nätverk genom att klicka på **Nästa**.

1. I ett produktionssystem där vi redan har andra komponenter vill vi använda ett _befintligt_ virtuellt nätverk. På så sätt kan vår virtuella dator kommunicera med de andra molntjänsterna i vår lösning. Om ingenting har definierats på den här platsen än, kan vi skapa det här och konfigurera:

    - **Adressutrymme**: Hela IPV4-utrymmet som är tillgängligt för det här nätverket.
    - **Intervall för undernät**: Det första undernätet som delar in adressutrymmet i underordnade nät – det var giltigt i det definierade adressutrymmet. När det virtuella nätverket har skapats kan du lägga till fler undernät.

> [!NOTE]
> Som standard skapar Azure ett virtuellt nätverk, ett nätverksgränssnitt och en offentlig IP för din virtuella dator. Det är inte helt okomplicerat att ändra nätverksalternativen i efterhand, när den virtuella datorn har skapats. Därför bör du alltid dubbelkolla nätverkstilldelningarna för tjänster som du skapar i Azure.

## <a name="finish-configuring-the-vm-and-create-the-image"></a>Slutför konfigurationen av den virtuella datorn och skapa avbildningen

Resten av alternativen har redan lämpliga standardinställningar och du behöver inte ändra dem. Utforska de andra flikarna om du vill. De enskilda alternativen visas med en `(i)`-ikon som öppnar en användbar hjälpbubbla som beskriver alternativet. På så sätt kan du enkelt lära dig om de olika alternativen som du kan använda för att konfigurera den virtuella datorn.

1. Klicka på knappen **Granska + skapa** längst ned på panelen.

1. Alternativen valideras och visar information om den virtuella datorn som skapas.

1. Skapa och distribuera den virtuella datorn genom att klicka på **Skapa**. På instrumentpanelen i Azure ser du den virtuella dator som distribueras. Det här kan ta flera minuter.

Medan den distribueras ska vi se vad vi kan göra mde den här virtuella datorn.
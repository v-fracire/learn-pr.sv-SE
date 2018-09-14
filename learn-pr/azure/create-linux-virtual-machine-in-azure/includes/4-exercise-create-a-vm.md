Vårt mål är alltså att flytta en befintlig Linux-server som kör Apache till Azure. Vi börjar med att skapa en Ubuntu Linux-server.

## <a name="create-a-new-linux-virtual-machine"></a>Skapa en ny virtuell Linux-dator

Vi kan skapa virtuella Linux-datorer med Azure-portalen, Azure CLI eller Azure PowerShell. Den enklaste metoden när du börjar med Azure är att använda portalen eftersom den går igenom informationen som krävs och ger tips och användbara meddelanden under skapandet:

1. Logga in på [Azure-portalen](https://portal.azure.com?azure-portal=true).

1. Klicka på **skapa en resurs** i det övre vänstra hörnet i Azure Portal.

1. I sökrutan anger du **Ubuntu Server** för att se de olika versionerna som är tillgängliga. Välj **Ubuntu Server 18.04 LTS** från listan över uppvisade.

1. Klicka på knappen **Skapa** för att börja konfigurera den virtuella datorn.

## <a name="configure-the-vm-settings"></a>Konfigurera inställningar för den virtuella datorn

Upplevelse för virtuell dator när du skapar i portalen visas i formatet guiden leder dig igenom alla Konfigurationsområden för den virtuella datorn. Klicka på den **nästa** knappen tar dig till nästa avsnitt för konfigurerbara. Du kan dock förflytta dig mellan avsnitten när du vill med hjälp av flikarna som visas längs överkanten och som identifierar varje del.

![Skärmbild av Azure-portalen som visar första skapa en VM-bladet för en Ubuntu Server-datorn.](../media/3-azure-portal-create-vm.png)

När du fyller i alla obligatoriska alternativ (som identifieras med en röd asterisk) kan du hoppa över resten av guiden och börja skapa den virtuella datorn via den **granska + skapa** längst ned.

Vi börjar med avsnittet **Grundläggande inställningar**.

### <a name="configure-basic-vm-settings"></a>Konfigurera grundläggande inställningar för en virtuell dator

[!include[](../../../includes/azure-sandbox-regions-first-mention-note.md)]

1. Välj den **prenumeration** som den virtuella datorns körningstimmar ska debiteras mot.

1. För **resursgrupp**väljer **Använd befintlig** och välj resursgruppen med namnet  **<rgn>[Sandbox resursgruppens namn]</rgn>**.

    > [!NOTE]
    > När du ändrar inställningar och fliken från varje fält, kommer Azure verifiera varje värde automatiskt och placera en grön bock bredvid den när det är bra. Du kan hovra över felindikatorerna om du vill ha mer information om problem som identifieras.

#### <a name="select-a-location"></a>Välj en plats

<!-- Resource selection -->
[!include[](../../../includes/azure-sandbox-regions-first-mention-note.md)]

1. I den **INSTANSINFORMATION** anger ett namn för din webbserver virtuell dator, till exempel **test-web-eus-vm1**. Detta anger miljön (**testa**), rollen (**web**), plats (**USA, östra**), service (**vm**), och antal (instanser**1**).
    - Det är en bra idé att standardisera ditt resursnamn, så du snabbt kan identifiera deras syfte. Namnen på virtuella Linux-datorer måste vara mellan 1 och 64 tecken och måste bestå av siffror, bokstäver och bindestreck.

1. Välj en region nära dig. Se till att välja en från de tillgängliga platserna som anges ovan.

1. Lämna **tillgänglighetsalternativ** som **ingen**. Det här alternativet används för att se till att Virtuellt datorn med hög tillgänglighet genom att gruppera flera virtuella datorer som en uppsättning utan planerat eller oplanerat underhåll eller avbrott.

1. Kontrollera att avbildningen är inställd på **Ubuntu Server 18.04 LTS**. Du kan öppna listrutan för att visa alla tillgängliga alternativ.

1. Den **storlek** fältet kan inte direkt ändras och har en **DS2_v3** standardstorlek, som är en av de allmänna databehandling val. Det här valet är perfekt för en offentlig webbserver, men klicka på länken **Ändra storlek** om du vill utforska andra storlekar på virtuella datorer. Resulterande dialogrutan kan du filtrera utifrån **antal processorer**, **namn**, och **disktyp**. Välj samma **DS2_v3** föredrar, vilket ger dig två virtuella processorer med 8 GB RAM.

    > [!TIP]
    > Du kan även dra vyn åt vänster för att gå tillbaka till inställningarna för den virtuella datorn eftersom ett nytt fönster öppnades till höger och placerades ovanpå det.

1. I avsnittet **ADMINISTRATÖRSÅTKOMST** väljer du alternativet offentlig SSH-nyckel för **Autentiseringstyp**.

1. Ange ett **användarnamn** som du kommer att använda för att logga in med SSH.

1. Kopiera SSH-nyckeln från filen med din offentliga nyckel och klistra in den i fältet **Offentlig SSH-nyckel**.

> [!IMPORTANT]
> När du kopierar den offentliga nyckeln till Azure-portalen, se till att du inte lägga till några extra blanksteg eller radmatning tecken.

1. I den **regler för INKOMMANDE PORT** avsnittet, öppna listan och avmarkera **inga offentliga inkommande portar**. Eftersom det här är en virtuell Linux-dator vill vi kunna komma åt den virtuella datorn med hjälp av SSH via en fjärranslutning. Bläddra i listan vid behov tills du hittar **SSH (22)** och markera den. Som anmärkningen i användargränssnittet indikerar kan vi även ändra nätverksportarna efter att vi har skapat den virtuella datorn.

    ![Skärmbild av Azure portal som visar administratörskontot och inställningar för inkommande port enligt så att du öppnar en Linux-VM för SSH-åtkomst.](../media/3-open-ports.png)

## <a name="configure-disks-for-the-vm"></a>Konfigurera diskar för den virtuella datorn

1. Klicka på **nästa: diskar >** att flytta till den **diskar** avsnittet.

    ![Skärmbild av Azure-portalen som visar avsnittet diskar för att skapa en VM-bladet.](../media/3-configure-disks.png)

1. Välj **Premium SSD** för den **OS disktyp**.

1. Använd hanterade diskar så att vi inte behöver arbeta med lagringskonton. Om du vill kan du växla inställningen i det grafiska användargränssnittet för att se skillnaden mellan vilken information Azure behöver.

### <a name="create-a-data-disk"></a>Skapa en datadisk

Kom ihåg att vi får en OS-disk (/ dev/sda) och en temporär disk (/ dev/sdb). Lägg till en datadisk:

1. Klicka på länken **Create and attach a new disk** (Skapa och koppla en ny disk) i avsnittet **DATADISKAR**.

    ![Skärmbild av Azure-portalen som visar skapa ett nytt blad för disken.](../media/3-add-data-disk.png)

1. Du kan vidta alla standardvärden: Premium SSD, 1 023 GB och **ingen** (tom disk), även om du se att här där vi kan använda en ögonblicksbild eller Azure Blob storage för att skapa en virtuell Hårddisk.

1. Klicka på **OK** för att skapa disken och gå tillbaka till avsnittet **DATADISKAR**.

1. En ny disk bör visas på den första raden.

    ![Skärmbild av Azure-portalen som visar nya data disk raden för VM-skapandeprocessen.](../media/3-new-disk.png)

## <a name="configure-the-network"></a>Konfigurera nätverket

1. Klicka på **nästa: nätverk >** att flytta till den **nätverk** avsnittet.

1. I ett produktionssystem där vi redan har andra komponenter vill vi använda ett _befintligt_ virtuellt nätverk. På så sätt kan våra virtuella datorn kan kommunicera med andra molntjänster i vår lösning. Om ingenting har definierats på den här platsen än kan vi skapa det här och konfigurera:
    - **Adressutrymme**: den övergripande IPV4-adressutrymmet för det här nätverket.
    - **Intervall för undernät**: det första undernätet att dela upp adressutrymmet - det måste rymmas inom adressutrymmet för definierade. När det virtuella nätverket har skapats kan du lägga till fler undernät.

> [!NOTE]
> Som standard skapar Azure ett virtuellt nätverk, ett nätverksgränssnitt och en offentlig IP för din virtuella dator. Det är inte helt enkelt att ändra alternativ för nätverksfunktioner när du har skapat den virtuella datorn, så du måste alltid kontrollera nätverksallokeringarna på tjänster som du skapar i Azure.

## <a name="finish-configuring-the-vm-and-create-the-image"></a>Slutför konfigurationen av den virtuella datorn och skapa avbildningen

Resten av alternativen har redan lämpliga standardinställningar och du behöver inte ändra dem. Utforska de andra flikarna om du vill. Enskilda alternativ har en `(i)` ikonen bredvid dem som en hjälp för att förklara alternativet visas. Det här är ett bra sätt att lära dig om de olika alternativen som du kan använda för att konfigurera den virtuella datorn:

1. Klicka på knappen **Granska + skapa** längst ned på panelen.

1. Systemet validerar alternativen och visar information om den virtuella datorn som skapas.

1. Skapa och distribuera den virtuella datorn genom att klicka på **Skapa**. På instrumentpanelen i Azure ser du den virtuella dator som distribueras. Det här kan ta flera minuter.

Medan den virtuella datorn distribueras ska vi titta närmare på vad vi kan göra med den.
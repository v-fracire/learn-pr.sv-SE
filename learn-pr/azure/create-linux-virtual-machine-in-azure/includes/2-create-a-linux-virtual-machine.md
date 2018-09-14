Vi har en befintlig webbplats som körs på en lokal Ubuntu Linux-server. Vårt mål är att skapa en Azure-dator (VM) med den senaste Ubuntu-avbildningen och migrera sedan platsen till molnet. I den här enheten lär du dig att de alternativ som du behöver för att utvärdera när du skapar en virtuell dator i Azure.

## <a name="introduction-to-azure-virtual-machines"></a>Introduktion till Azure Virtual Machines

Azure-datorer är en behovsbaserade och skalbara molntjänster resurs. De innehåller processorer, minne, lagring och nätverksresurser. Du kan starta och stoppa virtuella datorer när du vill och hantera dem från Azure portal eller med Azure CLI. Du kan också använda en fjärransluten SSH (Secure Shell) för att ansluta direkt till virtuell dator som körs och köra kommandon som om du var på en lokal dator.

### <a name="running-linux-in-azure"></a>Köra Linux i Azure

Att skapa Linux-baserade virtuella datorer i Azure är enkelt. Microsoft samarbetar med framstående Linux-leverantörer för att säkerställa att deras distributioner optimeras för Azure-plattformen. Du kan skapa virtuella datorer från färdiga avbildningar för en mängd olika populära Linux-distributioner, till exempel Ubuntu, SUSE och Red Hat eller skapa din egen Linux-distribution ska köras i molnet.

## <a name="creating-an-azure-vm"></a>Skapa en virtuell Azure-dator

Virtuella datorer kan definieras och distribueras på Azure på flera olika sätt: Azure-portalen, ett skript (med Azure CLI eller Azure PowerShell) eller en Azure Resource Manager-mall. I samtliga fall måste du ange flera typer av information som vi strax återkommer.

Azure Marketplace innehåller också förkonfigurerade avbildningar som innehåller en OS- och communitydrivna favoritprogram med verktyg som installerats för specifika scenarier.

![Skärmbild av Azure-portalen som visar alternativ för flera virtuella datorer i Azure Marketplace.](../media/2-marketplace-vm-choices.png)

## <a name="resources-used-in-a-linux-vm"></a>Resurser som används på en virtuell Linux-dator

När du skapar en virtuell Linux-dator i Azure skapar du även resurser för den virtuella datorns värdmiljö. Dessa resurser fungerar tillsammans för att virtualisera en dator och köra operativsystemet Linux. Dessa måste finnas (och väljas under skapandet av VM), eller kommer att skapas med den virtuella datorn:

- En virtuell dator som tillhandahåller CPU- och minnesresurser
- Ett Azure Storage-konto för att lagra de virtuella hårddiskarna
- Virtuella diskar med operativsystem, program och data
- Ett virtuellt nätverk för att ansluta den virtuella datorn till andra Azure-tjänster eller din lokala maskinvara
- Ett nätverksgränssnitt för att kommunicera med det virtuella nätverket
- En valfri offentlig IP-adress så att du kommer åt den virtuella datorn

Liksom andra Azure-tjänster behöver du en **resursgrupp** som innehåller den virtuella datorn (och du kan också gruppera resurserna för administration). När du skapar en ny virtuell dator kan du antingen använda en befintlig resursgrupp eller skapa en ny.

## <a name="choose-the-vm-image"></a>Välja VM-avbildning

Att välja avbildning är ett av de första och viktigaste besluten när du skapar en virtuell dator. En avbildning är mallen som används till att skapa den virtuella datorn. Dessa mallar har ett operativsystem och ofta annan programvara, som utvecklingsverktyg eller värdmiljöer för webben.

Allt som kan installeras på en dator kan ingå i en avbildning. Du kan skapa en virtuell dator från en avbildning som är förkonfigurerad att exakt de uppgifter som du behöver, till exempel som är värd för en webbapp on Apache HTTP-Server.

> [!TIP]
> Du kan också skapa och ladda upp anpassade diskavbildningar.

## <a name="sizing-your-vm"></a>Ange storlek för den virtuella datorn

En virtuell dator har en viss mängd minne och processorkraft precis som en fysisk dator. Azure erbjuder virtuella datorer i olika storlekar till olika priser. Storleken som du väljer avgör Virtuellt datorns bearbetningskraft, minne och maximal lagringskapacitet.

> [!WARNING]
> Det finns kvotgränser för varje prenumeration som kan påverka skapandet av den virtuella datorn. Som standard kan du inte ha mer än sammanlagt 20 virtuella _kärnor_ på dina virtuella datorer i en region. Du kan dela upp virtuella datorer i olika regioner eller skicka in en [onlinebegäran](https://docs.microsoft.com/azure/azure-supportability/resource-manager-core-quotas-request) för att höja gränsen.

Storlekar på virtuella datorer grupperas i kategorier, från B-serien för grundläggande testning och upp till H-serien för stora databehandlingsuppgifter. Du bör välja storlek av virtuell dator utifrån den arbetsbelastning du vill utföra. Det går att ändra storlek på en virtuell dator efter att det har skapats, men den virtuella datorn måste stängas först. Därför är det bäst att ändra storlek på den på rätt sätt från början om möjligt.

#### <a name="here-are-some-guidelines-based-on-the-scenario-you-are-targeting"></a>Här följer några riktlinjer som baserat på scenariot som du vill skicka meddelanden

| Vad gör du? | Överväg dessa storlekar
|-------|------------------|
| **Allmänt bruk databehandling/web**: testning och utveckling, små till medelstora databaser eller med låg till medelhög trafik webbservrar. | B, Dsv3, Dv3, DSv2, Dv2 |
| **Tung databaserad uppgifter**: webbservrar med medelhög trafik, nätverkstillämpningar, batchprocesser och programservrar. | Fsv2, Fs, F |
| **Hög minnesanvändning**: relationsdatabasservrar, mellanstora till stora cacheminnen och minnesinterna analyser. | Esv3, Ev3, M, GS, G, DSv2, Dv2 |
| **Datalagring och bearbetning av**: stordata, SQL och NoSQL-databaser som behöver högt diskgenomflöde och I/O. | Ls |
| **Tung grafikrendering** eller videoredigering samt modellträning och inferensjobb (ND) med djupinlärning. | NV, NC, NCv2, NCv3, ND |
| **Databehandling med höga prestanda (HPC)**: dina arbetsbelastningar måste de snabbaste och mest kraftfulla CPU virtuella datorerna med nätverksgränssnitt för valfritt högt dataflöde. | H |

## <a name="choosing-storage-options"></a>Välja lagringsalternativ

Nästa uppsättning beslut är uppbyggd kring lagring. Först kan du välja diskteknik. Du kan välja en traditionell skivbaserad hårddisk (HDD) eller en modernare Solid State-hårddisk (SSD). Precis som maskinvara som du köper kostar SSD-lagring mer men ger bättre prestanda.

> [!TIP]
> Det finns två nivåer av SSD-lagring: standard och premium. Välj Standard SSD-diskar om du har normala arbetsbelastningar men vill ha bättre prestanda. Välj Premium SSD-diskar om du har I/O-intensiva arbetsbelastningar eller verksamhetskritiska system som behöver bearbeta data mycket snabbt.

### <a name="mapping-storage-to-disks"></a>Mappa lagring till diskar

Azure använder virtuella hårddiskar (VHD) som representerar fysiska diskar för den virtuella datorn. Virtuella hårddiskar replikerar det logiska formatet och data hos en diskenhet men lagras som sidblobar på ett Azure Storage-konto. Du kan välja på basis av per disk vilken typ av lagring bör använda (SSD och HDD). Det gör att du kan kontrollera varje disks prestanda, troligen baserat på den I/O du planerar att utföra på den.

Som standard skapas två virtuella hårddiskar (VHD) för den virtuella Linux-datorn:

1. Den **operativsystemdisken**: Detta är den primära enheten och den har en maxkapacitet på 2 048 GB. Den är som standard märkt `/dev/sda`.

1. En **temporär disk**: Detta ger tillfällig lagring för Operativsystemet eller alla appar. På virtuella Linux-datorer är disken `/dev/sdb` och formateras och monteras i `/mnt` av Azure Linux-agenten. Dess storlek baseras på storleken på den virtuella datorn för att lagra växlingsfilen.

> [!WARNING]
> Den tillfälliga disken är inte beständig. Du ska bara skriva data på den här disken som inte är kritiska för systemet.

#### <a name="what-about-data"></a>Hur är det med data?

Du kan lagra data på den primära enheten tillsammans med operativsystemet men det är bättre att skapa dedikerade _datadiskar_. Du kan skapa och koppla ytterligare diskar till den virtuella datorn. Varje disk kan lagra upp till 4 095 GB data, där den maximala mängden lagring bestäms av den VM-storlek du väljer.

> [!NOTE]
> En intressant funktion är möjligheten att skapa en VHD-avbildning från en riktig disk. På så sätt kan du enkelt migrera _befintliga_ information från en lokal dator till molnet.

### <a name="unmanaged-vs-managed-disks"></a>Ohanterade respektive hanterade diskar

Du slutliga lagringsvalet du gör är om du ska använda **ohanterade** eller **hanterade** diskar.

Med ohanterade diskar ansvarar du för lagringskontona som används för att lagra de virtuella hårddiskarna som motsvarar diskarna på din virtuella dator. Du betalar lagringskontoavgifter för den mängd utrymme du använder. Ett lagringskonto har en fast gräns på 20 000 I/O-åtgärder per sekund. Det betyder att ett enda lagringskonto kan hantera 40 virtuella standardhårddiskar vid maxkapacitet. Om du behöver skala ut behöver du fler än ett lagringskonto, något som kan vara komplicerat.

Hanterade diskar är den nyare och rekommenderade disklagringsmodellen. De löser elegant den här komplexiteten genom att lägga ansvaret för hanteringen av lagringskonton på Azure. Du anger typ av disk (Premium eller Standard) och storleken på disken och skapar och hanterar både disken Azure _och_ lagring som används. Du behöver inte bekymra dig om begränsningar i lagringskontot, vilket gör det lättare att skala ut. De har också andra fördelar:

- **Ökad tillförlitlighet**: Azure ser till att virtuella hårddiskar som är associerade med hög tillförlitlighet virtuella datorer kommer att placeras i olika delar av Azure Storage och tillhandahåller liknande nivåer av återhämtning.
- **Bättre säkerhet**: Hanterade diskar är riktiga hanterade resurser i resursgruppen. Det betyder att de kan använda rollbaserad åtkomstkontroll för att begränsa vilka som kan arbeta med VHD-data.
- **Stöd för ögonblicksbilder**: Ögonblicksbilder kan användas för att skapa en skrivskyddad kopia av en VHD. Du måste stänga den ägande virtuella datorn, men skapa ögonblicksbilden tar bara några sekunder. När det är klart kan du sätter på den virtuella datorn och använda ögonblicksbilden för att skapa en duplicerad virtuell dator för att felsöka ett problem för produktion eller Återställ den virtuella datorn till punkten som ögonblicksbilden togs.
- **Stöd för säkerhetskopiering**: hanterade diskar kan automatiskt säkerhetskopieras till andra regioner för haveriberedskap med Azure Backup utan att påverka tjänsten för den virtuella datorn.

## <a name="network-communication"></a>Nätverkskommunikation

Virtuella datorer kommunicerar med externa resurser med hjälp av ett virtuellt nätverk. Det virtuella nätverket representerar ett privat nätverk i en region som dina resurser kommunicerar i. Ett virtuellt nätverk är precis som de nätverk du hanterar lokalt. Du kan dela upp dem i undernät för att isolera resurser, ansluta dem till andra nätverk (inklusive dina lokala nätverk) och tillämpa trafikregler för att styra inkommande och utgående anslutningar.

### <a name="planning-your-network"></a>Planera nätverket

När du skapar en ny virtuell dator har du möjlighet att skapa ett nytt virtuellt nätverk eller använda ett befintligt i din region.

Det är enkelt att låta Azure skapa nätverket tillsammans med den virtuella datorn men det passar troligen inte de flesta scenarier. Det är bättre att planera dina nätverkskrav _direkt_ för alla komponenter i din arkitektur och skapa VNet-strukturen separat. Sedan skapa de virtuella datorerna och placera dem i den redan skapat virtuella nätverken. Vi tittar mer på virtuella nätverk senare i den här modulen.

Innan vi skapar en virtuell dator måste vi bestämma hur vi vill administrera den virtuella datorn. Nu ska vi titta på de olika alternativen.
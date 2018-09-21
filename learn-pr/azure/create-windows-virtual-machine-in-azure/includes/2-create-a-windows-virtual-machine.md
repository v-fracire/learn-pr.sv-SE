Ditt företag har valt att hantera data som video från sina trafikkameror med virtuella datorer i Azure. För att köra de olika codecarna måste vi först skapa de virtuella datorerna. Vi måste också ansluta till och interagera med de virtuella datorerna. I den här kursdelen lär du dig hur du skapar en virtuell dator på Azure Portal. Du ska konfigurera den virtuella datorn för fjärråtkomst, välja en VM-avbildning och välja ett lämpligt lagringsalternativ.

## <a name="introduction-to-windows-virtual-machines-in-azure"></a>Introduktion till virtuella Windows-datorer i Azure

Virtuella datorer i Azure är databehandlingsresurser som kan skalas om på begäran. De påminner om virtuella datorer i Windows Hyper-V. De har processor, minne, lagring och nätverksresurser. Du kan starta och stoppa de virtuella datorerna precis som i Hyper-V, och du kan hantera dem från Azure Portal eller via Azure CLI. Du kan också använda en RDP-klient (Remote Desktop Protocol) och ansluta direkt till Windows-skrivbordet, och använda den virtuella datorn som om du var inloggad på en lokal Windows-dator.

## <a name="creating-an-azure-vm"></a>Skapa en virtuell Azure-dator

Virtuella datorer kan definieras och distribueras i Azure på flera olika sätt: Azure Portal, ett skript (med Azure CLI eller Azure PowerShell) eller via en Azure Resource Manager-mall. I samtliga fall behöver du ange flera typer av information, som vi snart går igenom.

Azure Marketplace har även förkonfigurerade avbildningar som innehåller både ett operativsystem och populära programvaruverktyg som installeras för specifika scenarier.

![Skärmbild som visar listan över virtuella datorer i Azure Marketplace.](../media/2-marketplace-vm-choices.png)

## <a name="resources-used-in-a-windows-vm"></a>Resurser som används på en virtuell Windows-dator

När du skapar en virtuell Windows-dator i Azure skapar du även resurser för den virtuella datorns värdmiljö. Dessa resurser fungerar tillsammans för att virtualisera en dator och köra operativsystemet Windows. Dessa måste redan finnas (och väljas när den virtuella datorn skapas). Annars skapas de samtidigt som den virtuella datorn.

- En virtuell dator som tillhandahåller CPU- och minnesresurser.
- Ett Azure Storage-konto där de virtuella hårddiskarna lagras.
- Virtuella diskar med operativsystem, program och data.
- Ett virtuellt nätverk för att ansluta den virtuella datorn till andra Azure-tjänster eller din egen lokala maskinvara.
- Ett nätverksgränssnitt för att kommunicera med det virtuella nätverket.
- En offentlig IP-adress så att du kan komma åt den virtuella datorn. Detta är valfritt.

Liksom andra Azure-tjänster behöver du en **resursgrupp** som innehåller den virtuella datorn (och som eventuellt grupperar dessa resurser för enklare administration). När du skapar en ny virtuell dator kan du antingen använda en befintlig resursgrupp eller skapa en ny.

## <a name="choose-the-vm-image"></a>Välja VM-avbildning

Att välja avbildning är ett av de första och viktigaste besluten när du skapar en virtuell dator. En avbildning är mallen som används till att skapa den virtuella datorn. Dessa mallar har ett operativsystem och ofta annan programvara, som utvecklingsverktyg eller värdmiljöer för webben.

Alla program som stöds av datorn kan ingå i VM-avbildningen. Du kan skapa en virtuell dator från en avbildning som är förkonfigurerad efter just dina behov, till exempel att köra en ASP.Net Core-app.

> [!TIP]
> Du kan även skapa och ladda upp egna avbildningar. I dokumentationen finns mer information.

## <a name="sizing-your-vm"></a>Ange storlek för den virtuella datorn
En virtuell dator har en viss mängd minne och processorkraft precis som en fysisk dator. Azure erbjuder virtuella datorer i olika storlekar till olika priser. Den storlek du väljer bestämmer den virtuella datorns bearbetningskraft, minne och maximala lagringskapacitet.

> [!WARNING]
> Det finns kvotgränser för varje prenumeration som kan påverka skapandet av den virtuella datorn. Som standard kan du inte ha mer än sammanlagt 20 virtuella _kärnor_ på dina virtuella datorer i en region. Du kan dela upp virtuella datorer i olika regioner eller lämna en [onlinebegäran](https://docs.microsoft.com/azure/azure-supportability/resource-manager-core-quotas-request) om att höja gränsen.

Storlekar på virtuella datorer grupperas i kategorier, från B-serien för grundläggande testning och upp till H-serien för stora databehandlingsuppgifter. Du bör välja storlek av virtuell dator utifrån den arbetsbelastning du vill utföra. Du kan ändra storlek på en virtuell dator när den har skapats, men eftersom den virtuella datorn i så fall måste stoppas först är det bäst att välja en lämplig storlek från början, om det är möjligt.

#### <a name="here-are-some-guidelines-based-on-the-scenario-you-are-targeting"></a>Här är några riktlinjer som baseras på scenariot du arbetar med.

| Vad gör du? | Överväg dessa storlekar
|-------|------------------|
| **Allmän databehandling/webb** Testning och utveckling, små till medelstora databaser eller webbservrar med låg till medelhög trafik. | B, Dsv3, Dv3, DSv2, Dv2 |
| **Tunga databehandlingsuppgifter** Webbservrar med medelhög trafik, nätverkstillämpningar, batchprocesser och programservrar. | Fsv2, Fs, F |
| **Stor minnesanvändning** Relationsdatabasservrar, medelstora till stora cacheminnen och minnesinterna analyser. | Esv3, Ev3, M, GS, G, DSv2, Dv2 |
| **Lagring och bearbetning av data** Stordata-, SQL- och NoSQL-databaser som behöver högt diskgenomflöde och I/O. | Ls |
| **Tung grafikrendering** eller videoredigering samt modellträning och inferensjobb (ND) med djupinlärning. | NV, NC, NCv2, NCv3, ND |
| **Databehandling med höga prestanda (HPC)** Om du behöver virtuella datorer med de snabbaste och mest kraftfulla processorerna med valfria nätverksgränssnitt för högt genomflöde. | H |

## <a name="choosing-storage-options"></a>Välja lagringsalternativ

Nästa uppsättning beslut handlar om lagring. Först kan du välja diskteknik. Du kan välja en traditionell skivbaserad hårddisk (HDD) eller en modernare Solid State-hårddisk (SSD). Precis som maskinvara som du köper kostar SSD-lagring mer men ger bättre prestanda.

> [!TIP]
> Det finns två nivåer av SSD-lagring: standard och premium. Välj Standard SSD-diskar om du har normala arbetsbelastningar men vill ha bättre prestanda. Välj Premium SSD-diskar om du har I/O-intensiva arbetsbelastningar eller verksamhetskritiska system som behöver bearbeta data mycket snabbt.

### <a name="mapping-storage-to-disks"></a>Mappa lagring till diskar

Azure använder virtuella hårddiskar (VHD) för att representera fysiska diskar för den virtuella datorn. Virtuella hårddiskar replikerar det logiska formatet och data hos en diskenhet men lagras som sidblobbar på ett Azure Storage-konto. För varje disk kan du välja vilken typ av lagring den ska använda (SSD eller HDD). Det gör att du kan kontrollera varje disks prestanda, troligen baserat på den I/O du planerar att utföra på den.

Som standard skapas två virtuella hårddiskar (VHD) för den virtuella Windows-datorn:

1. **Operativsystemdisken**. Det här är din primära enhet eller enheten C: och har en maxkapacitet på 2 048 GB.

1. En **tillfällig disk**. En tillfällig disk tillhandahåller temporär lagring för operativsystemet eller appar. Som standard konfigureras den som enhet D: och diskens storlek baseras på den virtuella datorns storlek, vilket gör det till en perfekt plats för Windows-växlingsfilen.

> [!WARNING]
> Den tillfälliga disken är inte beständig. Du bör endast skriva data till den här disken som du är beredd att förlora när som helst.

#### <a name="what-about-data"></a>Hur är det med data?

Du kan lagra data på enhet C: tillsammans med operativsystemet men det är bättre att skapa dedikerade _datadiskar_. Du kan skapa och koppla ytterligare diskar till den virtuella datorn. Varje disk kan lagra upp till 4 095 GB data, där den maximala mängden lagring bestäms av den VM-storlek du väljer.

> [!NOTE]
> En intressant funktion är möjligheten att skapa en VHD-avbildning från en riktig disk. Då kan du enkelt migrera _befintlig_ information från en lokal dator till molnet.

### <a name="unmanaged-vs-managed-disks"></a>Ohanterade och hanterade diskar

Det sista lagringsvalet som du behöver göra är huruvida du ska använda **ohanterade** eller **hanterade** diskar.

Med ohanterade diskar ansvarar du för lagringskontona som används för att lagra de virtuella hårddiskarna som motsvarar diskarna på din virtuella dator. Du betalar lagringskontoavgifter för den mängd utrymme du använder. Ett lagringskonto har en fast gräns på 20 000 I/O-åtgärder per sekund. Det betyder att ett enda lagringskonto kan hantera 40 virtuella standardhårddiskar vid maxkapacitet. Om du behöver skala ut behöver du fler än ett lagringskonto, något som kan vara komplicerat.

Hanterade diskar är den nyare och rekommenderade disklagringsmodellen. De löser elegant den här komplexiteten genom att lägga ansvaret för hanteringen av lagringskonton på Azure. Du anger disktypen (Premium eller Standard) och storleken på disken så skapar och hanterar Azure både disken _och_ den lagring som disken använder. Du behöver inte bekymra dig om begränsningar i lagringskontot, vilket gör det lättare att skala ut. De har också andra fördelar:

- **Ökad tillförlitlighet**: Azure ser till att virtuella hårddiskar associerade med virtuella datorer med hög tillförlitlighet placeras i olika delar av Azure Storage för att ge motsvarande återhämtningsnivåer.
- **Bättre säkerhet**: Hanterade diskar är hanterade resurser i resursgruppen. Det betyder att de kan använda rollbaserad åtkomstkontroll för att begränsa vem som kan arbeta med VHD-data.
- **Stöd för ögonblicksbilder**: Ögonblicksbilder kan användas för att skapa en skrivskyddad kopia av en VHD. Du måste stänga av den ägande virtuella datorn men att skapa ögonblicksbilden tar bara några sekunder. När det är gjort kan du slå på den virtuella datorn och använda ögonblicksbilden till att skapa en dubblett av den virtuella datorn för att felsöka ett produktionsproblem eller återställa den virtuella datorn till den tidpunkt då ögonblicksbilden togs.
- **Stöd för säkerhetskopiering**: Hanterade diskar kan automatiskt säkerhetskopieras till olika regioner för haveriberedskap med Azure Backup utan att påverka den virtuella datorns tjänst.

## <a name="network-communication"></a>Nätverkskommunikation

Virtuella datorer kommunicerar med externa resurser med hjälp av ett virtuellt nätverk. Det virtuella nätverket representerar ett privat nätverk i en region som dina resurser kommunicerar i. Ett virtuellt nätverk är precis som de nätverk du hanterar lokalt. Du kan dela upp dem i undernät för att isolera resurser, ansluta dem till andra nätverk (inklusive dina lokala nätverk) och tillämpa trafikregler för att styra inkommande och utgående anslutningar.

### <a name="planning-your-network"></a>Planera nätverket

När du skapar en ny virtuell dator kan du välja att skapa ett nytt virtuellt nätverk eller använda ett befintligt virtuellt nätverk i din region.

Det är enkelt att låta Azure skapa nätverket tillsammans med den virtuella datorn men det passar inte alla scenarier. Det är bättre att planera dina nätverkskrav _i förväg_ för alla komponenter i arkitekturen och att skapa strukturen för det virtuella nätverket separat. Skapa sedan de virtuella datorerna och placera dem i de redan skapade virtuella nätverken.

Vi ska titta närmare på virtuella nätverk lite senare i den här modulen. Nu ska vi dra nytta av det vi lärt oss och skapa en virtuell dator i Azure.
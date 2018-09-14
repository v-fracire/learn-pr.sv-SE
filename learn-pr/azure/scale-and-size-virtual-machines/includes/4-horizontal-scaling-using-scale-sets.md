Du kan få resurserna du behöver med hjälp av en stor virtuell dator eller flera små virtuella datorer med en belastningsutjämnare distribueras begäranden mellan de virtuella datorerna.

VM-pool har bra fördelen att du kan lägga till eller ta bort virtuella datorer snabbt när behoven ändras. Den här strategin är användbart för att hantera oväntade toppar i efterfrågan Sonens företagets för scenariot. Du kan lägga till virtuella datorer till poolen när efterfrågan ökar och ta bort dem när begäran tillbaka till normal. Poolen får du även redundans; Om en virtuell dator misslyckas, kan de andra kan fortsätta att hantera förfrågningar utan avbrott i tjänsten.

I det här avsnittet visas hur du etablerar flera virtuella datorer med skalningsuppsättningar och automatiskt lägga till och ta bort instanser som svar på förändrade behov. 

## <a name="what-is-horizontal-scaling"></a>Vad är horisontell skalning?

*Horisontell skalning* är en process för att lägga till eller ta bort virtuella datorer från en pool att justera mängden tillgängliga resurser. Lägger till datorer kallas _skala ut_, och tar bort datorer kallas _skalning i_. Lösningar som använder horisontell skalning omfattar en belastningsutjämnare eller gateway distribueras begäranden mellan de virtuella datorerna i poolen. Följande bild visar ett exempel på hur du ändrar antalet instanser av virtuella datorer.

![En bild som visar att skala ut resurser för att hantera begäran och skalning i resurser för att minska kostnaderna.](../media/4-ScaleInOut.png)

Den här tekniken fungerar bäst för program som kan köras på flera identiska servrar. Exempel: du kan duplicera webbservern och webbsidor på flera virtuella datorer och de kommer alla ger samma svar oavsett vilken server tar emot begäran. En virtuell dator som kör serverdelsdatabasen kan å andra sidan inte en perfekt kandidat eftersom när du kör flera kopior av databasen måste vissa arbete för att synkronisera kopiorna.

## <a name="what-is-a-scale-set"></a>Vad är skalningsuppsättningar?

En *skalningsuppsättning* är en uppsättning identiska virtuella datorer, en belastningsutjämnare eller gateway distribueras begäranden och en valfri uppsättning regler som styr när virtuella datorer läggs till eller tas bort från poolen. Här är innebär ”identiska” att varje virtuell dator i uppsättningen skapas med hjälp av samma avbildning och har samma storlek.

Har du viss flexibilitet i hur en ny virtuell dator konfigureras med programvaran du behöver. Du kan börja med en fördefinierad avbildning för den grundläggande OS och använda skript för att installera eller kopiera filer automatiskt när Operativsystemet har konfigurerats. Du kan också skapa en anpassad virtuell datoravbildning med operativsystemet och din programvara installerad.

## <a name="how-to-distribute-requests"></a>Hur du distribuerar begäranden

Du kan använda antingen en belastningsutjämnare eller en Programgateway för att distribuera förfrågningar till de Virtuella datorinstanserna i en skalningsuppsättning.

En Azure load balancer fungerar på OSI-nivå 4 (TCP och UDP) och dirigerar trafik baserat på källans IP-adress och port som kombineras med IP-måladress och port. Det kan ge tillhörighet, där trafiken från samma IP-källadress dirigeras till samma mål-servern så att en klientsession. Belastningsutjämnaren har också en avsökningsmekanism för hälsotillstånd som fastställer tillgängligheten för server-instanser. Om en virtuell dator blir svarar inte på hälsoavsökningen undviker belastningsutjämnaren routning alla nya anslutningar till den datorn.

En application gateway fungerar på OSI-nivå 7 (application layer). Till exempel om dina virtuella datorer kör en webbserver, kan sedan gatewayen använda den begärda Webbadressen för att utföra routning. Det innebär att du kan vidarebefordra begäranden med `*/customers*` i URL-Adressen till en pool av servrar och begäranden med `*/partners*` i URL-Adressen till en annan pool. Application gateway kan också tillhandahålla HTTP till HTTPS-omdirigering, Secure Sockets Layer (SSL)-avslutning till att minska bearbetning av kravet på de virtuella datorerna för kryptering och en brandvägg för webbaserade program (WAF) som använder regler för att identifiera kända web utnyttjar och förhindra att dessa begäranden når webbservrarna.

## <a name="what-is-autoscaling"></a>Vad är automatisk skalning?

_Automatisk skalning_ är automatiskt skala in eller ut baserat på en uppsättning regler. Reglerna kan utlösas av datorn belastning eller ett schema. Följande bild visar hur funktion skala automatiskt hanterar instanser för att hantera belastningen.

![En bild som visar hur funktioner för autoskalning övervakar CPU-nivåer av en pool med virtuella datorer och lägger till instanser när CPU-användningen är över tröskeln.](../media/4-autoscale.png)

Om du vill aktivera automatisk skalning för en skalningsuppsättning, måste du skapa en autoskalningsprofil. Profilen som definierar det lägsta och högsta antalet Virtuella datorinstanser för uppsättningen och skalar reglerna. Regler för automatisk skalning har följande element:

* Måttkälla - källan för information eller data som utlöser regeln för automatisk skalning. Det finns fyra alternativ:
  * *Aktuella skalningsuppsättning* ger värdbaserade mått som inte kräver någon ytterligare agenter.
  * *Storage-konto*. Azure-diagnostiktillägget skriver prestandamått till Azure Storage. De här måtten används för att utlösa regler för automatisk skalning.
  * *Azure Service Bus-kö* ange programbaserade eller andra Azure Service Bus-meddelanden för att utlösa automatisk skalning.
  * *Azure Application Insights* använder en instrumentationspaket som måste installeras i programmet som körs på skalningsuppsättningen att strömma mått data direkt från programmet.
* Villkor – det här är den specifika mått som du vill använda för att utlösa en regel för automatisk skalning. Om du använder värdbaserade mått kan omfatta detta aspekter, till exempel CPU-användning, mängden nätverkstrafik, diskåtgärder eller CPU-krediter. Du kan till exempel konfigurera en regel för att skala ut om disken skrivåtgärder per sekund överskrider ett tröskelvärde. Med hjälp av Azure-diagnostiktillägget eller Application Insights kan du använda alla tillgängliga mått för att utlösa regeln, men kräver konfiguration av lämplig agent.
* Sammansättningstyp – detta anger hur du vill mäta måttdata och kommer vara något av följande alternativ:
  * Medel
  * Minimum
  * Maximal
  * Totalt
  * Senaste
  * Antal
* Operator - operatorn anger hur ett mått får inte vara samma med ett angivet tröskelvärde som utlöser åtgärden regler. Detta är särskilt viktigt när du identifierar om regeln skalas in eller ut. Operatörer kan vara:
  * Större än
  * Större än eller lika med
  * Mindre än
  * Mindre än eller lika med
  * Lika med
  * Inte lika med
* Åtgärd - detta avgör hur antalet instanser kommer att ändras när regeln utlöses. Följande åtgärder är tillgängliga:
  * *Öka antal med* ett fast antal virtuella datorer.
  * *Öka procent med* procentandelar av befintliga instanser.
  * *Öka antalet till* ett visst antal virtuella datorer.
  * *Minska antal med* ett fast antal virtuella datorer.
  * *Minska procent med* procentandelar av befintliga instanser.
  * *Minska antal till* ett visst antal virtuella datorer.

Du kan också skapa regler för automatisk skalning som utlöser enligt ett schema. Du kan till exempel definiera en regel som skalas ut på morgonen när du vet att begäran är hög och sedan skalas in efter lunch när efterfrågan vanligtvis minskar.

## <a name="how-to-create-a-scale-set"></a>Så här skapar du en skalningsuppsättning

Du kan skapa en skalningsuppsättning med Azure-portalen, Azure PowerShell eller Azure CLI.

### <a name="portal"></a>Portalen

Om du använder Azure-portalen för att skapa skalningsuppsättningen, anger du avbildningen av operativsystemet ska användas för de virtuella datorerna och hur många Virtuella datorinstanser att skapa vid start. Du kan också ange storleken på virtual machine för varje instans och om du vill använda Azure load balancer eller application gateway för belastningsutjämning. Om du väljer en belastningsutjämnare, skapa en standard-hälsoavsökning på port 80 för den med portalen.

### <a name="powershell"></a>PowerShell

Du kan skapa en VM-skalningsuppsättning med den **New-AzureRmVmss** PowerShell-cmdlet. Denna cmdlet kan skapa en ny skalningsuppsättning, en belastningsutjämnare och kontrollera IP-adress och tilldelningar för virtuellt nätverk. Om inte inställningar har angetts i cmdlet, **New-AzureRmVmss** kommer att använda följande standardinställningar:

* Skapa två instanser av virtuella datorer
* Använda Windows Server 2016 Datacenter-avbildning
* Använd Standard DS1_v2 VM-storlek
* Skapa en lastbalanserare
* Skapa regler för belastningsutjämnaren för port 3389 och 5985 för Windows, port 22 för Linux

**New-Azurermvmsssupport** skapar inte en hälsoavsökning för belastningsutjämnaren. Det bästa sättet är att skapa detta med hjälp av **Add-AzureRmLoadBalancerProbeConfig** när du har skapat skalningsuppsättningen.

Horisontell skalning med skalningsuppsättningar ger dig flera servrar för att köra programmet. Med hjälp av flera servrar kan du hantera hög läser in och säkerställer att dina tjänster hålls tillgängliga även om en server kraschar. Du kan lägga till automatisk skalning till dina skalningsuppsättningar, så att systemet automatiskt anpassar sig till oväntade förändringar av behov.
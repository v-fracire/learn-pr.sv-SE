Research-teamet har samlat in enorma mängder bilddata som kan leda till en identifiering på Mars. De behöver för att utföra beräkningsmässigt intensiv databehandling men inte har utrustningen som ska utföra arbetet. Låt oss se varför Azure är ett bra alternativ för att göra data-analys.

## <a name="what-is-azure-compute"></a>Vad är Azure Compute?
Azure-instanser är en databehandling tjänst på begäran för att köra molnbaserade program. Den innehåller resurser som processorer med flera kärnor och superdatorer som du får tillgång till via virtuella datorer och containrar. Det ger också databearbetning utan server för att köra appar utan infrastruktur installation eller konfiguration. Resurserna är tillgängliga på begäran och kan vanligtvis skapas på bara några minuter eller till och med sekunder. Du betalar bara för de resurser du använder och endast så länge du använder dem.

Det finns tre vanliga tekniker för databehandling i Azure:

- Virtuella datorer
- Containrar
- Serverfri databehandling

## <a name="what-are-virtual-machines"></a>Vad är virtuella datorer?

**Virtuella datorer**, eller virtuella datorer, programvara emuleringar för fysiska datorer. De innehåller en virtuell processor, minne, lagring och nätverksresurser. De är värd för ett operativsystem, och du kan installera och köra programvara precis som på en fysisk dator. Och med hjälp av en fjärrskrivbordsklient som du kan använda och kontrollera den virtuella datorn som om du satt framför den.

### <a name="virtual-machines-in-azure"></a>Virtuella datorer i Azure

Du kan skapa och vara värd för virtuella datorer i Azure. Normalt nya virtuella datorer skapas och debiteras i minuter genom att välja en förkonfigurerad virtuell datoravbildning.

Välja en bild är en av de viktigaste beslut du måste ta när du skapar en virtuell dator. En avbildning är mallen som används till att skapa den virtuella datorn. Dessa mallar har redan ett operativsystem (OS) och ofta annan programvara, till exempel utvecklingsverktyg eller värdmiljöer för webben.

## <a name="what-are-containers"></a>Vad är containrar?

> [!VIDEO https://www.microsoft.com/videoplayer/embed/RE2yMhY]

Containrar är en virtualiseringsmiljö, men till skillnad från virtuella datorer har de inget operativsystem. I stället refererar de till operativsystemet i den värdmiljö som kör containern. Om fem containrar till exempel körs på en server med en specifik Linux-kernel så körs alla fem containrar på samma kernel.

Följande bild visar en jämförelse mellan program som körs direkt på en virtuell dator och program som körs i behållare på en virtuell dator.

![En bild som visar hur operativsystemet är en del av den virtuella datorn och inte en del av behållaren](../media/2-vm-versus-containers.png)

Behållare innehåller vanligtvis ett program som du skriver &mdash; tillsammans med alla bibliotek som krävs för programmet att köras på den värdmiljö kernel.

Containrar är menade att vara enkla och utformade för att kunna skapas, skalas ut och stoppas dynamiskt. På så sätt kan du svara på ändringar på begäran och snabbt starta om vid en krasch eller maskinvara avbrott.

En annan fördel med att använda behållare är möjligheten att köra flera isolerade program på en virtuell dator. Eftersom containrar är skyddade och isolerade i sig själva behöver du inte nödvändigtvis separata virtuella datorer för olika arbetsbelastningar.

Azure har stöd för Docker-containrar och flera olika sätt att hantera dessa containrar. Du kan hantera containrar manuellt eller med Azure-tjänster, till exempel Azure Kubernetes Service.

### <a name="what-is-serverless-computing"></a>Vad är serverfri databehandling?

Serverfri databehandling är en molnbaserad körningsmiljö som kör din kod men där den underliggande värdmiljön är helt abstrakt. Du skapar en instans av tjänsten och lägger till din kod, men du behöver inte, och kan inte, konfigurera eller underhålla någon infrastruktur.

> [!VIDEO https://www.microsoft.com/videoplayer/embed/RE2yzjL]

Du konfigurerar dina serverfria appar för att svara på _händelser_. Det kan gälla en REST-slutpunkt, en periodisk timer eller till och med ett meddelande som tas emot från en annan Azure-tjänst. Den serverfria appen körs bara när den utlöses av en händelse.

I grunden innebär inte infrastruktur är ditt ansvar. Skalning och prestanda hanteras automatiskt och du debiteras endast för de resurser du använder. Du behöver inte ens reservera kapacitet.

Azure har två implementeringar av beräkning utan server:

- **Azure Functions** som kan köra kod på nästan alla moderna språk.
- **Azure Logic Apps** som är utformade i en webbaserad designer och kan köra logik som utlöses av Azure-tjänster utan att behöva skriva någon kod.

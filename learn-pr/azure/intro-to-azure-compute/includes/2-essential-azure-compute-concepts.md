Forskningsteamet behöver utföra beräkningsintensiv databehandling och har inte den utrustning som krävs för arbetet. De har bestämt sig för att utföra dataanalysen i Azure.

## <a name="what-is-azure-compute"></a>Vad är Azure Compute?
Azure Compute är en databehandlingstjänst på begäran för molnbaserade program. Den innehåller resurser som processorer med flera kärnor och superdatorer som du får tillgång till via virtuella datorer och containrar. Dessutom finns serverfri databehandling så att du kan köra appar utan att behöva installera eller konfigurera någon infrastruktur. Resurserna är tillgängliga på begäran och kan vanligtvis skapas på bara några minuter eller till och med sekunder. Du betalar bara för de resurser du använder och endast så länge du använder dem.

Det finns tre vanliga tekniker för databehandling i Azure:
1. Virtuella datorer
1. Containrar
1. Serverfri databehandling

## <a name="what-are-virtual-machines"></a>Vad är virtuella datorer?

En **virtuell dator (VM)** är en programvaruemulering av en fysisk dator. De innehåller en virtuell processor, minne, lagring och nätverksresurser. De är värd för ett operativsystem, och du kan installera och köra programvara precis som på en fysisk dator. Via en fjärrskrivbordsklient kan du använda och styra den virtuella datorn precis som om du satt framför en fysisk terminal.

### <a name="virtual-machines-in-azure"></a>Virtuella datorer i Azure

Du kan skapa och vara värd för virtuella datorer i Azure. I vanliga fall kan du skapa och etablera nya virtuella datorer på bara några minuter genom att välja en förkonfigurerad avbildning.

Att välja avbildning är ett av de viktigaste besluten när du skapar en virtuell dator. En avbildning är mallen som används till att skapa den virtuella datorn. Dessa mallar har redan ett operativsystem (OS) och ofta annan programvara, till exempel utvecklingsverktyg eller värdmiljöer för webben.

## <a name="what-are-containers"></a>Vad är containrar?

Containrar är en virtualiseringsmiljö, men till skillnad från virtuella datorer har de inget operativsystem. I stället refererar de till operativsystemet i den värdmiljö som kör containern. Om fem containrar till exempel körs på en server med en specifik Linux-kernel så körs alla fem containrar på samma kernel. 

Containrar innehåller normalt ett program som du skriver och som innehåller de bibliotek som krävs för att programmet ska kunna köras på värdmiljöns kernel. 

Containrar är menade att vara enkla och utformade för att kunna skapas, skalas ut och stoppas dynamiskt när miljön och behoven ändras.

En fördel med att använda containrar är att du kan köra flera isolerade program på en virtuell dator. Eftersom containrar är skyddade och isolerade i sig själva behöver du inte nödvändigtvis separata virtuella datorer för olika arbetsbelastningar.

Azure har stöd för Docker-containrar och flera olika sätt att hantera dessa containrar. Du kan hantera containrar manuellt eller med Azure-tjänster, till exempel Azure Kubernetes Service.

### <a name="what-is-serverless-computing"></a>Vad är serverfri databehandling?

Serverfri databehandling är en molnbaserad körningsmiljö som kör din kod men där den underliggande värdmiljön är helt abstrakt. Du skapar en instans av tjänsten och lägger till din kod, men du behöver inte, och kan inte, konfigurera eller underhålla någon infrastruktur.

Du konfigurerar dina serverfria appar för att svara på händelser. Det kan gälla en REST-slutpunkt, en timer eller ett meddelande som tas emot från en annan Azure-tjänst. Den serverfria appen körs bara när den utlöses av en händelse. 

Du ansvarar inte för infrastrukturen. Skalning och prestanda hanteras automatiskt och du debiteras endast för de resurser du använder. Du behöver inte ens reservera kapacitet.

Azure har två implementeringar av serverfri databehandling: **Azure Functions** som kan köra kod på nästan alla moderna språk, och **Azure Logic Apps** där du kan köra logik som utlöses av Azure-tjänster utan att du behöver skriva någon kod.

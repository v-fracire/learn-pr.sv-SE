Ditt forskningsteam har samlat in enorma mängder bilddata som kanske kan leda till en upptäckt på Mars. De behöver utföra beräkningsintensiv databehandling men har inte den utrustning som krävs för arbetet. Vi tittar på varför Azure är ett bra alternativ för den här dataanalysen.

## <a name="what-is-azure-compute"></a>Vad är Azure Compute?
Azure Compute är en databehandlingstjänst på begäran för körning av molnbaserade program. Den innehåller resurser som processorer med flera kärnor och superdatorer som du får tillgång till via virtuella datorer och containrar. Dessutom finns serverfri databehandling så att du kan köra appar utan att behöva installera eller konfigurera någon infrastruktur. Resurserna är tillgängliga på begäran och kan vanligtvis skapas på bara några minuter eller till och med sekunder. Du betalar bara för de resurser du använder och endast så länge du använder dem.

Det finns tre vanliga tekniker för databehandling i Azure:

- Virtuella datorer
- Containrar
- Serverfri databehandling

## <a name="what-are-virtual-machines"></a>Vad är virtuella datorer?

**Virtuella datorer**, eller VM, är programvaruemuleringar av fysiska datorer. De innehåller en virtuell processor, minne, lagring och nätverksresurser. De är värd för ett operativsystem, och du kan installera och köra programvara precis som på en fysisk dator. Via en fjärrskrivbordsklient kan du använda och styra den virtuella datorn precis som om du satt framför den.

:::row:::
  :::column:::
    ![Bild som representerar en virtuell dator](../media/2-vm.png)
  :::column-end:::
    ::: kolumnomfång = ”3”::: **Virtuella datorer i Azure**

Virtuella datorer kan skapas och hanteras i Azure. I vanliga fall kan du skapa och etablera nya virtuella datorer på bara några minuter genom att välja en förkonfigurerad avbildning.

Att välja avbildning är ett av de viktigaste besluten när du skapar en virtuell dator. Avbildningar är mallar som används till att skapa virtuella datorer. Dessa mallar har redan ett operativsystem (OS) och ofta annan programvara, till exempel utvecklingsverktyg eller värdmiljöer för webben.

  :::column-end:::
:::row-end:::

## <a name="what-are-containers"></a>Vad är containrar?

> [!VIDEO https://www.microsoft.com/videoplayer/embed/RE2yMhY]

Containrar är en virtualiseringsmiljö, men till skillnad från virtuella datorer har de inget operativsystem. I stället refererar de till operativsystemet i den värdmiljö som kör containern. Om fem containrar till exempel körs på en server med en specifik Linux-kernel så körs alla fem containrar på samma kernel.

I följande illustration visas en jämförelse mellan program som körs direkt på en virtuell dator och program som körs i containrar på en virtuell dator.

![En illustration som visar hur operativsystemet är en del av den virtuella datorn och inte en del av containern](../media/2-vm-versus-containers.png)

Containrar innehåller normalt ett program som du skriver samt de bibliotek som krävs för att programmet ska kunna köras på värdmiljöns kernel.

Containrar är tänkta att vara enkla och är utformade för att kunna skapas, skalas ut och stoppas dynamiskt. På så sätt kan du svara på ändringar i efterfrågan och snabbt starta om vid krasch eller avbrott i maskinvaran.

Ytterligare en fördel med att använda containrar är att du kan köra flera isolerade program på en virtuell dator. Eftersom själva containrarna är skyddade och isolerade behöver du inte nödvändigtvis separata virtuella datorer för olika arbetsbelastningar.

Azure har stöd för Docker-containrar och flera olika sätt att hantera dessa containrar. Du kan hantera containrar manuellt eller med Azure-tjänster, till exempel Azure Kubernetes Service.

### <a name="what-is-serverless-computing"></a>Vad är serverfri databehandling?

Serverfri databehandling är en molnbaserad körningsmiljö som kör din kod men där den underliggande värdmiljön är helt abstrakt. Du skapar en instans av tjänsten och lägger till din kod, men du behöver inte, och kan inte, konfigurera eller underhålla någon infrastruktur.

#### <a name="what-is-serverless-computing"></a>Vad är serverfri databehandling?

> [!VIDEO https://www.microsoft.com/videoplayer/embed/RE2yzjL]

Du konfigurerar dina serverfria appar för att svara på _händelser_. Det kan gälla en REST-slutpunkt, en periodisk timer eller till och med ett meddelande som tas emot från en annan Azure-tjänst. Den serverfria appen körs bara när den utlöses av en händelse.

Du ansvarar i princip inte för infrastrukturen. Skalning och prestanda hanteras automatiskt, och du debiteras endast för de resurser du använder. Du behöver inte ens reservera kapacitet.

:::row:::
  :::column:::
    ![Bild som representerar serverlös databehandling](../media/2-serverless.png)
  :::column-end:::
    ::: kolumnomfång = ”3”::: **Serverlös databehandling i Azure**

Azure har två implementeringar av serverlös beräkning:

- **Azure Functions**, som kan köra kod på nästan alla moderna språk.
- **Azure Logic Apps**, som är utformade i en webbaserad designer och kan köra logik som utlöses av Azure-tjänster utan att du behöver skriva någon kod.

  :::column-end:::
:::row-end:::
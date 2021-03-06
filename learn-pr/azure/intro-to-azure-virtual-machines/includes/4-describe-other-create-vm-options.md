Azure-portalen är det enklaste sättet att skapa resurser såsom virtuella datorer när du kör igång. Det är dock inte nödvändigtvis det effektivaste eller snabbaste sättet att arbeta med Azure, särskilt om du vill skapa flera resurser tillsammans. I vårt fall kommer vi så småningom att skapa flera virtuella datorer för att hantera olika uppgifter. Det vore inte särskilt roligt att skapa dem manuellt i Azure-portalen!

Vi tittar på några andra metoder för att skapa och administrera resurser i Azure:

- Azure Resource Manager
- Azure PowerShell
- Azure CLI
- Azure REST API
- Azure Client SDK
- Azure VM-tillägg
- Azure Automation Services

## <a name="azure-resource-manager"></a>Azure Resource Manager

Anta att du vill skapa en kopia av en virtuell dator med samma inställningar. Du kan skapa en avbildning av en virtuell dator, överföra den till Azure och referera till den som underlag för den nya virtuella datorn. Den här processen är ineffektiv och tidskrävande. Med Azure kan du skapa en mall och använda den för att skapa en exakt kopia av en virtuell dator.

Vanligtvis innehåller din Azure-infrastruktur många resurser, varav många är relaterade till varandra på något sätt. Till exempel har den virtuella dator som vi skapade den virtuella datorn själv, lagring, nätverksgränssnitt, webbserver och en databasen – alla skapade tillsammans för att köra WordPress-webbplatsen. **Azure Resource Manager** gör det effektivare att arbeta med dessa relaterade resurser. Den ordnar resurser i namngivna **resursgrupper** som du kan använda för att distribuera, uppdatera eller ta bort alla resurser samtidigt. När vi skapade WordPress-webbplatsen identifierade vi resursgruppen som en del av att skapa den virtuella datorn. Resource Manager placerade därefter de associerade resurserna i samma grupp.

Med Resource Manager kan du även skapa _mallar_ som kan användas för att skapa och distribuera specifika konfigurationer.

### <a name="what-are-resource-manager-templates"></a>Vad är Resource Manager-mallar?

**Resource Manager-mallar** är JSON-filer som definierar de resurser du behöver för att distribuera lösningen.

Du kan skapa resursmallar från avsnittet **Inställningar** för en specifik virtuell dator genom att välja alternativet för automatiseringsskript.

![Automatiseringsskript för vår virtuella dator](../media/4-automation-script.png)

Du har möjlighet att spara resursmallen för senare användning eller omedelbart distribuera en ny virtuell dator baserat på den här mallen. Till exempel skapar du kanske en virtuell dator från en mall i en testmiljö och upptäcker att den inte riktigt fungerar som ersättning för din lokala dator. Du kan ta bort resursgruppen, vilket tar bort alla resurser, justera mallen och försöka igen. Om du bara vill göra ändringar i befintliga distribuerade resurser kan du ändra den mall som används för att skapa den och distribuera den igen. Resource Manager ändrar resurserna för att matcha den nya mallen.

När det fungerar på det sätt du vill kan du använda den mallen och enkelt skapa flera versioner av infrastrukturen, till exempel mellanlagring och produktion. Du kan parameterisera fält som den virtuella datorns namn, nätverksnamn, lagringskontonamn osv. och läsa in mallen upprepade gånger med olika parametrar för att anpassa varje miljö.

Du kan använda skriptverktyg för automatisering som Azure CLI, Azure PowerShell eller till och med Azures REST API:er med det programmeringsspråk du föredrar för att bearbeta resursmallarna. Detta gör det här till ett kraftfullt verktyg för att snabbt få fart på din infrastruktur.

## <a name="azure-powershell"></a>Azure PowerShell

Att skapa administrationsskript är ett kraftfullt sätt att optimera ditt arbetsflöde på. Du kan automatisera vanliga, repetitiva uppgifter och när ett skript har verifierats kan det kan köras konsekvent, vilket minskar risken för fel. **Azure PowerShell** är perfekt för interaktiva engångsuppgifter och/eller automatisering av återkommande uppgifter.

> [!NOTE]
> PowerShell är ett plattformsoberoende gränssnitt som tillhandahåller tjänster som gränssnittsfönstret och kommandoparsning. Azure PowerShell är ett valfritt tilläggspaket som lägger till Azure-specifika kommandon (kallas för **cmdletar**). Du kan lära dig mer om att installera och använda Azure PowerShell i en separat utbildningsmodul.

Du kan till exempel använda cmdleten `New-AzureRmVM` för att skapa en ny virtuell Azure-dator.

```powershell
New-AzureRmVm `
    -ResourceGroupName "TestResourceGroup" `
    -Name "test-wp1-eus-vm" `
    -Location "East US" `
    -VirtualNetworkName "test-wp1-eus-network" `
    -SubnetName "default" `
    -SecurityGroupName "test-wp1-eus-nsg" `
    -PublicIpAddressName "test-wp1-eus-pubip" `
    -OpenPorts 80,3389
```

Enligt vad som visas här kan du ange olika parametrar för att hantera det stora antalet tillgängliga konfigurationsinställningar för virtuella datorer. De flesta av parametrarna har rimliga värden, så du behöver bara ange de parametrar som krävs. Läs mer om att skapa och hantera virtuella datorer med Azure PowerShell i modulen **Automatisera Azure-åtgärder med hjälp av PowerShell-skript**.

## <a name="azure-cli"></a>Azure CLI

Ett annat alternativ för Azure interaktion via skriptning och kommandorad är **Azure CLI**.

Azure CLI är Microsofts plattformsoberoende kommandoradsverktyg för att hantera Azure-resurser såsom virtuella datorer och diskar från kommandoraden. Det finns för macOS, Linux och Windows eller i webbläsaren med hjälp av Cloud Shell. Liksom Azure PowerShell är Azure CLI ett kraftfullt sätt att effektivisera det administrativa arbetsflödet. Till skillnad från Azure PowerShell behöver Azure CLI inte PowerShell för att fungera.

Du kan till exempel skapa en virtuell Azure-dator med kommandot `az vm create`.

```azurecli
az vm create \
    --resource-group TestResourceGroup \
    --name test-wp1-eus-vm \
    --image win2016datacenter \
    --admin-username jonc \
    --admin-password aReallyGoodPasswordHere
```

Azure CLI kan användas med andra skriptspråk, till exempel Ruby och Python. Båda språken används ofta på icke-Windows-baserade datorer där utvecklaren kanske inte är bekant med PowerShell.

Läs mer om att skapa och hantera virtuella datorer i modulen **Hantera virtuella datorer med Azure CLI-verktyget**.

## <a name="programmatic-apis"></a>Programmatiskt (API:er)

Både Azure PowerShell och Azure CLI är generellt sett bra alternativ om du har enkla skript som ska köras och om du vill hålla dig till kommandoradsverktygen. När det gäller mer komplicerade scenarier där skapandet och hanteringen av virtuella datorer utgör en del av ett större program med komplex logik krävs en annan metod.

Du kan interagera med alla typer av resurser i Azure programmatiskt.

### <a name="azure-rest-api"></a>Azure REST API

Med Azure REST API får utvecklarna sina åtgärder kategoriserade efter resurs, samt möjlighet att skapa och hantera virtuella datorer. Åtgärder görs tillgängliga som URI:er med motsvarande HTTP-metoder (`GET`, `PUT`, `POST`, `DELETE` och `PATCH`) och ett motsvarande svar.

Med Azure Compute-API:erna får du programmatisk åtkomst till virtuella datorer och deras stödresurser. Med detta API får du åtgärder för att:

- Skapa och hantera tillgänglighetsuppsättningar
- Lägga till och hantera VM-tillägg
- Skapa och hantera hanterade diskar, ögonblicksbilder och avbildningar
- Komma åt plattformsavbildningar som är tillgängliga i Azure
- Hämta användningsinformation för dina resurser
- Skapa och hantera virtuella datorer
- Skapa och hantera VM-skalningsuppsättningar

### <a name="azure-client-sdk"></a>Azure Client SDK

Även om REST API är plattforms- och språkoberoende, vill de flesta utvecklare ha en högre abstraktionsnivå. Azure Client SDK kapslar in Azure REST API, vilket gör det enklare för utvecklarna att interagera med Azure.

Azure Client SDK:er är tillgängliga för olika språk och ramverk, däribland .NET-baserade språk som C#, Java, Node.js, PHP, Python, Ruby och Go.

Här är ett kodfragment med C#-kod som skapar en virtuell Azure-dator med hjälp av NuGet-paketet `Microsoft.Azure.Management.Fluent`:

```csharp
var azure = Azure
    .Configure()
    .WithLogLevel(HttpLoggingDelegatingHandler.Level.Basic)
    .Authenticate(credentials)
    .WithDefaultSubscription();
// ...
var vmName = "test-wp1-eus-vm";

azure.VirtualMachines.Define(vmName)
    .WithRegion(Region.USEast)
    .WithExistingResourceGroup("TestResourceGroup")
    .WithExistingPrimaryNetworkInterface(networkInterface)
    .WithLatestWindowsImage("MicrosoftWindowsServer", "WindowsServer", "2012-R2-Datacenter")
    .WithAdminUsername("jonc")
    .WithAdminPassword("aReallyGoodPasswordHere")
    .WithComputerName(vmName)
    .WithSize(VirtualMachineSizeTypes.StandardDS1)
    .Create();
```

Här är samma kodavsnitt i Java med hjälp av **Azure Java SDK**:

```java
String vmName = "test-wp1-eus-vm";
// ...
VirtualMachine virtualMachine = azure.virtualMachines()
    .define(vmName)
    .withRegion(Region.US_EAST)
    .withExistingResourceGroup("TestResourceGroup")
    .withExistingPrimaryNetworkInterface(networkInterface)
    .withLatestWindowsImage("MicrosoftWindowsServer", "WindowsServer", "2012-R2-Datacenter")
    .withAdminUsername("jonc")
    .withAdminPassword("aReallyGoodPasswordHere")
    .withComputerName(vmName)
    .withSize("Standard_DS1")
    .create();
```

## <a name="azure-vm-extensions"></a>Azure VM-tillägg

Anta att du vill konfigurera och installera ytterligare programvara på din virtuella dator efter den första distributionen. Du vill att den här uppgiften ska använda en specifik konfiguration, övervakas och köras automatiskt.

**Azure VM-tillägg** är små program som gör att du kan konfigurera och automatisera uppgifter på virtuella Azure-datorer efter den första distributionen. **Azure VM-tillägg** kan köras med Azure CLI, PowerShell, Azure Resource Manager-mallar och Azure-portalen.

Du paketerar tillägg med en ny VM-distribution eller kör dem mot ett befintligt system.

## <a name="azure-automation-services"></a>Azure Automation Services

Att spara tid, minska fel och öka effektiviteten är några av de viktigaste driftsutmaningarna vid hantering av infrastruktur för fjärråtkomst. Om du har flera infrastrukturtjänster bör du överväga att använda tjänster på högre nivå i Azure för att underlätta ditt arbete.

Med **Azure Automation** kan du integrera tjänster för att enkelt automatisera återkommande, tidskrävande och felbenägna hanteringsuppgifter. Tjänsterna omfattar bland annat **processautomatisering**, **konfigurationshantering** och **uppdateringshantering**.

- **Processhantering**. Anta att du har en virtuell dator som övervakas för en särskild felhändelse. Du vill vidta åtgärder och lösa problemet så fort det rapporteras. Med processautomatisering kan du konfigurera bevakaruppgifter som kan svara på händelser som kan uppstå i datacentret.

- **Konfigurationshantering**.  Du vill kanske spåra programuppdateringar som blir tillgängliga för det operativsystem som körs på den virtuella datorn. Det finns specifika uppdateringar som du kan vilja inkludera eller exkludera. Konfigurationshantering gör att du kan spåra de här uppdateringarna och vidta åtgärder vid behov. Du använder **System Center Configuration Manager** för att hantera företagets PC-datorer, servrar och mobila enheter. Du kan utöka det här stödet till dina virtuella Azure-datorer med Configuration Manager.

- **Uppdateringshantering**. Det här används till att hantera uppdateringar och korrigeringar för dina virtuella datorer. Med den här tjänsten kan du utvärdera statusen för tillgängliga uppdateringar, schemalägga installation och granska distributionsresultat för att verifiera att uppdateringarna tillämpas korrekt. Uppdateringshantering omfattar tjänster som ger process- och konfigurationshantering. Du aktiverar uppdateringshantering för en virtuell dator direkt via ditt **Azure Automation**-konto. Du kan även tillåta uppdateringshantering för en enskild virtuell dator från bladet för virtuella datorer i portalen.

Som du ser erbjuder Azure en mängd olika verktyg för att skapa och administrera resurser, så att du kan integrera hanteringsåtgärder i en process _som passar dig_. Nu ska vi undersöka några av de andra Azure-tjänsterna för att se till att dina infrastrukturresurser körs problemfritt.

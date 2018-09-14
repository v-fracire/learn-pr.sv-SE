En teknik som professionella som har du sannolikt expertis i ett visst område. Du kanske är en storage-administratör eller virtualisering experten eller kanske du fokusera på de senaste säkerhetsrutiner. Om du är student, kan du fortfarande att utforska vad som intresserar dig mest.

::: zone pivot="windows-cloud"

Oavsett din roll börjar de flesta med molnet genom att skapa en virtuell dator. Här kan du sätta upp en virtuell dator som kör Windows Server 2016.

::: zone-end

::: zone pivot="linux-cloud"

Oavsett din roll börjar de flesta med molnet genom att skapa en virtuell dator. Här kan du sätta upp en virtuell dator som kör Ubuntu 16.04.

::: zone-end

Det finns många sätt att skapa en virtuell dator på Azure. Här får du ta fram en Windows- eller Linux-dator med en interaktiv terminal kallas Cloud Shell. Om du arbetar från terminalen dagligen, vet du det här är ofta det snabbaste sättet att få jobbet gjort.

::: zone pivot="windows-cloud"

> [!TIP]
> Föredrar Linux eller vill du testa något nytt? Välj **Linux** högst upp på sidan för att köra en virtuell Linux-dator.

::: zone-end

::: zone pivot="linux-cloud"

> [!TIP]
> Föredrar Windows eller vill du testa något nytt? Välj **Windows** högst upp på sidan för att köra en Windows Server-dator.

::: zone-end

Nu ska vi gå igenom några grundläggande termer och få din första virtuella dator och drift.

## <a name="what-is-a-virtual-machine"></a>Vad är en virtuell dator?

En virtuell dator eller virtuell dator, är en programvaruemulering av en fysisk dator. Eftersom virtuella datorer finns som programvaran, dussintals, hundratals eller tusentals virtuella Azure-datorer kan skapas på några minuter, sedan tas bort när du inte längre behövs. Med låg kostnad, per minut fakturering betalar du bara för de beräkningsresurser du använder, så länge som du använder dem. Dessutom finns många olika sätt att konfigurera de virtuella datorerna så att de passar dina behov.

::: zone pivot="windows-cloud"

En ögonblicksbild av en aktiv virtuell dator kallas en _bild_. Azure erbjuder avbildningar för Windows och flera varianter av Linux. Du kan också skapa dina egna förkonfigurerade avbildningar för att göra distributioner gå snabbare. Här kan du öppna en Windows Server 2016 VM, som tillhandahålls av Microsoft.

::: zone-end

::: zone pivot="linux-cloud"

En ögonblicksbild av en aktiv virtuell dator kallas en _bild_. Azure erbjuder avbildningar för Windows och flera varianter av Linux. Du kan också skapa dina egna förkonfigurerade avbildningar för att göra distributioner gå snabbare. Här får du ta fram en Ubuntu 16.04 VM som tillhandahålls av Canonical.

::: zone-end

## <a name="what-defines-a-virtual-machine-on-azure"></a>Vad definierar en virtuell dator på Azure?

En virtuell dator definieras av ett antal faktorer, inklusive dess storlek och plats. Innan du skapar den virtuella datorn ska vi kortfattat beskriver vad ingår.

:::row:::
    :::column:::
        **Storlek**
    :::column-end:::
    ::: kolumnen span = ”3”::: A VM _storlek_ definierar dess processorhastighet, mängden minne, inledande mängden lagring och förväntade nätverkets bandbredd. Vissa värden är även specialiserad maskinvara, till exempel GPU: er för tung grafikrendering och videoredigering.
    :::column-end:::
:::row-end:::

:::row:::
    :::column:::
        **Region**
    :::column-end:::
    ::: kolumnen span = ”3”::: Azure består av Datacenter runtom i världen. En _region_ är en uppsättning Azure-datacenter i en namngiven geografisk plats. Varje Azure-resursen, inklusive virtuella datorer tilldelas en region. Östra USA och Nordeuropa är exempel på regioner.
    :::column-end:::
:::row-end:::

:::row:::
    :::column:::
        **Nätverk**
    :::column-end:::
    ::: kolumnen span = ”3”::: A _virtuellt nätverk_ är ett logiskt isolerat nätverk i Azure. Varje virtuell dator på Azure är associerad med ett virtuellt nätverk. Azure tillhandahåller moln på servernivå brandväggar för dina virtuella nätverk som kallas _nätverkssäkerhetsgrupper_.
    :::column-end:::
:::row-end:::

:::row:::
    :::column:::
        **Resursgrupper**
    :::column-end:::
    ::: kolumnen span = ”3”::: virtuella datorer och andra molnresurser grupperas i en logisk behållare som kallas _resursgrupper_. Grupper används vanligtvis för att organisera uppsättningar av resurser som distribueras tillsammans som en del av ett program eller tjänst. Du referera till en resursgrupp med namnet.
    :::column-end:::
:::row-end:::

## <a name="what-is-azure-cloud-shell"></a>Vad är Azure Cloud Shell?

Azure Cloud Shell är en webbläsarbaserad kommandoradsmiljö för att hantera och utveckla Azure-resurser. Tänk på Cloud Shell som en interaktiv konsol som du kör i molnet.

Cloudshell tillhandahåller två upplevelser för att välja mellan: Bash och PowerShell. Båda omfattar tillgång till Azure CLI, kommandoradsgränssnittet för Azure.

Du kan använda valfri Azure hanteringsgränssnitt, inklusive Azure-portalen, Azure CLI och Azure PowerShell för att hantera alla typer av virtuella datorer. I utbildningssyfte ska här du använda Azure CLI att skapa och hantera en Windows- eller Linux VM.

::: zone pivot="windows-cloud"

## <a name="create-a-windows-vm"></a>Skapa en virtuell Windows-dator

[!include[](../../../includes/azure-sandbox-activate.md)]

Få din virtuella Windows-dator vi igång.

<!--

TODO: Omitted for sandbox. Possibly re-insert later for non-sandbox workflow.

1. From Cloud Shell on the right side of this page, run the `az group create` command to create a resource group named **myResourceGroup** in the East US region.

    ```azurecli
    az group create \
      --location eastus \
      --name myResourceGroup
    ```
-->

1. Kör från Cloud Shell på höger sida av den här sidan i `az vm create` kommando för att skapa den virtuella datorn. Vi rekommenderar att du ändrar lösenordet nedan till en ska lagras.

      > [!NOTE]
    > Välj ett lösenord som innehåller minst 8 tecken med en kombination av övre och gemena bokstäver, siffror och symboler.

    ```azurecli
    az vm create \
      --name myWindowsVM \
      --resource-group <rgn>[Sandbox resource group name]</rgn> \
      --image Win2016Datacenter \
      --size Standard_DS2_v2 \
      --admin-username azureuser \
      --admin-password "Password1234&"
    ```

    > [!TIP]
    > Du kan använda den **kopiera** för att kopiera varje kommando. Om du vill klistra in den, högerklicka på den nya raden i Cloud Shell-fönstret och välj **klistra in**.

    Den virtuella datorn tar fyra till fem minuter att få fram. Jämför med den tid det tar att köpa, rack och konfigurera ett system i ditt datacenter. En skillnad!

Under tiden du väntar vi ska gå igenom kommandot som du körde.

* Den virtuella datorn **myWindowsVM**. Det här namnet identifierar den virtuella datorn i Azure. Det blir även Virtuellt datorns interna värdnamn eller datornamn.
* Resursgruppen eller den Virtuella datorns logisk behållare, heter  **<rgn>[Sandbox resursgruppens namn]</rgn>**.
* **Win2016Datacenter** anger Windows Server 2016 VM-avbildning.
* **Standard_DS2_v2** refererar till den virtuella datorn storlek. Den här storleken har två virtuella processorer och 7 GB minne.
* Användarnamnet och lösenordet kan du ansluta till den virtuella datorn senare. Du kan till exempel ansluta via fjärrskrivbord eller WinRM för att arbeta med och konfigurera systemet.

Som standard tilldelar Azure offentlig IP-adress till den virtuella datorn. Du kan konfigurera en virtuell dator för att vara tillgänglig från Internet eller från det interna nätverket.

Du kan också Kolla in den här korta videon om några av de alternativ som du måste skapa och hantera virtuella datorer.

> [!VIDEO https://www.microsoft.com/videoplayer/embed/RE2yJKx]

När den virtuella datorn är klar kan se du information om den. Här är ett exempel.

```console
{
  "fqdns": "",
  "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachines/myWindowsVM",
  "location": "eastus",
  "macAddress": "00-0D-3A-1E-1B-3B",
  "powerState": "VM running",
  "privateIpAddress": "10.0.0.5",
  "publicIpAddress": "104.211.9.245",
  "resourceGroup": "myResourceGroup",
  "zones": ""
}
```

::: zone-end

::: zone pivot="linux-cloud"

## <a name="create-a-linux-vm"></a>Skapa en virtuell Linux-dator

[!include[](../../../includes/azure-sandbox-activate.md)]

Få din Linux-VM vi igång.

<!--

TODO: Omitted for sandbox. Possibly re-insert later for non-sandbox workflow.
 
1. From Cloud Shell on the right side of this page, run the `az group create` command to create a resource group named **myResourceGroup** in the East US region.

    ```azurecli
    az group create \
      --location eastus \
      --name myResourceGroup
    ```
-->

1. Kör från Cloud Shell på höger sida av den här sidan i `az vm create` kommando för att skapa den virtuella datorn.

    ```azurecli
    az vm create \
      --name myLinuxVM \
      --resource-group <rgn>[Sandbox resource group name]</rgn> \
      --image UbuntuLTS \
      --size Standard_DS2_v2 \
      --generate-ssh-keys
    ```

    > [!TIP]
    > Du kan använda den **kopiera** för att kopiera varje kommando. Om du vill klistra in den, högerklicka på den nya raden i Cloud Shell-fönstret och välj **klistra in**.

    Den virtuella datorn tar ungefär två minuter för att få fram. Jämför med den tid det tar att köpa, rack och konfigurera ett system i ditt datacenter. En skillnad!

Under tiden du väntar vi ska gå igenom kommandot som du körde.

* Den virtuella datorn **myLinuxVM**. Det här namnet identifierar den virtuella datorn i Azure. Det blir även Virtuellt datorns interna värdnamn eller datornamn.
* Resursgruppen eller den Virtuella datorns logisk behållare, heter  **<rgn>[Sandbox resursgruppens namn]</rgn>**.
* **UbuntuLTS** anger Ubuntu 16.04 LTS VM-avbildning.
* **Standard_DS2_v2** refererar till den virtuella datorn storlek. Den här storleken har två virtuella processorer och 7 GB minne.
* Den `--generate-ssh-keys` alternativ för att skapa en SSH-nyckelpar så att du kan logga in på den virtuella datorn.

Som standard tilldelar Azure offentlig IP-adress till den virtuella datorn. Du kan konfigurera en virtuell dator för att vara tillgänglig från Internet eller från det interna nätverket.

Du kan också Kolla in den här korta videon om några av de alternativ som du måste skapa och hantera virtuella datorer.

> [!VIDEO https://www.microsoft.com/videoplayer/embed/RE2yJKx]

När den virtuella datorn är klar kan se du information om den. Här är ett exempel.

```console
{
    "fqdns": "",
    "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachines/myLinuxVM",
    "location": "eastus",
    "macAddress": "00-0D-3A-1D-EB-02",
    "powerState": "VM running",
    "privateIpAddress": "10.0.0.4",
    "publicIpAddress": "137.135.110.210",
    "resourceGroup": "myResourceGroup",
    "zones": ""
}
```

::: zone-end

## <a name="summary"></a>Sammanfattning

Med bara några begrepp när du klarat kan du sätta upp en virtuell dator i Azure på bara några minuter. Många av de här koncepten, till exempel en VM-storlek och brandväggsregler, är redan sannolikt välbekant för dig.

Nu ska du installera en webbserver på den virtuella datorn och konfigurera webbservern för att leverera en grundläggande webbplats.
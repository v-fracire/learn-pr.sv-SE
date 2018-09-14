I den här övningen ska du skapa en virtuell dator och ändra sedan storlek på den med hjälp av portalen och Azure PowerShell.

## <a name="create-a-vm"></a>Skapa en virtuell dator

[!include[](../../../includes/azure-sandbox-activate.md)]

[!include[](../../../includes/azure-sandbox-regions-first-mention-note.md)]

1. Logga in på [Azure Portal](https://portal.azure.com/?azure-portal=true).

1. Skapa en ny resurs i Azure-portalen. I den **New** bladet, skriver du in **VM** i sökrutan och tryck på RETUR.

1. På den **allt** bladet under **resultat**, klickar du på **Windows Server 2016 Datacenter**.

1. På den **Windows Server 2016 Datacenter** bladet klickar du på **skapa**.

1. På den **grunderna** bladet Slutför detaljerna med följande information och klicka sedan på **OK**.

    |Inställning|Värde|
    |---|---|
    |Namn|DB01|
    |Användarnamn|LocalAdmin|
    |Lösenord och Bekräfta lösenord|Adm1nPa$ word|
    |Resursgrupp|<rgn>[Sandbox resursgruppens namn]</rgn>|
    |Plats|*Välj en region i listan*|

1. På den **väljer du en storlek** bladet väljer **D2s_v3**, och klicka sedan på **Välj**.

1. På den **inställningar** bladet under **Välj offentliga inkommande portar** Välj **HTTP**, **HTTPS**, och **RDP (port 3389)**. Under **Startdiagnostik**, klickar du på **inaktiverad**. Lämna alla andra inställningar på standardvärdet och klicka sedan på **OK**.

1. På bladet **API-app** klickar du på **Skapa**.

1. Vänta tills distributionen är klar innan du fortsätter den här övningen.

## <a name="resize-using-the-portal"></a>Ändra storlek med hjälp av portalen

1. I Azure-portalen bläddrar du till resursgruppen ExerciseRG och i den **ExerciseRG** bladet klickar du på den **DB01** virtuella datorobjektet.

1. På den **DB01** bladet klickar du på **storlek**. Observera den markerade storleken är storleken som du valde när du skapar den virtuella datorn.

1. På den **väljer du en storlek** bladet försök att hitta den **F2s_v2** storlek – det ska inte finnas i en lista med storlekar eftersom den virtuella datorn körs och den storleken är från en annan familj. Stäng den **väljer du en storlek** bladet.

1. I den **DB01** bladet klickar du på **stoppa**. I den **stoppa denna virtuella dator** dialogrutan klickar du på **Ja**, och vänta tills status för virtuell dator att visa **Stoppad (frigjord)**.

1. På den **DB01** bladet klickar du på **storlek**. På den **väljer du en storlek** bladet väljer **F2s_v2** och klicka sedan på **Välj**. Ser meddelandet om att ändra storlek på den virtuella datorn.

1. På den **DB01** bladet klickar du på **översikt**, och klicka sedan på **starta**.

## <a name="resize-using-powershell"></a>Ändra storlek med hjälp av PowerShell

1. Öppna Azure Cloud Shell i Azure-portalen.

1. Använd följande cmdlet för att hämta listan över tillgängliga virtuella datorstorlekar.

    ```PowerShell
    Get-AzureRmVMSize -ResourceGroupName ExerciseRG -VMName DB01
    ```

1. Använd följande cmdlet för att ändra storlek på den virtuella datorn till en F4s_v2 storlek.

    ```PowerShell
    $vm = Get-AzureRmVM -ResourceGroupName ExerciseRG -VMName DB01
    $vm.HardwareProfile.VmSize = "Standard_F4s_v2"
    Update-AzureRmVM -VM $vm -ResourceGroupName ExerciseRG
    ```

1. Klicka på Uppdatera i bladet DB01 medan du väntar i PowerShell-kommandot har slutförts. Du bör se att den virtuella datorn startas om efter ändringen i storlek.

I den här övningen ska du skapade en virtuell dator och storlek med två olika verktyg. Ett bra tips att tänka på är att målstorleken inte kanske tillgänglig medan den virtuella datorn körs på. Stoppa den virtuella datorn kan du välja fler storlekar.
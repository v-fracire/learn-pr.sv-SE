I den här övningen ska du skapa en virtuell dator och sedan ändra storlek på den med hjälp av portalen och Azure PowerShell.

## <a name="create-a-vm"></a>Skapa en virtuell dator

1. I webbläsaren går du till [Azure Portal](https://portal.azure.com?azure-portal=true) och loggar in på ditt konto.

1. Skapa en ny resurs i Azure Portal. På bladet **Nytt**  skriver du **virtuell dator** i sökrutan. Tryck sedan på RETUR.

1. På bladet **Allting** under **Resultat**, klickar du på **Windows Server 2016 Datacenter**.

1. Klicka på **Skapa** på bladet **Windows Server 2016 Datacenter**.

1. På bladet **Grunder** fyller du i följande information och klickar sedan på **OK**.

    |Inställning|Värde|
    |---|---|
    |Namn|DB01|
    |Användarnamn|LocalAdmin|
    |Lösenord och Bekräfta lösenord|Adm1nPa$$word|
    |Resursgrupp|ExerciseRG|
    |Plats|USA, centrala|

1. På bladet **Välj en storlek** väljer du **D2s_v3** och klickar sedan på **Välj**.

1. På bladet **Inställningar** under **Välj offentliga inkommande portar** väljer du **HTTP**, **HTTPS** och **RDP (3389)** under **Startdiagnostik**. Klicka på **Inaktiverad**, lämna alla andra inställningar med standardvärdet och klicka sedan på **OK**.

1. På bladet **Skapa** klickar du på **Skapa**.

1. Vänta tills distributionen är klar innan du fortsätter med den här övningen.

## <a name="resize-using-the-portal"></a>Ändra storlek med hjälp av portalen

1. I Azure Portal bläddrar du till resursgruppen ExerciseRG och på bladet **ExerciseRG** klickar du på det virtuella datorobjektet **DB01**.

1. På bladet **DB01** klickar du på **Storlek**. Observera att den markerade storleken är storleken som du valde när du skapade den virtuella datorn.

1. På bladet **Välj en storlek** försöker du hitta storleken **F2s_v2** – den bör inte finnas i listan med storlekar eftersom den virtuella datorn körs och den storleken är från en annan familj. Stäng bladet **Välj en storlek**.

1. På bladet **DB01** klickar du på **Stoppa**. I dialogrutan **Stoppa denna virtuella dator** klickar du på **Ja** och väntar tills statusen för den virtuella datorn visar **Stoppad (frigjord)**.

1. På bladet **DB01** klickar du på **Storlek**. På bladet **Välj en storlek** väljer du **F2s_v2** och klickar sedan på **Välj**. Observera meddelandet om att ändra storlek på den virtuella datorn.

1. På bladet **DB01** klickar du på **Översikt** och sedan på **Starta**.

## <a name="resize-using-powershell"></a>Ändra storlek med hjälp av PowerShell

1. Öppna Cloud Shell i Azure Portal.

1. Använd följande cmdlet för att hämta listan med tillgängliga virtuella datorstorlekar.

    ```PowerShell
    Get-AzureRmVMSize -ResourceGroupName ExerciseRG -VMName DB01
    ```

1. Använd följande cmdlet för att ändra storlek på den virtuella datorn till en F4s_v2-storlek.

    ```PowerShell
    $vm = Get-AzureRmVM -ResourceGroupName ExerciseRG -VMName DB01
    $vm.HardwareProfile.VmSize = "Standard_F4s_v2"
    Update-AzureRmVM -VM $vm -ResourceGroupName ExerciseRG
    ```

1. Klicka på knappen Uppdatera på bladet DB01 medan du väntar på att PowerShell-kommandot ska slutföras. Du bör se att den virtuella datorn startas om efter ändringen i storlek.

I den här övningen skapade du en virtuell dator och ändrade storlek på den med två olika verktyg. Ett bra tips att tänka på är att målstorleken kanske inte är tillgänglig medan den virtuella datorn körs. Om du stoppar den virtuella datorn kan du välja fler storlekar.
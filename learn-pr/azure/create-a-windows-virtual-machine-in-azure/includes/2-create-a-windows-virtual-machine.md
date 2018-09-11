Ditt företag har valt att hantera data som video från sina trafikkameror med virtuella datorer i Azure. För att köra de olika codecarna måste vi först skapa de virtuella datorerna. Vi måste också ansluta till och interagera med de virtuella datorerna. I den här utbildningsenheten får du lära dig att skapa en virtuell dator i Azure Portal. Du kommer att konfigurera den virtuella datorn för RDP, välja en VM-avbildning och ett lämpligt lagringsalternativ.

## <a name="introduction-to-windows-virtual-machines-in-azure"></a>Introduktion till virtuella Windows-datorer i Azure

Virtuella datorer i Azure är databehandlingsresurser som kan skalas om på begäran. De liknar de virtuella datorer som används i Windows Hyper-V. De har en processor, minne, lagring och nätverksresurser. Du kan starta och stoppa de virtuella datorerna precis som i Hyper-V, och du kan hantera dem från portalen eller via Azure CLI. Du kan också använda en RDP-klient och ansluta direkt till Windows-skrivbordet, och använda den virtuella datorn som precis som om du var inloggad på en lokal Windows-dator.

## <a name="create-resources-for-a-windows-vm"></a>Skapa resurser för en virtuell Windows-dator

När du skapar en virtuell Windows-dator i Azure skapar du även resurser för den virtuella datorns värdmiljö. Precis som för andra Azure-tjänster behöver du en **resursgrupp**. En vanlig metod är att lägga andra tjänster som hör till den virtuella datorn i samma resursgrupp, bland annat följande:

* Den virtuella datorn
* Lagring
* Virtuella nätverk 
* Nätverksgränssnitt
* Undernät
* Offentlig IP-adress

När du skapar en ny virtuell dator kan du antingen använda en befintlig resursgrupp eller skapa en ny.

## <a name="choose-the-vm-image"></a>Välja VM-avbildning

Att välja avbildning är ett av de första och viktigaste besluten när du skapar en virtuell dator. En avbildning är mallen som används till att skapa den virtuella datorn. Dessa mallar har ett operativsystem och ofta annan programvara, som utvecklingsverktyg eller värdmiljöer för webben.

Allt som en dator kan ha installerat kan ingå i en avbildning. Det är mycket användbart. Du kan skapa en virtuell dator från en avbildning som är förkonfigurerad för just det du behöver göra, som att köra en ASP.Net Core-app.

Du kan också skapa och ladda upp egna avbildningar, men det går vi inte igenom i den här modulen.

> [!Note] 
> Du kanske ser en funktion som heter ”Virtuell dator (klassisk)”. Det här är en äldre funktion för kunder som redan har skapat instanser. För nya arbetsbelastningar ska du använda funktionen ”Virtuell Azure-dator” eller ”Virtuella datorer”.

## <a name="sizing-your-vm"></a>Ange storlek för den virtuella datorn

En virtuell dator har en viss mängd minne och processorkraft precis som en fysisk dator. Azure erbjuder virtuella datorer i olika storlekar till olika priser. De virtuella datorerna finns i allt från A-seriens avbildningar på ingångsnivå för utveckling och testning, till D-serien som är till för allmän databehandling. Du har H-serien för krävande beräkningsbehov och N-serien som är optimerad för grafisk användning.

Senare kommer du se att du kan ändra storlek för en virtuell dator efter att den har skapats. Då måste du dock stänga av den, och det kan leda till driftstopp för våra arbetsbelastningar.

## <a name="choosing-storage-options"></a>Välja lagringsalternativ

Nästa val för din virtuella dator gäller hårddisktekniken. Du kan välja en traditionell skivbaserad hårddisk (HDD) eller en modernare Solid State-hårddisk (SSD). SSD-lagringen kostar mer att använda men ger bättre prestanda, särskilt i miljöer som har stöd för snabb dataöverföring och frekvent I/O.

> [!Note] 
> Program i Azure Virtual Machines kan också ansluta till andra former av beständig lagring i Azure, som Azure Cosmos DB eller Azure Blob Storage.

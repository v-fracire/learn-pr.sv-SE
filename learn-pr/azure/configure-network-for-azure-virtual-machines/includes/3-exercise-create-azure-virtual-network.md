I den här övningen skapar du ett virtuellt nätverk i Microsoft Azure. Du kommer sedan att skapa två virtuella datorer och använda det virtuella nätverket för att ansluta de virtuella datorerna till varandra och till internet.

Innan du börjar den här enheten måste du måste logga in på [Azure Cloud Shell](https://shell.azure.com) med dina autentiseringsuppgifter för utvärderingsprenumeration. Vi använder Azure CLI via Azure Cloud Shell för att skapa resursgrupper, virtuella nätverk och virtuella datorer.

## <a name="create-a-resource-group"></a>Skapa en resursgrupp

1. I den **Välkommen till Azure Cloud Shell** fönstret klickar du på **Bash (Linux)**.

1. I fönstret **Du har ingen lagring monterad** klickar du på **Skapa lagring**.

1. Azure PowerShell-Kommandotolken, Skriv följande kod och tryck på RETUR. Ersätt den `<myResourceGroup>` värde med ett beskrivande namn som gör det enkelt att komma ihåg när du rensa alla resurser som skapas senare. Du använder det här namnet via återställning av den här övningen.

    ```azurecli
    az group create --name <myResourceGroup> --location eastus
    ```

## <a name="create-a-virtual-network"></a>Skapa ett virtuellt nätverk

1. Om du vill skapa ett virtuellt nätverk anger du följande kommando och trycker på Retur. Ersätt den `<myVirtualNetwork>` värde med ett beskrivande namn som gör det enkelt att komma ihåg

    ```azurecli
    az network vnet create --name <myVirtualNetwork> --resource-group <myResourceGroup> --subnet-name default
    ```

## <a name="create-two-virtual-machines"></a>Skapa två virtuella datorer

1. Om du vill skapa den första virtuella datorn kör du följande kommando för att skapa en virtuell Windows-dator med en offentlig IP-adress som kan nås via port 3389 (Remote Desktop). Den virtuella datorn namnet `dataProcStage1`:

    ```azurecli
    az vm create --name dataProcStage1 --resource-group <myResourceGroup> --admin-username "DataAdmin" --image Win2016Datacenter
    ```

1. Ange värden för ditt lösenord i meddelanderutorna. Kom ihåg att anteckna det här lösenordet eftersom du behöver dem senare för att få åtkomst till servern.

1. Nu ska du skapa den andra virtuella datorn. Den här virtuella datorn har inte en offentlig IP-adress. Kör följande kommando för att skapa en virtuell Windows-dator **utan** offentlig IP-adress med en tom sträng. Den virtuella datorn namnet `dataProcStage2`:

    ```azurecli
    az vm create -n dataProcStage2 -g <myResourceGroup> --public-ip-address "" --admin-username "DataAdmin" --image Win2016Datacenter
    ```

1. När du är klar returnerar utdatan från det andra kommandot ett värde för publicIpAddress.  

## <a name="connect-to-dataprocstage1-using-remote-desktop"></a>Ansluta till dataProcStage1 med hjälp av fjärrskrivbord

1. På klientdatorn trycker du på Windows-tangenten och skriver RDP.

1. Se till att **anslutning till fjärrskrivbord** app är markerad och tryck sedan på RETUR.

1. I den **anslutning till fjärrskrivbord** i dialogrutan den **datorn** fältet, anger du värdet för `dataProcStage1`är PublicIPAddress och klicka sedan på **Connect**.
    
    Anvisningar för att få fjärr-ip, om inte nedskrivna

1. I dialogrutan **Litar du på den här fjärranslutningen?** klickar du på **Anslut**.

1. I den **Windows Security** dialogrutan anger du användarnamn och lösenord som du använde när du skapade `dataProcStage1`. 

    Ändra profiler här

1. I dialogrutan **Anslutning till fjärrskrivbord** klickar du på **OK**.

1. Logga in på fjärrdatorn i Azure.

1. När meddelandet **Nätverk** visas klickar du på **Nej**.

1. Stäng Serverhanteraren.

1. I fjärrsessionen, högerklicka på Windows-tangenten och klicka på **kommandotolk**.

1. I kommandotolkens fönster anger du följande kommando och trycker på Retur.

    ```cmd
    ping dataProcStage2 -4
    ```

1. Det bör finnas något svar från `dataProcStage2`. Detta beror på att Windows-brandväggen förhindrar som standard ICMP-svar på `dataProcStage2`.

## <a name="connect-to-dataprocstage2-using-remote-desktop"></a>Ansluta till dataProcStage2 med hjälp av fjärrskrivbord

Konfigurerar du Windows-brandväggen på `dataProcStage2` med hjälp av en ny remote desktop seesion. Men du kommer inte att komma åt `dataProcStage2` från skrivbordet. Kom ihåg, `dataProcStage2` inte har en offentlig IP-adress. Du kommer att med hjälp av fjärrskrivbord från `dataProcStage1` att ansluta till `dataProcStage2`.

1. På `dataProcStage1`, tryck på Windows-tangenten och skriv **RDP**. Välj appen **Anslutning till fjärrskrivbord** och tryck på Retur.

1. I den **datorn** anger `dataProcStage2`, och klicka sedan på **Connect**. Baserat på standard-nätverkskonfiguration`dataProcStage1` kan matcha adress för `dataProcStage2` med hjälp av namnet på datorn.

1. I dialogrutan **Litar du på den här fjärranslutningen?** klickar du på **Anslut**.

1. I den **Windows Security** dialogrutan anger du användarnamn och lösenord som du använde när du skapade `dataProcStage2`.

1. I dialogrutan **Anslutning till fjärrskrivbord** klickar du på **OK**. Nu loggar du in på fjärrdatorn i Azure.

1. När meddelandet **Nätverk** visas klickar du på **Nej**.

1. Stäng Serverhanteraren.

1. På `dataProcStage2`, tryck på Windows, nyckeltypen **brandväggen**, och tryck på RETUR. Konsolen **Windows-brandväggen med avancerad säkerhet** visas.

1. I det vänstra fönstret klickar du på **Ingående regler**.

1. I den högra rutan, rulla nedåt och högerklicka på **File and Printer Sharing (ekobegäran - ICMPv4-In)**, och klicka sedan på **Aktivera regel**.

1. Gå tillbaka till den `dataProcStage1` konsolen och i kommandotolksfönstret skriver du följande kommando och tryck på RETUR.

    ```cmd
    ping dataProcStage2 -4
    ```

1. `dataProcStage2` svarar med fyra svar, vilket demonstrerar anslutning mellan de två virtuella datorerna.

## <a name="summary"></a>Sammanfattning

Du skapat har ett virtuellt nätverk, skapat två virtuella datorer som är kopplade till det virtuella nätverket, ansluten till någon av de virtuella datorerna och visas nätverksanslutning till den andra virtuella datorn i samma virtuella nätverk. Du kan använda Azure Virtual Network för att ansluta resurser i Azure-nätverket. Dessa resurser måste dock vara i samma resursgrupp och ingå i samma prenumeration. Nu ska ska vi titta på VPN-gatewayer som gör det möjligt att ansluta virtuella nätverk i olika resursgrupper, prenumerationer och även geografiska regioner.

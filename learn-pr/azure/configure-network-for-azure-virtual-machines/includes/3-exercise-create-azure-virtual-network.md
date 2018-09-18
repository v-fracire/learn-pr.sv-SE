I den här övningen skapar du ett virtuellt nätverk i Microsoft Azure. Du kommer sedan att skapa två virtuella datorer och använda det virtuella nätverket för att ansluta de virtuella datorerna till varandra och till internet.

Innan du påbörjar den här delen behöver du logga in på [Azure Cloud Shell](https://shell.azure.com) med dina autentiseringsuppgifter för utvärderingsprenumerationen. Vi använder Azure Cloud Shell till att skapa resursgrupper, virtuella nätverk och virtuella datorer.

## <a name="create-a-resource-group"></a>Skapa en resursgrupp

1. I fönstret **Välkommen till Azure Cloud Shell** klickar du på **PowerShell (Linux)**.

1. I fönstret **Du har ingen lagring monterad** klickar du på **Skapa lagring**.

1. På kommandoraden i Azure PowerShell anger du följande kod och trycker på Retur.

    ```PowerShell
    az group create --name myResourceGroup --location eastus
    ```

## <a name="create-a-virtual-network"></a>Skapa ett virtuellt nätverk

1. Om du vill skapa ett virtuellt nätverk anger du följande kommando och trycker på Retur.

    ```PowerShell
    az network vnet create --name myVirtualNetwork --resource-group myResourceGroup --subnet-name default
    ```

## <a name="create-two-virtual-machines"></a>Skapa två virtuella datorer

1. Om du vill skapa den första virtuella datorn kör du följande kommando för att skapa en virtuell Windows-dator med en offentlig IP-adress som kan nås via port 3389 (Fjärrskrivbord):

    ``` PowerShell
    az vm create --name dataProcessingStage1 --resource-group MyResourceGroup --admin-username "DataAdmin"--image Win2016Datacenter
    ```

1. Ange värden för ditt lösenord i meddelanderutorna.

1. Kör följande kommando för att skapa en virtuell Windows-dator utan en offentlig IP-adress:

    ```PowerShell
    az vm create -n dataProcessingStage2 -g MyResourceGroup --public-ip-address '' --admin-username "DataAdmin"--image Win2016Datacenter
    ```

1. När du är klar returnerar utdatan från det andra kommandot ett värde för publicIpAddress.

## <a name="connect-to-dataprocessingstage1-using-remote-desktop"></a>Anslut till dataProcessingStage1 med hjälp av fjärrskrivbordet

1. På klientdatorn trycker du på Windows-tangenten och skriver RDP.

1. Se till att appen **Anslutning till fjärrskrivbord** har valts och tryck sedan på Retur.

1. I dialogrutan **Anslutning till fjärrskrivbord**, i fältet **Dator**, anger du värdet dataProcessingStage1PublicIPAddress och klickar sedan på **Anslut**.

1. I dialogrutan **Litar du på den här fjärranslutningen?** klickar du på **Anslut**.

1. I dialogrutan **Windows-säkerhet** anger du användarnamnet och lösenordet du använde när du skapade dataProcessingStage1.

1. I dialogrutan **Anslutning till fjärrskrivbord** klickar du på **OK**.

1. Logga in på fjärrdatorn i Azure.

1. När meddelandet **Nätverk** visas klickar du på **Nej**.

1. Stäng Serverhanteraren.

1. I fjärrsessionen högerklickar du på Windows-tangenten och klickar på **kommandotolken**.

1. I kommandotolkens fönster anger du följande kommando och trycker på Retur.

    ```cmd
    ping dataProcessingStage2 -4
    ```

1. Det bör inte finnas något svar från fjärrdatorn. Det beror på att Windows-brandväggen som standard förhindrar ICMP-svar.

## <a name="connect-to-dataprocessingstage2-using-remote-desktop"></a>Anslut till dataProcessingStage2 med hjälp av fjärrskrivbordet

1. På klientdatorn trycker du på Windows-tangenten och skriver **RDP**. Välj appen **Anslutning till fjärrskrivbord** och tryck på Retur.

1. I fältet **Dator** anger du värdet dataProcessingStage2PublicIPAddress och klickar på **Anslut**.

1. I dialogrutan **Litar du på den här fjärranslutningen?** klickar du på **Anslut**.

1. I dialogrutan **Windows-säkerhet** anger du användarnamnet och lösenordet du använde när du skapade dataProcessingStage2.

1. I dialogrutan **Anslutning till fjärrskrivbord** klickar du på **OK**. Nu loggar du in på fjärrdatorn i Azure.

1. När meddelandet **Nätverk** visas klickar du på **Nej**.

1. Stäng Serverhanteraren.

1. På dataProcessingStage2 trycker du på Windows-tangenten. Ange sedan **Brandvägg** och tryck på Retur. Konsolen **Windows-brandväggen med avancerad säkerhet** visas.

1. I det vänstra fönstret klickar du på **Ingående regler**.

1. I det högra fönstret bläddrar du ned till och högerklickar på **Fil- och skrivardelning (ekobegäran – ICMPv4-In)**. Klicka sedan på **Aktivera regel**.

1. Växla tillbaka till konsolen dataProcessingStage1, ange följande kommando i kommandotolken och tryck på Retur.

    ```cmd
    ping dataProcessingStage2 -4
    ```

1. dataProcessingStage2 svarar med fyra svar, vilket demonstrerar anslutning mellan de två virtuella datorerna.

## <a name="summary"></a>Sammanfattning

Du har skapat ett virtuellt nätverk och två virtuella datorer som är kopplade till det virtuella nätverket, anslutit till en av de virtuella datorerna och visat nätverksanslutningen till den andra virtuella datorn i samma virtuella nätverk. Du kan använda virtuella Azure-nätverk till att ansluta resurser i Azure-nätverket. Dessa resurser måste dock vara i samma resursgrupp och ingå i samma prenumeration. Nu ska vi titta på VPN-gatewayer som gör det möjligt att ansluta till virtuella nätverk i olika resursgrupper, prenumerationer och även geografiska regioner.

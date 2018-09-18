<span data-ttu-id="f7c85-101">I den här övningen skapar du ett virtuellt nätverk i Microsoft Azure.</span><span class="sxs-lookup"><span data-stu-id="f7c85-101">In this exercise, you will create a virtual network in Microsoft Azure.</span></span> <span data-ttu-id="f7c85-102">Du kommer sedan att skapa två virtuella datorer och använda det virtuella nätverket för att ansluta de virtuella datorerna till varandra och till internet.</span><span class="sxs-lookup"><span data-stu-id="f7c85-102">You will then create two virtual machines and use the virtual network to connect the virtual machines to each other and to the internet.</span></span>

<span data-ttu-id="f7c85-103">Innan du påbörjar den här delen behöver du logga in på [Azure Cloud Shell](https://shell.azure.com) med dina autentiseringsuppgifter för utvärderingsprenumerationen.</span><span class="sxs-lookup"><span data-stu-id="f7c85-103">Before you begin this unit, you will need to log into [Azure Cloud Shell](https://shell.azure.com) with your trial subscription credentials.</span></span> <span data-ttu-id="f7c85-104">Vi använder Azure Cloud Shell till att skapa resursgrupper, virtuella nätverk och virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="f7c85-104">We will use Azure Cloud Shell to create the resource groups, virtual networks, and virtual machines.</span></span>

## <a name="create-a-resource-group"></a><span data-ttu-id="f7c85-105">Skapa en resursgrupp</span><span class="sxs-lookup"><span data-stu-id="f7c85-105">Create a resource group</span></span>

1. <span data-ttu-id="f7c85-106">I fönstret **Välkommen till Azure Cloud Shell** klickar du på **PowerShell (Linux)**.</span><span class="sxs-lookup"><span data-stu-id="f7c85-106">In the **Welcome to Azure Cloud Shell** window, click **PowerShell (Linux)**.</span></span>

1. <span data-ttu-id="f7c85-107">I fönstret **Du har ingen lagring monterad** klickar du på **Skapa lagring**.</span><span class="sxs-lookup"><span data-stu-id="f7c85-107">In the **you have no storage mounted** window, click **Create Storage**.</span></span>

1. <span data-ttu-id="f7c85-108">På kommandoraden i Azure PowerShell anger du följande kod och trycker på Retur.</span><span class="sxs-lookup"><span data-stu-id="f7c85-108">In the Azure PowerShell command-line prompt, type the following code and press Enter.</span></span>

    ```PowerShell
    az group create --name myResourceGroup --location eastus
    ```

## <a name="create-a-virtual-network"></a><span data-ttu-id="f7c85-109">Skapa ett virtuellt nätverk</span><span class="sxs-lookup"><span data-stu-id="f7c85-109">Create a virtual network</span></span>

1. <span data-ttu-id="f7c85-110">Om du vill skapa ett virtuellt nätverk anger du följande kommando och trycker på Retur.</span><span class="sxs-lookup"><span data-stu-id="f7c85-110">To create a virtual network, enter the following command and press Enter.</span></span>

    ```PowerShell
    az network vnet create --name myVirtualNetwork --resource-group myResourceGroup --subnet-name default
    ```

## <a name="create-two-virtual-machines"></a><span data-ttu-id="f7c85-111">Skapa två virtuella datorer</span><span class="sxs-lookup"><span data-stu-id="f7c85-111">Create two virtual machines</span></span>

1. <span data-ttu-id="f7c85-112">Om du vill skapa den första virtuella datorn kör du följande kommando för att skapa en virtuell Windows-dator med en offentlig IP-adress som kan nås via port 3389 (Fjärrskrivbord):</span><span class="sxs-lookup"><span data-stu-id="f7c85-112">To create the first virtual machine, execute the following command to create a Windows VM with a public IP address that is accessible over port 3389 (Remote Desktop):</span></span>

    ``` PowerShell
    az vm create --name dataProcessingStage1 --resource-group MyResourceGroup --admin-username "DataAdmin"--image Win2016Datacenter
    ```

1. <span data-ttu-id="f7c85-113">Ange värden för ditt lösenord i meddelanderutorna.</span><span class="sxs-lookup"><span data-stu-id="f7c85-113">Supply values for your password at the prompts.</span></span>

1. <span data-ttu-id="f7c85-114">Kör följande kommando för att skapa en virtuell Windows-dator utan en offentlig IP-adress:</span><span class="sxs-lookup"><span data-stu-id="f7c85-114">Execute the following command to create a Windows VM without a public IP address:</span></span>

    ```PowerShell
    az vm create -n dataProcessingStage2 -g MyResourceGroup --public-ip-address '' --admin-username "DataAdmin"--image Win2016Datacenter
    ```

1. <span data-ttu-id="f7c85-115">När du är klar returnerar utdatan från det andra kommandot ett värde för publicIpAddress.</span><span class="sxs-lookup"><span data-stu-id="f7c85-115">When completed, the output from the second command will return a value for publicIpAddress.</span></span>

## <a name="connect-to-dataprocessingstage1-using-remote-desktop"></a><span data-ttu-id="f7c85-116">Anslut till dataProcessingStage1 med hjälp av fjärrskrivbordet</span><span class="sxs-lookup"><span data-stu-id="f7c85-116">Connect to dataProcessingStage1 using Remote Desktop</span></span>

1. <span data-ttu-id="f7c85-117">På klientdatorn trycker du på Windows-tangenten och skriver RDP.</span><span class="sxs-lookup"><span data-stu-id="f7c85-117">On your client computer, press the Windows key and type RDP.</span></span>

1. <span data-ttu-id="f7c85-118">Se till att appen **Anslutning till fjärrskrivbord** har valts och tryck sedan på Retur.</span><span class="sxs-lookup"><span data-stu-id="f7c85-118">Ensure that **Remote Desktop Connection** app is selected, and then press Enter.</span></span>

1. <span data-ttu-id="f7c85-119">I dialogrutan **Anslutning till fjärrskrivbord**, i fältet **Dator**, anger du värdet dataProcessingStage1PublicIPAddress och klickar sedan på **Anslut**.</span><span class="sxs-lookup"><span data-stu-id="f7c85-119">In the **Remote Desktop Connection** dialog box, in the **Computer** field, enter the value of dataProcessingStage1PublicIPAddress, and then click **Connect**.</span></span>

1. <span data-ttu-id="f7c85-120">I dialogrutan **Litar du på den här fjärranslutningen?** klickar du på **Anslut**.</span><span class="sxs-lookup"><span data-stu-id="f7c85-120">In the **Do you trust this remote connection?** dialog box, click **Connect**.</span></span>

1. <span data-ttu-id="f7c85-121">I dialogrutan **Windows-säkerhet** anger du användarnamnet och lösenordet du använde när du skapade dataProcessingStage1.</span><span class="sxs-lookup"><span data-stu-id="f7c85-121">In the **Windows Security** dialog box, enter the user name and password you used when you created dataProcessingStage1.</span></span>

1. <span data-ttu-id="f7c85-122">I dialogrutan **Anslutning till fjärrskrivbord** klickar du på **OK**.</span><span class="sxs-lookup"><span data-stu-id="f7c85-122">In the **Remote Desktop Connection** dialog box, click **OK**.</span></span>

1. <span data-ttu-id="f7c85-123">Logga in på fjärrdatorn i Azure.</span><span class="sxs-lookup"><span data-stu-id="f7c85-123">Sign in to the remote computer in Azure.</span></span>

1. <span data-ttu-id="f7c85-124">När meddelandet **Nätverk** visas klickar du på **Nej**.</span><span class="sxs-lookup"><span data-stu-id="f7c85-124">When the **Networks** message appears, click **No**.</span></span>

1. <span data-ttu-id="f7c85-125">Stäng Serverhanteraren.</span><span class="sxs-lookup"><span data-stu-id="f7c85-125">Close Server Manager.</span></span>

1. <span data-ttu-id="f7c85-126">I fjärrsessionen högerklickar du på Windows-tangenten och klickar på **kommandotolken**.</span><span class="sxs-lookup"><span data-stu-id="f7c85-126">In the remote session, right-click the Windows key and click **Command Prompt**.</span></span>

1. <span data-ttu-id="f7c85-127">I kommandotolkens fönster anger du följande kommando och trycker på Retur.</span><span class="sxs-lookup"><span data-stu-id="f7c85-127">In the command prompt window, type the following command and press Enter.</span></span>

    ```cmd
    ping dataProcessingStage2 -4
    ```

1. <span data-ttu-id="f7c85-128">Det bör inte finnas något svar från fjärrdatorn.</span><span class="sxs-lookup"><span data-stu-id="f7c85-128">There should be no response from the remote computer.</span></span> <span data-ttu-id="f7c85-129">Det beror på att Windows-brandväggen som standard förhindrar ICMP-svar.</span><span class="sxs-lookup"><span data-stu-id="f7c85-129">This is because by default, Windows Firewall prevents ICMP responses.</span></span>

## <a name="connect-to-dataprocessingstage2-using-remote-desktop"></a><span data-ttu-id="f7c85-130">Anslut till dataProcessingStage2 med hjälp av fjärrskrivbordet</span><span class="sxs-lookup"><span data-stu-id="f7c85-130">Connect to dataProcessingStage2 using Remote Desktop</span></span>

1. <span data-ttu-id="f7c85-131">På klientdatorn trycker du på Windows-tangenten och skriver **RDP**.</span><span class="sxs-lookup"><span data-stu-id="f7c85-131">On your client computer, press the Windows key and type **RDP**.</span></span> <span data-ttu-id="f7c85-132">Välj appen **Anslutning till fjärrskrivbord** och tryck på Retur.</span><span class="sxs-lookup"><span data-stu-id="f7c85-132">Select the **Remote Desktop Connection** app and press Enter.</span></span>

1. <span data-ttu-id="f7c85-133">I fältet **Dator** anger du värdet dataProcessingStage2PublicIPAddress och klickar på **Anslut**.</span><span class="sxs-lookup"><span data-stu-id="f7c85-133">In the **Computer** field, enter the value of dataProcessingStage2PublicIPAddress, and then click **Connect**.</span></span>

1. <span data-ttu-id="f7c85-134">I dialogrutan **Litar du på den här fjärranslutningen?** klickar du på **Anslut**.</span><span class="sxs-lookup"><span data-stu-id="f7c85-134">In the **Do you trust this remote connection?** dialog box, click **Connect**.</span></span>

1. <span data-ttu-id="f7c85-135">I dialogrutan **Windows-säkerhet** anger du användarnamnet och lösenordet du använde när du skapade dataProcessingStage2.</span><span class="sxs-lookup"><span data-stu-id="f7c85-135">In the **Windows Security** dialog box, enter the user name and password you used when you created dataProcessingStage2.</span></span>

1. <span data-ttu-id="f7c85-136">I dialogrutan **Anslutning till fjärrskrivbord** klickar du på **OK**.</span><span class="sxs-lookup"><span data-stu-id="f7c85-136">In the **Remote Desktop Connection** dialog box, click **OK**.</span></span> <span data-ttu-id="f7c85-137">Nu loggar du in på fjärrdatorn i Azure.</span><span class="sxs-lookup"><span data-stu-id="f7c85-137">You now sign in to the remote computer in Azure.</span></span>

1. <span data-ttu-id="f7c85-138">När meddelandet **Nätverk** visas klickar du på **Nej**.</span><span class="sxs-lookup"><span data-stu-id="f7c85-138">When the **Networks** message appears, click **No**.</span></span>

1. <span data-ttu-id="f7c85-139">Stäng Serverhanteraren.</span><span class="sxs-lookup"><span data-stu-id="f7c85-139">Close Server Manager.</span></span>

1. <span data-ttu-id="f7c85-140">På dataProcessingStage2 trycker du på Windows-tangenten. Ange sedan **Brandvägg** och tryck på Retur.</span><span class="sxs-lookup"><span data-stu-id="f7c85-140">On dataProcessingStage2, press the Windows key, type **Firewall**, and press Enter.</span></span> <span data-ttu-id="f7c85-141">Konsolen **Windows-brandväggen med avancerad säkerhet** visas.</span><span class="sxs-lookup"><span data-stu-id="f7c85-141">The **Windows Firewall with Advanced Security** console appears.</span></span>

1. <span data-ttu-id="f7c85-142">I det vänstra fönstret klickar du på **Ingående regler**.</span><span class="sxs-lookup"><span data-stu-id="f7c85-142">In the left-hand pane, click **Inbound Rules**.</span></span>

1. <span data-ttu-id="f7c85-143">I det högra fönstret bläddrar du ned till och högerklickar på **Fil- och skrivardelning (ekobegäran – ICMPv4-In)**. Klicka sedan på **Aktivera regel**.</span><span class="sxs-lookup"><span data-stu-id="f7c85-143">In the right-hand pane, scroll down and right-click **File and Printer Sharing (Echo Request - ICMPv4-In)**, and then click **Enable Rule**.</span></span>

1. <span data-ttu-id="f7c85-144">Växla tillbaka till konsolen dataProcessingStage1, ange följande kommando i kommandotolken och tryck på Retur.</span><span class="sxs-lookup"><span data-stu-id="f7c85-144">Switch back to the dataProcessingStage1 console and in the command prompt window, type the following command and press Enter.</span></span>

    ```cmd
    ping dataProcessingStage2 -4
    ```

1. <span data-ttu-id="f7c85-145">dataProcessingStage2 svarar med fyra svar, vilket demonstrerar anslutning mellan de två virtuella datorerna.</span><span class="sxs-lookup"><span data-stu-id="f7c85-145">dataProcessingStage2 responds with four replies, demonstrating connectivity between the two VMs.</span></span>

## <a name="summary"></a><span data-ttu-id="f7c85-146">Sammanfattning</span><span class="sxs-lookup"><span data-stu-id="f7c85-146">Summary</span></span>

<span data-ttu-id="f7c85-147">Du har skapat ett virtuellt nätverk och två virtuella datorer som är kopplade till det virtuella nätverket, anslutit till en av de virtuella datorerna och visat nätverksanslutningen till den andra virtuella datorn i samma virtuella nätverk.</span><span class="sxs-lookup"><span data-stu-id="f7c85-147">You have successfully created a virtual network, created two VMs that are attached to that virtual network, connected to one of the VMs and shown network connectivity to the other VM within the same virtual network.</span></span> <span data-ttu-id="f7c85-148">Du kan använda virtuella Azure-nätverk till att ansluta resurser i Azure-nätverket.</span><span class="sxs-lookup"><span data-stu-id="f7c85-148">You can use Azure Virtual Network to connect resources within the Azure network.</span></span> <span data-ttu-id="f7c85-149">Dessa resurser måste dock vara i samma resursgrupp och ingå i samma prenumeration.</span><span class="sxs-lookup"><span data-stu-id="f7c85-149">However, those resources need to be within the same resource group and subscription.</span></span> <span data-ttu-id="f7c85-150">Nu ska vi titta på VPN-gatewayer som gör det möjligt att ansluta till virtuella nätverk i olika resursgrupper, prenumerationer och även geografiska regioner.</span><span class="sxs-lookup"><span data-stu-id="f7c85-150">Next, we will look at VPN gateways, which enable you to connect virtual network in different resource groups, subscriptions, and even geographical regions.</span></span>

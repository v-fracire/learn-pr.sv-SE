<span data-ttu-id="a816c-101">I den här kursdelen installerar du Node.js – **N** i förkortningen MEAN – på en Azure-värdbaserad virtuell Ubuntu Linux-dator.</span><span class="sxs-lookup"><span data-stu-id="a816c-101">In this unit, you will install Node.js - the **N** in the MEAN acronym - on an Azure-hosted Ubuntu Linux virtual machine.</span></span> <span data-ttu-id="a816c-102">Node.js kommer att fungera som CLR-stöd för hanteringen av vår HTTP-trafik och som värd för webbappen.</span><span class="sxs-lookup"><span data-stu-id="a816c-102">Node.js will serve as our runtime for handling our HTTP traffic and hosting our web application.</span></span>

## <a name="connect-to-the-vm"></a><span data-ttu-id="a816c-103">Ansluta till den virtuella datorn</span><span class="sxs-lookup"><span data-stu-id="a816c-103">Connect to the VM</span></span>

<span data-ttu-id="a816c-104">För att kunna installera Node.js måste du ansluta till den virtuella datorn med hjälp av **ssh**.</span><span class="sxs-lookup"><span data-stu-id="a816c-104">In order to install Node.js, you have to connect to the VM using **ssh**.</span></span> <span data-ttu-id="a816c-105">Om du inte fortfarande är ansluten till den virtuella datorn kör du följande kommando.</span><span class="sxs-lookup"><span data-stu-id="a816c-105">If you aren't still connected to your VM, run the following command.</span></span> <span data-ttu-id="a816c-106">Ange administratörsanvändarnamnet och den virtuella datorns offentliga IP-adress i platshållarna `<vm-admin-username>` och `<vm-public-ip>`.</span><span class="sxs-lookup"><span data-stu-id="a816c-106">Substitute your admin username and your VM's public IP address from above for the `<vm-admin-username>` and `<vm-public-ip>` placeholders.</span></span>

```bash
ssh <vm-admin-username>@<vm-public-ip>
```

## <a name="install-nodejs"></a><span data-ttu-id="a816c-107">Installera Node.js</span><span class="sxs-lookup"><span data-stu-id="a816c-107">Install Node.js</span></span>

> [!Important]
> <span data-ttu-id="a816c-108">Ubuntu tillhandahåller ett inofficiell paket som heter **Node.js-legacy**.</span><span class="sxs-lookup"><span data-stu-id="a816c-108">Ubuntu provides an unofficial package called **Node.js-legacy**.</span></span> <span data-ttu-id="a816c-109">Det här paketet underhålls inte av Node.js och är inaktuellt.</span><span class="sxs-lookup"><span data-stu-id="a816c-109">This package is not maintained by Node.js and is outdated.</span></span>

1. <span data-ttu-id="a816c-110">Registrera Node.js-paketlagringsplatsen så att **apt-get** kan hitta rätt paket att installera på den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="a816c-110">Register the Node.js package repository, so **apt-get** can find the right package to install on your virtual machine.</span></span>

    ```bash
    curl -sL https://deb.nodesource.com/setup_8.x | sudo -E bash -
    ```

1. <span data-ttu-id="a816c-111">Installera Node.js-paketet på Linux-datorn.</span><span class="sxs-lookup"><span data-stu-id="a816c-111">Install the Node.js package on your Linux system.</span></span>

    ```bash
    sudo apt-get install -y Node.js
    ```

1. <span data-ttu-id="a816c-112">Verifiera att Node.js-installationen lyckades genom att köra följande enkla Node.js-kommando.</span><span class="sxs-lookup"><span data-stu-id="a816c-112">Verify the Node.js installation succeeded by running the following simple Node.js command.</span></span>

    ```bash
    node -v
    ```

    <span data-ttu-id="a816c-113">Utdata bör se ut ungefär som `v8.11.4`, där versionen speglar den senast tillgängliga Node.js-versionen när du installerar paketet.</span><span class="sxs-lookup"><span data-stu-id="a816c-113">The output should be something like `v8.11.4`, with the version reflecting the latest Node.js version that's available when you install the package.</span></span>

## <a name="summary"></a><span data-ttu-id="a816c-114">Sammanfattning</span><span class="sxs-lookup"><span data-stu-id="a816c-114">Summary</span></span>

<span data-ttu-id="a816c-115">Med Node.js installerat på den virtuella datorn kan vi börja skapa ett webbprogram som att det blir ansvarigt för att köra.</span><span class="sxs-lookup"><span data-stu-id="a816c-115">With Node.js installed on your virtual machine, we can start building a web application that it will be responsible for running.</span></span>
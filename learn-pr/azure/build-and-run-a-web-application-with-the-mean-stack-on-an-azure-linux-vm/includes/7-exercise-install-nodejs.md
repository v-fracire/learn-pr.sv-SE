<span data-ttu-id="46cff-101">I den här övningen installerar du Node.js – **N** i förkortningen MEAN – på en Azure-värdbaserad virtuell Ubuntu Linux-dator.</span><span class="sxs-lookup"><span data-stu-id="46cff-101">In this exercise, you will install Node.js - the **N** in the MEAN acronym - on an Azure-hosted Ubuntu Linux virtual machine.</span></span> <span data-ttu-id="46cff-102">Node.js kommer att fungera som runtime för att hantera vår HTTP-trafik och som värd för webbappen.</span><span class="sxs-lookup"><span data-stu-id="46cff-102">Node.js will serve as our runtime for handling our HTTP traffic and hosting our web application.</span></span>

## <a name="connect-to-the-vm"></a><span data-ttu-id="46cff-103">Ansluta till den virtuella datorn</span><span class="sxs-lookup"><span data-stu-id="46cff-103">Connect to the VM</span></span>

<span data-ttu-id="46cff-104">För att kunna installera Node.js måste du ansluta till den virtuella datorn med hjälp av **ssh**.</span><span class="sxs-lookup"><span data-stu-id="46cff-104">In order to install Node.js, you have to connect to the VM using **ssh**.</span></span> <span data-ttu-id="46cff-105">Om du inte fortfarande är ansluten till den virtuella datorn kör du följande kommando.</span><span class="sxs-lookup"><span data-stu-id="46cff-105">If you aren't still connected to your VM, run the following command.</span></span> <span data-ttu-id="46cff-106">Ange administratörsanvändarnamnet och den virtuella datorns offentliga IP-adress i platshållarna `<vm-admin-username>` och `<vm-public-ip>`.</span><span class="sxs-lookup"><span data-stu-id="46cff-106">Substitute your admin username and your VM's public IP address from above for the `<vm-admin-username>` and `<vm-public-ip>` placeholders.</span></span>

```bash
ssh <vm-admin-username>@<vm-public-ip>
```

## <a name="install-nodejs"></a><span data-ttu-id="46cff-107">Installera Node.js</span><span class="sxs-lookup"><span data-stu-id="46cff-107">Install Node.js</span></span>

> [!Important]
> <span data-ttu-id="46cff-108">Ubuntu tillhandahåller ett inofficiellt paket som heter **Node.js-legacy**, men det här paketet underhålls inte av Node.js och är inaktuellt.</span><span class="sxs-lookup"><span data-stu-id="46cff-108">Ubuntu provides an unofficial package called **Node.js-legacy**, but this package is not maintained by Node.js and is outdated.</span></span>

1. <span data-ttu-id="46cff-109">Registrera Node.js-paketlagringsplatsen så att **apt-get** kan hitta rätt paket att installera på den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="46cff-109">Register the Node.js package repository so **apt-get** can find the right package to install on your virtual machine.</span></span>

    ```bash
    curl -sL https://deb.nodesource.com/setup_8.x | sudo -E bash -
    ```

1. <span data-ttu-id="46cff-110">Installera Node.js-paketet på Linux-datorn.</span><span class="sxs-lookup"><span data-stu-id="46cff-110">Install the Node.js package on your Linux system.</span></span>

    ```bash
    sudo apt-get install -y Node.js
    ```

1. <span data-ttu-id="46cff-111">Verifiera att Node.js-installationen lyckades genom att köra följande enkla Node.js-kommando.</span><span class="sxs-lookup"><span data-stu-id="46cff-111">Verify the Node.js installation succeeded by running the following simple Node.js command.</span></span>

    ```bash
    node -v
    ```

    <span data-ttu-id="46cff-112">Utdata bör se ut ungefär som `v8.11.4`, där versionen speglar den senast tillgängliga Node.js-versionen i när du installerar paketet.</span><span class="sxs-lookup"><span data-stu-id="46cff-112">The output should be something like `v8.11.4`, with the version reflecting the latest Node.js version available when you install the package.</span></span>

## <a name="summary"></a><span data-ttu-id="46cff-113">Sammanfattning</span><span class="sxs-lookup"><span data-stu-id="46cff-113">Summary</span></span>

<span data-ttu-id="46cff-114">Med Node.js installerat på den virtuella datorn kan vi börja skapa ett webbprogram som att det blir ansvarigt för att köra.</span><span class="sxs-lookup"><span data-stu-id="46cff-114">With Node.js installed on your virtual machine, we can start building a web application that it will be responsible for running.</span></span>

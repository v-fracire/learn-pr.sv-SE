<span data-ttu-id="de7b1-101">I den här kursdelen installerar du Node.js – **N** i förkortningen MEAN – på en Azure-värdbaserad virtuell Ubuntu Linux-dator.</span><span class="sxs-lookup"><span data-stu-id="de7b1-101">In this unit, you will install Node.js - the **N** in the MEAN acronym - on an Azure-hosted Ubuntu Linux virtual machine.</span></span> <span data-ttu-id="de7b1-102">Node.js kommer att fungera som CLR-stöd för hanteringen av vår HTTP-trafik och som värd för webbappen.</span><span class="sxs-lookup"><span data-stu-id="de7b1-102">Node.js will serve as our runtime for handling our HTTP traffic and hosting our web application.</span></span>

## <a name="install-nodejs"></a><span data-ttu-id="de7b1-103">Installera Node.js</span><span class="sxs-lookup"><span data-stu-id="de7b1-103">Install Node.js</span></span>

1. <span data-ttu-id="de7b1-104">**Om du fortfarande inte är ansluten med SSH till den virtuella datorn från den föregående övningen**, ska du ansluta nu.</span><span class="sxs-lookup"><span data-stu-id="de7b1-104">**If you aren't still SSHed into your VM from the previous exercise**, SSH in.</span></span>

    ```bash
    ssh <vm-admin-username>@<vm-public-ip>
    ```

1. <span data-ttu-id="de7b1-105">Registrera Node.js-paketlagringsplatsen så att **apt-get** kan hitta rätt paket att installera på den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="de7b1-105">Register the Node.js package repository so **apt-get** can find the right package to install on your virtual machine.</span></span>

    ```bash
    curl -sL https://deb.nodesource.com/setup_8.x | sudo -E bash -
    ```

1. <span data-ttu-id="de7b1-106">Installera Node.js-paketet på Linux-datorn.</span><span class="sxs-lookup"><span data-stu-id="de7b1-106">Install the Node.js package on your Linux system.</span></span>

    ```bash
    sudo apt-get install -y Node.js
    ```

1. <span data-ttu-id="de7b1-107">Verifiera att Node.js-installationen lyckades genom att köra följande enkla Node.js-kommando.</span><span class="sxs-lookup"><span data-stu-id="de7b1-107">Verify the Node.js installation succeeded by running the following simple Node.js command.</span></span>

    ```bash
    node -v
    ```

    <span data-ttu-id="de7b1-108">Utdata bör se ut ungefär som `v8.11.4`, där versionen speglar den senast tillgängliga Node.js-versionen när du installerar paketet.</span><span class="sxs-lookup"><span data-stu-id="de7b1-108">The output should be something like `v8.11.4`, with the version reflecting the latest Node.js version that's available when you install the package.</span></span>

## <a name="summary"></a><span data-ttu-id="de7b1-109">Sammanfattning</span><span class="sxs-lookup"><span data-stu-id="de7b1-109">Summary</span></span>

<span data-ttu-id="de7b1-110">Med Node.js installerat på den virtuella datorn kan vi börja skapa ett webbprogram som att det blir ansvarigt för att köra.</span><span class="sxs-lookup"><span data-stu-id="de7b1-110">With Node.js installed on your virtual machine, we can start building a web application that it will be responsible for running.</span></span>
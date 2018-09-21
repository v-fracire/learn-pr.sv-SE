<span data-ttu-id="b51ce-101">Nu när din Data Science Virtual Machine är igång beslutar du dig för att träna din första djupinlärningsmodell.</span><span class="sxs-lookup"><span data-stu-id="b51ce-101">Now that you have your Data Science Virtual Machine up and running, you decide to train your first deep learning model.</span></span> <span data-ttu-id="b51ce-102">Du vill köra dina experiment avskilt från allt annat på servern.</span><span class="sxs-lookup"><span data-stu-id="b51ce-102">You want to run your experiments in isolation from everything else on your server.</span></span> <span data-ttu-id="b51ce-103">För att göra det kör du dem i en Docker-container.</span><span class="sxs-lookup"><span data-stu-id="b51ce-103">To do that, you run them within a Docker container.</span></span>

## <a name="connect-to-the-vm-with-ssh"></a><span data-ttu-id="b51ce-104">Ansluta till den virtuella datorn med SSH</span><span class="sxs-lookup"><span data-stu-id="b51ce-104">Connect to the VM with ssh</span></span>

<span data-ttu-id="b51ce-105">Kontrollera att du är fortfarande ansluten till den virtuella datorn via SSH.</span><span class="sxs-lookup"><span data-stu-id="b51ce-105">Make sure you're still connected to your VM through ssh.</span></span> <span data-ttu-id="b51ce-106">Om inte så kör du det här kommandot igen för att fjärransluta tillbaka till den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="b51ce-106">If not, just run this command again to remote back into the virtual machine.</span></span>

1. <span data-ttu-id="b51ce-107">Kör följande kommando i Azure Cloud Shell för att logga in på den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="b51ce-107">Execute the following command in Azure Cloud Shell to sign into the VM.</span></span>

    ```azurecli 
    ssh <USERNAME>@<IP>
    ``` 
    
    <span data-ttu-id="b51ce-108">Ersätt `<USERNAME>` med det administratörskontonamn som du definierade när du skapade den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="b51ce-108">Replace  `<USERNAME>` with the admin account name you defined when creating the VM.</span></span> <span data-ttu-id="b51ce-109">Ersätt `<IP>` med IP-adressen för den virtuella dator som du sparade i föregående övning.</span><span class="sxs-lookup"><span data-stu-id="b51ce-109">Replace `<IP>` with the IP address of the VM you saved in the preceding exercise.</span></span>  

1. <span data-ttu-id="b51ce-110">När du uppmanas anger du lösenordet för administratörskontot för att slutföra inloggningen.</span><span class="sxs-lookup"><span data-stu-id="b51ce-110">When prompted, enter your password for the admin account to complete the sign-in process.</span></span>

## <a name="run-a-jupyter-notebook-server-in-a-docker-container-in-the-vm"></a><span data-ttu-id="b51ce-111">Köra en Jupyter Notebook-server i en Docker-container i den virtuella datorn</span><span class="sxs-lookup"><span data-stu-id="b51ce-111">Run a Jupyter Notebook server in a Docker container in the VM</span></span>

> [!NOTE]
> <span data-ttu-id="b51ce-112">Eftersom vi bara har ett administratörskonto, eller rotkonto, på den virtuella datorn måste vi köra alla Docker-kommandon som rot med hjälp av `sudo`</span><span class="sxs-lookup"><span data-stu-id="b51ce-112">Since we only have an admin, or root, account on our VM, we have to run all Docker commands as root using `sudo`</span></span>

1. <span data-ttu-id="b51ce-113">Om du vill se vilka Docker-containrar som finns på den virtuella datorn kör du följande kommando i kommandotolken.</span><span class="sxs-lookup"><span data-stu-id="b51ce-113">To see what Docker containers exist on the VM, run the following command at the command prompt.</span></span>

    ```azurecli 
    sudo docker ps
    ```

1. <span data-ttu-id="b51ce-114">Kör följande kommando i kommandotolken för att skapa en ny container för våra experiment.</span><span class="sxs-lookup"><span data-stu-id="b51ce-114">Run the following command at the command prompt to create a new container for our experiments.</span></span>

    ```azurecli 
    sudo docker run --rm -it --entrypoint '/bin/sh' -p 8888:8888 pytorch/pytorch -c \
    'conda install jupyter matplotlib -y &&\
    curl https://pytorch.org/tutorials/_downloads/cifar10_tutorial.ipynb > first_pytorch_classifier.ipynb &&\
    jupyter notebook --ip=0.0.0.0 --no-browser --allow-root'
    ``` 

    <span data-ttu-id="b51ce-115">Det här kommandot kommer att köras ett tag.</span><span class="sxs-lookup"><span data-stu-id="b51ce-115">This command will run for quite a while.</span></span> <span data-ttu-id="b51ce-116">Därför passar vi på att diskutera vad kommandot gör.</span><span class="sxs-lookup"><span data-stu-id="b51ce-116">So, while we have some time, let's discuss what the command does.</span></span> 
    - <span data-ttu-id="b51ce-117">`docker run` kör ett kommando i en ny container.</span><span class="sxs-lookup"><span data-stu-id="b51ce-117">`docker run` runs a command in a new container.</span></span> <span data-ttu-id="b51ce-118">Den Docker-avbildning som används är pytorch/pytorch.</span><span class="sxs-lookup"><span data-stu-id="b51ce-118">The Docker image being used is pytorch/pytorch.</span></span> <span data-ttu-id="b51ce-119">Den skapar först ett skrivbart containerlager över den angivna avbildningen och startar det sedan med hjälp av det angivna kommandot.</span><span class="sxs-lookup"><span data-stu-id="b51ce-119">It first creates a writeable container layer over the specified image, and then starts it using the specified command.</span></span>
    - <span data-ttu-id="b51ce-120">`--rm` tar bort containern när den avslutas.</span><span class="sxs-lookup"><span data-stu-id="b51ce-120">`--rm` will remove the container once it exits.</span></span> <span data-ttu-id="b51ce-121">Om du vill behålla containern tar du bort det här argumentet.</span><span class="sxs-lookup"><span data-stu-id="b51ce-121">If you want to keep the container around, drop this argument.</span></span> 
    - <span data-ttu-id="b51ce-122">`--entrypoint 'bin/sh'` skriver över standardstartpunkten för den avbildning som ska vara Bash-gränssnittet</span><span class="sxs-lookup"><span data-stu-id="b51ce-122">`--entrypoint 'bin/sh'` overwrites the default entry point of the image to be the Bash shell</span></span>
    - <span data-ttu-id="b51ce-123">`-c` definierar vilket kommando som ska köras när containern startas.</span><span class="sxs-lookup"><span data-stu-id="b51ce-123">`-c` defines what command to run when the container starts.</span></span> <span data-ttu-id="b51ce-124">I det här fallet körs tre kommandon:</span><span class="sxs-lookup"><span data-stu-id="b51ce-124">In this case, it's running three commands:</span></span>
        - <span data-ttu-id="b51ce-125">Installerar Jupyter och matplotlib</span><span class="sxs-lookup"><span data-stu-id="b51ce-125">Installs Jupyter and matplotlib</span></span>
        - <span data-ttu-id="b51ce-126">Kopierar en notebook (cifar10_tutorial.ipynb) från pytorch.org till en fil i containern som heter `first_pytorch_classifier.ipynb`</span><span class="sxs-lookup"><span data-stu-id="b51ce-126">Copies a notebook (cifar10_tutorial.ipynb) from pytorch.org to a file in the container called `first_pytorch_classifier.ipynb`</span></span>
        - <span data-ttu-id="b51ce-127">Startar notebook-servern i containern på samma sätt som i den föregående övningen.</span><span class="sxs-lookup"><span data-stu-id="b51ce-127">Starts the notebook server in the container in the same way as the preceding exercise.</span></span>  <span data-ttu-id="b51ce-128">Det startas ingen webbläsare. Notebook tillåts kommas åt av roten och kan lyssna på alla portar.</span><span class="sxs-lookup"><span data-stu-id="b51ce-128">No browser is started, allow the notebook to be accessed by root and listen on all ports.</span></span> 
    
    <span data-ttu-id="b51ce-129">Notebook-servern lyssnar på alla portar för den containern.</span><span class="sxs-lookup"><span data-stu-id="b51ce-129">The notebook server listens on all ports for that container.</span></span> <span data-ttu-id="b51ce-130">Men hur kommer trafik utifrån containern in?</span><span class="sxs-lookup"><span data-stu-id="b51ce-130">But, how will traffic come from outside the container?</span></span> <span data-ttu-id="b51ce-131">Argumentet `-p 8888:8888` bilder porten `8888` i containern till TCP-port 8888 på värddatorn.</span><span class="sxs-lookup"><span data-stu-id="b51ce-131">The `-p 8888:8888` argument binds port `8888` of the container to TCP port 8888 on the host machine.</span></span> <span data-ttu-id="b51ce-132">Trafik till den virtuella datorn via port 8888 kommer därför att hämtas av containern.</span><span class="sxs-lookup"><span data-stu-id="b51ce-132">So, traffic coming to the VM over port 8888 will be picked up by the container.</span></span> 

## <a name="connect-to-the-jupyter-notebook-server-from-a-remote-browser"></a><span data-ttu-id="b51ce-133">Ansluta till Jupyter Notebook-servern från en fjärransluten webbläsare</span><span class="sxs-lookup"><span data-stu-id="b51ce-133">Connect to the Jupyter Notebook server from a remote browser</span></span> 

<span data-ttu-id="b51ce-134">När Jupyter Notebook körs i containern visas ett meddelande som liknar följande meddelande.</span><span class="sxs-lookup"><span data-stu-id="b51ce-134">Once the Jupyter notebook is running in the container, you'll  see a message similar to the following message.</span></span> 

> <span data-ttu-id="b51ce-135">*Kopiera och klistra in URL:en i webbläsaren när du ansluter för första gången för att logga in med en token: http://(5b8783e7911d or 127.0.0.1):8888/?token={sometoken}*</span><span class="sxs-lookup"><span data-stu-id="b51ce-135">*Copy/paste this URL into your browser when you connect for the first time, to login with a token: http://(5b8783e7911d or 127.0.0.1):8888/?token={sometoken}*</span></span>

1. <span data-ttu-id="b51ce-136">Ersätt **http://(5b8783e7911d or 127.0.0.1)** av URL:en med `<HOSTNAME>.<REGION>.cloudapp.azure.com` eller IP-adressen för den virtuella datorn och gå till adressen i en ny en flik i webbläsaren.</span><span class="sxs-lookup"><span data-stu-id="b51ce-136">Replace the **http://(5b8783e7911d or 127.0.0.1)** part of the URL with `<HOSTNAME>.<REGION>.cloudapp.azure.com` or the IP address of the VM and navigate to the address  in a new a tab of your browser.</span></span>

    ![<span data-ttu-id="b51ce-137">Skärmbild som visar instrumentpanelen för Jupyter Notebooks.</span><span class="sxs-lookup"><span data-stu-id="b51ce-137">Screenshot showing Jupyter Notebooks dashboard.</span></span> ](../media/notebook-in-docker.png)
    
    <span data-ttu-id="b51ce-138">Den här gången ser vi bara en enskild notebook.</span><span class="sxs-lookup"><span data-stu-id="b51ce-138">This time we only see a single notebook.</span></span> <span data-ttu-id="b51ce-139">Det beror på vi är i en container och endast kopierade den här notebook.</span><span class="sxs-lookup"><span data-stu-id="b51ce-139">That's because we're in a container and only copied down this notebook.</span></span> <span data-ttu-id="b51ce-140">I nästa övning experimenterar vi med den här notebook.</span><span class="sxs-lookup"><span data-stu-id="b51ce-140">In the next exercise, we'll experiment with this notebook.</span></span> 
    
    > [!TIP]
    > <span data-ttu-id="b51ce-141">Stäng inte av notebook-servern än.</span><span class="sxs-lookup"><span data-stu-id="b51ce-141">Don't shut down the notebook server just yet.</span></span> <span data-ttu-id="b51ce-142">Vi tittar på `first_pytorch_classifier.ipynb`-notebook i nästa övning.</span><span class="sxs-lookup"><span data-stu-id="b51ce-142">We'll look at the `first_pytorch_classifier.ipynb` notebook in the next exercise.</span></span>
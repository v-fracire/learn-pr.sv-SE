## <a name="introduction-to-jupyter-for-more-interactive-deep-learning"></a><span data-ttu-id="6ee5c-101">Introduktion till Jupyter för mer interaktiv djupinlärning</span><span class="sxs-lookup"><span data-stu-id="6ee5c-101">Introduction to Jupyter for more interactive deep learning</span></span> 

<span data-ttu-id="6ee5c-102">Jupyter Notebook är ett webbprogram med öppen källkod som gör det möjligt att skapa och dela dokument som innehåller kod, formler, visualiseringar och löpande text.</span><span class="sxs-lookup"><span data-stu-id="6ee5c-102">Jupyter Notebook is an open-source web application that allows you to create and share documents that contain live code, equations, visualizations, and narrative text.</span></span> <span data-ttu-id="6ee5c-103">Användningsområden är: datarensning och -transformering, numerisk simulering, statistisk modellering, datavisualisering, maskininlärning och mycket mer.</span><span class="sxs-lookup"><span data-stu-id="6ee5c-103">Uses include: data cleaning and transformation, numerical simulation, statistical modeling, data visualization, machine learning, and much more.</span></span>

## <a name="serving-jupyter-notebooks-with-nvidia-docker-on-an-azure-dsvm"></a><span data-ttu-id="6ee5c-104">Använda Jupyter Notebook med Nvidia Docker i en Azure DSVM</span><span class="sxs-lookup"><span data-stu-id="6ee5c-104">Serving Jupyter Notebooks with Nvidia Docker on an Azure DSVM</span></span>

### <a name="step-1-create-a-linux-dsvm"></a><span data-ttu-id="6ee5c-105">Steg 1 Skapa en Linux DSVM</span><span class="sxs-lookup"><span data-stu-id="6ee5c-105">Step 1 Create a Linux DSVM</span></span>

<span data-ttu-id="6ee5c-106">Använd call code (anropskod) från Azure CLI</span><span class="sxs-lookup"><span data-stu-id="6ee5c-106">Use call code from the Azure CLI</span></span>

```
code .
```

<span data-ttu-id="6ee5c-107">Fyll i följande distributionsschema och spara som parameter_file.json</span><span class="sxs-lookup"><span data-stu-id="6ee5c-107">Fill in the following deployment schema and save it as parameter_file.json</span></span>

``` 
{ 
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
     "adminUsername": { "value" : "YOURUSERNAME FOR NEW DSVM"},
     "adminPassword": { "value" : "PASSWORD FOR NEW DSVM"},
     "vmName": { "value" : "HOSTNAME OF DSVM"},
     "vmSize": { "value" : "VM SIZE For eg: Standard_DS2_v2"},
  }
}
```

<span data-ttu-id="6ee5c-108">En lista över tillgängliga storlekar för virtuella maskiner finns här [Ubuntu DSVM ARM template](https://azure.microsoft.com/global-infrastructure/services/?WT.mc_id=blog-learning-abornst).</span><span class="sxs-lookup"><span data-stu-id="6ee5c-108">A list of available vm sizes can be found here [Ubuntu DSVM ARM template](https://azure.microsoft.com/global-infrastructure/services/?WT.mc_id=blog-learning-abornst).</span></span>


### <a name="create-a-resource-group-for-your-dsvm-in-a-region-of-your-choice"></a><span data-ttu-id="6ee5c-109">Skapa en resursgrupp för din DSVM i en valfri region:</span><span class="sxs-lookup"><span data-stu-id="6ee5c-109">Create a resource group for your DSVM in a region of your choice:</span></span>
```
az group create --name [[NAME OF RESOURCE GROUP]] --location [[ Data center. For eg: "West US 2"]]
```

<span data-ttu-id="6ee5c-110">En lista över tillgängliga regioner hittar du här [Azure-regioner](https://github.com/Azure/DataScienceVM/blob/master/Scripts/CreateDSVM/Ubuntu/azuredeploy.json).</span><span class="sxs-lookup"><span data-stu-id="6ee5c-110">A list of available regions can be found here [Azure Regions](https://github.com/Azure/DataScienceVM/blob/master/Scripts/CreateDSVM/Ubuntu/azuredeploy.json).</span></span>

### <a name="deploy-your-dsvm-to-your-new-resource-group"></a><span data-ttu-id="6ee5c-111">Distribuera din DSVM till din nya resursgrupp</span><span class="sxs-lookup"><span data-stu-id="6ee5c-111">Deploy your DSVM to your new resource group</span></span>

```
az group deployment create --resource-group  [[NAME OF RESOURCE GROUP ABOVE]]  --template-uri https://raw.githubusercontent.com/Azure/DataScienceVM/master/Scripts/CreateDSVM/Ubuntu/azuredeploy.json --parameters parameter_file.json
```

## <a name="step-2-open-the-port-8888-22-on-the-dsvm"></a><span data-ttu-id="6ee5c-112">Steg 2 Öppna Port 8888, 22 på din DSVM</span><span class="sxs-lookup"><span data-stu-id="6ee5c-112">Step 2 Open the Port 8888, 22 on the DSVM</span></span> 

```
$ az vm open-port -g [[NAME OF RESOURCE GROUP]] -n [[HOSTNAME OF DSVM]] --port 22 --priority 900
$ az vm open-port -g [[NAME OF RESOURCE GROUP]] -n [[HOSTNAME OF DSVM]] --port 8888 --priority 901
```

<span data-ttu-id="6ee5c-113">Port 8888 är standardporten för Jupyter Notebook [Klicka här](https://docs.microsoft.com/azure/virtual-machines/windows/nsg-quickstart-portal?WT.mc_id=blog-medium-abornst) för detaljerade anvisningar om hur du öppnar en port</span><span class="sxs-lookup"><span data-stu-id="6ee5c-113">Port 8888 is the default port for Jupyter Notebooks For detailed steps on opening a port [click here](https://docs.microsoft.com/azure/virtual-machines/windows/nsg-quickstart-portal?WT.mc_id=blog-medium-abornst)</span></span>
 
## <a name="step-3-connect-to-the-dsvm-with-the-azure-shell"></a><span data-ttu-id="6ee5c-114">Steg 3 Anslut till din DSVM via Azure Shell</span><span class="sxs-lookup"><span data-stu-id="6ee5c-114">Step 3 Connect to the DSVM with the Azure Shell</span></span> 
 
``` 
ssh myuser@[[HOSTNAME OF DSVM]].westus2.cloudapp.azure.com 
``` 

## <a name="step-4-run-jupyter-in-docker-container--link-8888-port-to-the-vm-host"></a><span data-ttu-id="6ee5c-115">Steg 4 Kör Jupyter i Docker Container och länka port 8888 till VM-värden</span><span class="sxs-lookup"><span data-stu-id="6ee5c-115">Step 4 Run Jupyter in Docker Container & link 8888 port to the VM Host</span></span> 

<span data-ttu-id="6ee5c-116">Länka port 8888 mellan den virtuella datorn och dockerbehållaren, installera jupyter och hämta pytorch självstudier.</span><span class="sxs-lookup"><span data-stu-id="6ee5c-116">Link port 8888 between the VM and the docker container, install jupyter and pull pytorch tutorials.</span></span>  

```  
sudo docker run --rm -it --entrypoint '/bin/sh' -p 8888:8888 pytorch/pytorch -c \
  'conda install jupyter matplotlib -y &&\
  curl https://pytorch.org/tutorials/_downloads/cifar10_tutorial.ipynb > first_pytorch_classifier.ipynb &&\
  jupyter notebook --ip=0.0.0.0 --no-browser --allow-root'
``` 

## <a name="step-5-navigate-to-the-jupyter-notebook-in-the-browser"></a><span data-ttu-id="6ee5c-117">Steg 5 Navigera till Jupyter Notebook i webbläsaren</span><span class="sxs-lookup"><span data-stu-id="6ee5c-117">Step 5 Navigate to the Jupyter Notebook in the Browser</span></span> 

<span data-ttu-id="6ee5c-118">När Jupyter körs visas följande meddelande:</span><span class="sxs-lookup"><span data-stu-id="6ee5c-118">Once the Jupyter notebook is running you will see a message as follows :</span></span> 

> <span data-ttu-id="6ee5c-119">Kopiera och klistra in URL:en i webbläsaren när du ansluter för första gången, logga in med en token: http://(5b8783e7911d or 127.0.0.1):8888/?token={sometoken}</span><span class="sxs-lookup"><span data-stu-id="6ee5c-119">Copy/paste this URL into your browser when you connect for the first time, to login with a token: http://(5b8783e7911d or 127.0.0.1):8888/?token={sometoken}</span></span>

<span data-ttu-id="6ee5c-120">Ersätt **http://(5b8783e7911d or 127.0.0.1)** av URL:en med **[[VÄRDNAMN FÖR DSVM]].westus2.cloudapp.azure.com** och gå till adressen i en ny en flik i webbläsaren:</span><span class="sxs-lookup"><span data-stu-id="6ee5c-120">Replace the **http://(5b8783e7911d or 127.0.0.1)** part of the url with **[[HOSTNAME OF DSVM]].westus2.cloudapp.azure.com** and navigate to the address  in a new a tab in your browser:</span></span>
- <span data-ttu-id="6ee5c-121">[[VÄRDNAMN FÖR DSVM]].westus2.cloudapp.azure.com:8888/?token={sometoken}</span><span class="sxs-lookup"><span data-stu-id="6ee5c-121">[[HOSTNAME OF DSVM]].westus2.cloudapp.azure.com:8888/?token={sometoken}</span></span>

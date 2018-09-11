## <a name="introduction-to-jupyter-for-more-interactive-deep-learning"></a>Introduktion till Jupyter för mer interaktiv djupinlärning 

Jupyter Notebook är ett webbprogram med öppen källkod som gör det möjligt att skapa och dela dokument som innehåller kod, formler, visualiseringar och löpande text. Användningsområden är: datarensning och -transformering, numerisk simulering, statistisk modellering, datavisualisering, maskininlärning och mycket mer.

## <a name="serving-jupyter-notebooks-with-nvidia-docker-on-an-azure-dsvm"></a>Använda Jupyter Notebook med Nvidia Docker i en Azure DSVM

### <a name="step-1-create-a-linux-dsvm"></a>Steg 1 Skapa en Linux DSVM

Använd call code (anropskod) från Azure CLI

```
code .
```

Fyll i följande distributionsschema och spara som parameter_file.json

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

En lista över tillgängliga storlekar för virtuella maskiner finns här [Ubuntu DSVM ARM template](https://azure.microsoft.com/en-us/global-infrastructure/services/?WT.mc_id=blog-learning-abornst).


### <a name="create-a-resource-group-for-your-dsvm-in-a-region-of-your-choice"></a>Skapa en resursgrupp för din DSVM i en valfri region:
```
az group create --name [[NAME OF RESOURCE GROUP]] --location [[ Data center. For eg: "West US 2"]]
```

En lista över tillgängliga regioner hittar du här [Azure-regioner](https://github.com/Azure/DataScienceVM/blob/master/Scripts/CreateDSVM/Ubuntu/azuredeploy.json).

### <a name="deploy-your-dsvm-to-your-new-resource-group"></a>Distribuera din DSVM till din nya resursgrupp

```
az group deployment create --resource-group  [[NAME OF RESOURCE GROUP ABOVE]]  --template-uri https://raw.githubusercontent.com/Azure/DataScienceVM/master/Scripts/CreateDSVM/Ubuntu/azuredeploy.json --parameters parameter_file.json
```

## <a name="step-2-open-the-port-8888-22-on-the-dsvm"></a>Steg 2 Öppna Port 8888, 22 på din DSVM 

```
$ az vm open-port -g [[NAME OF RESOURCE GROUP]] -n [[HOSTNAME OF DSVM]] --port 22 --priority 900
$ az vm open-port -g [[NAME OF RESOURCE GROUP]] -n [[HOSTNAME OF DSVM]] --port 8888 --priority 901
```

Port 8888 är standardporten för Jupyter Notebook [Klicka här](https://docs.microsoft.com/en-us/azure/virtual-machines/windows/nsg-quickstart-portal?WT.mc_id=blog-medium-abornst) för detaljerade anvisningar om hur du öppnar en port
 
## <a name="step-3-connect-to-the-dsvm-with-the-azure-shell"></a>Steg 3 Anslut till din DSVM via Azure Shell 
 
``` 
ssh myuser@[[HOSTNAME OF DSVM]].westus2.cloudapp.azure.com 
``` 

## <a name="step-4-run-jupyter-in-docker-container--link-8888-port-to-the-vm-host"></a>Steg 4 Kör Jupyter i Docker Container och länka port 8888 till VM-värden 

Länka port 8888 mellan den virtuella datorn och dockerbehållaren, installera jupyter och hämta pytorch självstudier.  

```  
sudo docker run --rm -it --entrypoint '/bin/sh' -p 8888:8888 pytorch/pytorch -c \
  'conda install jupyter matplotlib -y &&\
  curl https://pytorch.org/tutorials/_downloads/cifar10_tutorial.ipynb > first_pytorch_classifier.ipynb &&\
  jupyter notebook --ip=0.0.0.0 --no-browser --allow-root'
``` 

## <a name="step-5-navigate-to-the-jupyter-notebook-in-the-browser"></a>Steg 5 Navigera till Jupyter Notebook i webbläsaren 

När Jupyter körs visas följande meddelande: 

> Kopiera och klistra in URL:en i webbläsaren när du ansluter för första gången, logga in med en token: http://(5b8783e7911d or 127.0.0.1):8888/?token={sometoken}

Ersätt **http://(5b8783e7911d or 127.0.0.1)** av URL:en med **[[VÄRDNAMN FÖR DSVM]].westus2.cloudapp.azure.com** och gå till adressen i en ny en flik i webbläsaren:
- [[VÄRDNAMN FÖR DSVM]].westus2.cloudapp.azure.com:8888/?token={sometoken}

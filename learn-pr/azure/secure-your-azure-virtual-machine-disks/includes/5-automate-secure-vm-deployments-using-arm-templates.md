Tänk dig att ditt företag distribuerar flera servrar som en del av övergången till molnet. Virtuella datordiskar måste krypteras under distributionen så att de inte är sårbara under något skede. Du vill automatisera den här processen och måste ändra Azure Resource Management-mallarna så att kryptering aktiveras automatiskt.

Här tar vi en titt på hur du använder en Azure Resource Manager-mall för att automatiskt aktivera kryptering för nya virtuella Windows-datorer.

## <a name="what-are-azure-resource-manager-templates"></a>Vad är Azure Resource Manager-mallar?

Resource Manager-mallar är JSON-filer som används till att definiera en uppsättning resurser som ska distribueras till Azure. Du kan skriva dem från början, och för vissa Azure-resurser som virtuella datorer kan du generera dem genom att använda Azure Portal. Du måste fylla i all nödvändig information för en manuell distribution av virtuella datorer, men i stället för att distribuera den virtuella datorn till Azure sparar du mallen. Du kan sedan _återanvända_ mallen för att konfigurera den specifika VM-konfigurationen.

Det finns [exempelmallar i docs](https://azure.microsoft.com/resources/templates) för att automatisera alla typer av administrativa uppgifter. Vi kunde faktiskt ha använt en av dessa mallar till att kryptera vår virtuella dator som vi precis gjorde manuellt!

![Skärmbild som visar Azure-mallarna](../media/5-browse-templates.png)

## <a name="using-github-templates"></a>Använda GitHub-mallar

Den faktiska mallkällan lagras i GitHub. Du kan bläddra till en mall i GitHub och distribuera direkt till Azure från sidan.

![Skärmbild som visar en GitHub-mall med knappen för att distribuera till Azure markerad](../media/5-deploy-from-github.png)

När mallen distribueras visas en lista med obligatoriska inmatningsfält.

![Skärmbild som visar mallen i Azure-portalen](../media/5-fill-in-template.png)

Du kan sedan köra mallen för att skapa, ändra eller ta bort resurser.

### <a name="running-templates-in-the-azure-portal"></a>Köra mallar i Azure-portalen

Om du redan vet vilken mall du vill använda, eller om du har sparat mallar i ditt Azure-konto, kan du använda resursen **Skapa en resurs** > **Malldistribution** för att hitta och köra definierade mallar i portalen. Du kan söka igenom mallar efter namn, redigera en mall för att ändra parametrar eller beteende och köra mallen direkt från det grafiska användargränssnittet.

### <a name="running-templates-from-the-command-line"></a>Köra mallar från kommandoraden

Om du får en webbadress till en mall kan du köra den med Azure PowerShell. Vi kan till exempel köra diskkrypteringsmallen med följande PowerShell-kommando:

```powershell
New-AzureRmResourceGroupDeployment `
    -Name encrypt-disk `
    -ResourceGroupName <resource-group-name> `
    -TemplateUri https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/201-encrypt-running-windows-vm-without-aad/azuredeploy.json
```

Om du hellre vill använda Azure CLI kan du göra det med kommandot `group deployment create`.

```azurecli
azure config mode arm
azure group deployment create <my-resource-group> <my-deployment-name> \ 
    --template-uri https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/201-encrypt-running-windows-vm-without-aad/azuredeploy.json
```


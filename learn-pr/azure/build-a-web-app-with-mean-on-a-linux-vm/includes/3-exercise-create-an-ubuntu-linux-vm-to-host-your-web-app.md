MEAN-komponentstacken kräver en server. Det kan vara en Linux-dator eller en virtuell dator som du kör i ditt eget serverrum, eller så kan du konfigurera en molnbaserad virtuell dator. I den här modulen konfigurerar du stacken för körning på en virtuell Ubuntu Linux-dator som körs i Azure.

I den här enheten skapar du en ny Ubuntu Linux virtuell dator på Azure. Du kan också installera komponenterna i MEAN-stacken på en befintlig virtuell dator eller på en fysisk värddator. Genom att skapa en ny virtuell dator för övningen kan du koppla samman alla komponenter i en Azure-resursgrupp för enklare hantering och borttagning när du har slutfört övningarna.

## <a name="provision-an-ubuntu-linux-vm"></a>Etablera en virtuell Ubuntu Linux-dator

[!include[](../../../includes/azure-sandbox-activate.md)]

<!--
TODO: Omitting for sandbox. Keeping here for possible later inclusion.

1. In Cloud Shell, execute the command to create an Azure resource group, which will include our VM. Substitute your own resource group name for `<resource-group-name>` and your desired Azure location for `<resource-group-location>` (`westus`, for example).


    ```azurecli
    az group create --name <resource-group-name> --location <resource-group-location>
    ```

    Remember your resource group name, as we will use it in other commands.
-->

1. Kör följande kommando i Cloud Shell för att skapa en ny virtuell Ubuntu Linux-dator. Ersätt önskade administratörens användarnamn och lösenord för `<vm-admin-username>` och `<vm-admin-password>`.

    ```azurecli
    az vm create \
        --resource-group <rgn>[Sandbox resource group name]</rgn> \
        --name MeanDemo \
        --image UbuntuLTS \
        --admin-username <vm-admin-username> \
        --admin-password <vm-admin-password> \
        --generate-ssh-keys
    ```

    Skriv ned administratörsanvändarnamnet och administratörslösenordet så att du kan ansluta till den här virtuella datorn senare.

    Det här kommandot tar ungefär två minuter att köra. När kommandot har slutförts bör utdata se ut ungefär så här:

    ```json
    {
        "fqdns": "",
        "id": "...",
        "location": "<resource group location>",
        "macAddress": "00-0D-3A-3A-54-EC",
        "powerState": "VM running",
        "privateIpAddress": "10.0.0.4",
        "publicIpAddress": "<the public IP address of the newly created machine>",
        "resourceGroup": "<rgn>[Sandbox resource group name]</rgn>",
        "zones": ""
    }
    ```

    Du bör även spara den offentliga IP-adressen för den nyligen skapade virtuella datorn för att kunna ansluta till den virtuella datorn.

1. Prova att ansluta till den nya virtuella datorn.

    Kör följande kommando från Cloud Shell. Ange administratörsanvändarnamnet och den virtuella datorns offentliga IP-adress i platshållarna `<vm-admin-username>` och `<vm-public-ip>`.

    ```bash
    ssh <vm-admin-username>@<vm-public-ip>
    ```

    Första gången du ansluter till datorn får du ange om du litar på fjärrdatorn. Om du svarar `yes` sparas fingeravtrycket för datorns ECDSA-nyckel lokalt så att efterföljande anslutningar automatiskt betraktas som betrodda.

    Om allt ser bra ut skriver du `exit` för att stänga SSH-sessionen.

1. Öppna port 80 på den virtuella datorn så att inkommande HTTP-trafik till det nya webbprogrammet som du vill skapa.

    ``` bash
    az vm open-port --port 80 --resource-group <rgn>[Sandbox resource group name]</rgn> --name MeanDemo
    ```

    Det här kommandot öppnar HTTP-porten på den virtuella datorn som fick namnet ”MeanDemo” när den skapades.

## <a name="summary"></a>Sammanfattning

Nu när den nya virtuella Ubuntu Linux-datorn är klar kan vi ansluta till den för att börja installera de olika komponenterna i MEAN-stacken.
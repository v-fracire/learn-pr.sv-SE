MEAN-komponentstacken kräver en server. Det kan vara en Linux-dator eller en virtuell dator som du kör i ditt eget serverrum, eller som du konfigurerar på en molnbaserad virtuell dator. I den här modulen konfigurerar vi stacken som ska köras på en virtuell Ubuntu Linux-dator i Azure.

I den här kursdelen skapar du en ny virtuell Ubuntu Linux-dator i Azure. Du kan också installera komponenterna i MEAN-stacken på en befintlig virtuell dator eller på en fysisk värddator. Genom att skapa en ny virtuell dator i den här övningen kan du koppla samman alla komponenter i en Azure-resursgrupp för enklare hantering och borttagning när du har slutfört övningarna.

Vi använder Cloud Shell-kommandoraden som är integrerad i Azure Portal för att skapa den virtuella Linux-datorn.

## <a name="provision-an-ubuntu-linux-vm"></a>Etablera en virtuell Ubuntu Linux-dator

1. Gå till [Azure Portal](https://portal.azure.com?azure-portal=true).
1. Öppna Cloud Shell från vinkelparentesen (>_) i verktygsfältet i Azure Portal.
1. I Cloud Shell kör du kommandot för att skapa den Azure-resursgrupp som ska innehålla vår virtuella dator. Byt ut `<resource-group-name>` mot namnet på din egen resursgrupp och `<resource-group-location>` mot önskad Azure-region (till exempel `westus`).


    ```bash
    az group create --name <resource-group-name> --location <resource-group-location>
    ```

    Skriv ned namnet på resursgruppen eftersom vi ska använda det i andra kommandon.

1. Kör följande kommando i Cloud Shell för att skapa en ny virtuell Ubuntu Linux-dator. Ange namnet på din egen resursgrupp i `<resource-group-name>`, samt ett administratörsanvändarnamn och administratörslösenord i `<vm-admin-username>` och `<vm-admin-password>`.

    ```bash
    az vm create \
        --resource-group <resource-group-name> \
        --name MeanDemo \
        --image UbuntuLTS \
        --admin-username <vm-admin-username> \
        --admin-password <vm-admin-password> \
        --generate-ssh-keys
    ```

    Skriv ned administratörsanvändarnamnet och administratörslösenordet så att du kan ansluta till den här virtuella datorn senare.

    Det här kommandot tar ungefär två minuter att köra. När kommandot har slutförts returneras utdata liknande de nedan.

    ```json
    {
        "fqdns": "",
        "id": "...",
        "location": "<location you chose for the resource group>",
        "macAddress": "00-0D-3A-3A-54-EC",
        "powerState": "VM running",
        "privateIpAddress": "10.0.0.4",
        "publicIpAddress": "<the public IP address of the newly created machine>",
        "resourceGroup": "<name you chose for thr resource group>",
        "zones": ""
    }
    ```

    Du bör även spara den offentliga IP-adressen för den nyligen skapade virtuella datorn för att kunna ansluta till den virtuella datorn.

1. Prova att ansluta till den nya virtuella datorn.

    Öppna en kommandotolk eller ett terminalfönster och kör följande kommando. Ange administratörsanvändarnamnet och den virtuella datorns offentliga IP-adress i platshållarna `<vm-admin-username>` och `<vm-public-ip>`.

    ```bash
    ssh <vm-admin-username>@<vm-public-ip>
    ```

    Första gången du ansluter till datorn uppmanas du ange om du litar på fjärrdatorn. Om du svarar `yes` sparas fingeravtrycket för datorns ECDSA-nyckel lokalt, för att efterföljande anslutningar ska anses vara betrodda.

    Om allt ser bra ut skriver du `exit` för att stänga SSH-sessionen.

1. Öppna port 80 för att tillåta inkommande HTTP-trafik till det nya webbprogrammet du kommer att skapa.

    Gå tillbaka till Cloud Shell i Azure Portal. Kör följande kommando och byt ut `<resource-group-name>` mot det ursprungliga resursgruppnamnet.

    ``` bash
    az vm open-port --port 80 --resource-group <resource-group-name> --name MeanDemo
    ```

    Det här kommandot öppnar HTTP-porten på den virtuella datorn, som fick namnet ”MeanDemo” när den skapades.

## <a name="summary"></a>Sammanfattning

Nu när den nya virtuella Ubuntu Linux-datorn är klar kan vi ansluta till den för att börja installera de olika komponenterna i MEAN-stacken.
MEAN-komponentstacken kräver en server. Det kan vara en Linux-dator eller en virtuell dator som du kör i ditt eget serverrum, eller som du konfigurerar på en molnbaserad virtuell dator. I den här modulen ska vi konfigurera stacken att köra på en virtuell Ubuntu Linux-dator som körs i Azure.

I den här övningen ska du skapa en ny virtuell Ubuntu Linux-dator i Azure. Du kan också installera komponenterna i MEAN-stacken på en befintlig virtuell dator eller på en fysisk värddator. Genom att skapa en ny virtuell dator i den här övningen kan du koppla samman alla komponenter i en Azure-resursgrupp för enklare hantering och borttagning när du har slutfört övningarna.

Vi ska använda Cloud Shell-kommandoradsverktyget som är integrerat med Azure Portal för att skapa den virtuella Linux-datorn.

## <a name="provision-an-ubuntu-linux-vm"></a>Etablera en virtuell Ubuntu Linux-dator

1. Gå till [Azure Portal](https://portal.azure.com?azure-portal=true).
1. Öppna Cloud Shell från `>_`-ikonen i verktygsfältet på Azure Portal.
1. I Cloud Shell kör du kommandot för att skapa en Azure-resursgrupp, som ska innehålla vår virtuella dator. Ange ett resursgruppnamn i `<resource-group-name>` och en valfri Azure-plats i `<resource-group-location>` (till exempel `westus`).

    ```bash
    az group create --name <resource-group-name> --location <resource-group-location>
    ```

    Skriv ned namnet på resursgruppen eftersom vi ska använda det i andra kommandon.

1. Kör följande kommando i Cloud Shell för att skapa en ny virtuell Ubuntu Linux-dator. Ange namnet på resursgruppen i `<resource-group-name>` och ett administratörsanvändarnamn och administratörslösenord i `<vm-admin-username>` och `<vm-admin-password>`.

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

    Det här kommandot tar cirka 2 minuter. När kommandot har slutförts returneras utdata liknande de nedan.

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

    Första gången du ansluter till datorn uppmanas du att ange om du litar på fjärrdatorn. Om du svarar `yes` sparas fingeravtrycket för datorns ECDSA-nyckel lokalt så att efterföljande anslutningar automatiskt betraktas som betrodda.

    Om allt ser bra ut skriver du `exit` för att stänga SSH-sessionen.

1. Öppna port 80 för att tillåta inkommande HTTP-trafik till det nya webbprogrammet som du ska skapa.

    Gå tillbaka till Cloud Shell på Azure Portal och kör följande kommando med det ursprungliga resursgruppnamnet för `<resource-group-name>`.

    ``` bash
    az vm open-port --port 80 --resource-group <resource-group-name> --name MeanDemo
    ```

    När du kör kommandot öppnas HTTP-porten på den virtuella datorn med namnet ”MeanDemo”.

## <a name="summary"></a>Sammanfattning

Nu när den nya virtuella Ubuntu Linux-datorn är klar kan vi ansluta till den för att börja installera de olika komponenterna i MEAN-stacken.

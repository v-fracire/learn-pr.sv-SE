MEAN-komponentstacken kräver en server. Det kan vara en Linux-dator eller en virtuell dator som du kör i ditt eget serverrum, eller som du konfigurerar på en molnbaserad virtuell dator. I den här modulen konfigurerar vi stacken som ska köras på en virtuell Ubuntu Linux-dator i Azure.

I den här kursdelen skapar du en ny virtuell Ubuntu Linux-dator i Azure. Du kan också installera komponenterna i MEAN-stacken på en befintlig virtuell dator eller på en fysisk värddator. Genom att skapa en ny virtuell dator i den här övningen kan du koppla samman alla komponenter i en Azure-resursgrupp för enklare hantering och rensning när du har slutfört övningarna.

## <a name="provision-an-ubuntu-linux-vm"></a>Etablera en virtuell Ubuntu Linux-dator

[!include[](../../../includes/azure-sandbox-activate.md)]

### <a name="creating-a-resource-group"></a>Skapa resursgruppen

Normalt sett är det första du gör när du skapar en ny uppsättning resurser att skapa en _resursgrupp_ som alla ingår i. Det här är ett onödigt steg i sandbox-miljön i Azure, men när du arbetar med din egen prenumeration ska du använda följande kommando för att skapa en resursgrupp på en plats nära dig.

```azurecli
az group create --name <resource-group-name> --location <resource-group-location>
```

> [!IMPORTANT]
> Du behöver inte skapa en resursgrupp med sandbox-miljön i Azure. Använd i stället resursgruppen **<rgn>[resursgrupp för sandbox-miljö]</rgn>** som har skapats i förväg.

1. Skapa en ny virtuell Ubuntu Linux-dator genom att ange följande kommando till höger i Cloud Shell. Ange ett administratörsanvändarnamn och administratörslösenord för `<vm-admin-username>` och `<vm-admin-password>`.

    ```azurecli
    az vm create \
        --resource-group <rgn>[sandbox resource group name]</rgn> \
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
        "location": "<resource group location>",
        "macAddress": "00-0D-3A-3A-54-EC",
        "powerState": "VM running",
        "privateIpAddress": "10.0.0.4",
        "publicIpAddress": "<the public IP address of the newly created machine>",
        "resourceGroup": "<rgn>[sandbox resource group name]</rgn>",
        "zones": ""
    }
    ```

    Du bör även spara den offentliga IP-adressen för den nyligen skapade virtuella datorn för att kunna ansluta till den virtuella datorn.

1. Prova att ansluta till den nya virtuella datorn.

    Från Cloud Shell kör du följande kommandon. Ange administratörsanvändarnamnet och den virtuella datorns offentliga IP-adress i platshållarna `<vm-admin-username>` och `<vm-public-ip>`.

    ```bash
    ssh <vm-admin-username>@<vm-public-ip>
    ```

    Första gången du ansluter till datorn uppmanas du ange om du litar på fjärrdatorn. Om du svarar `yes` sparas fingeravtrycket för datorns ECDSA-nyckel lokalt, för att efterföljande anslutningar ska anses vara betrodda. Du uppmanas sedan att lösenordet, vilket du ser varje gång du ansluter.

    Om allt ser bra ut skriver du `exit` för att stänga SSH-sessionen.

1. Öppna port 80 på den virtuella datorn för att tillåta inkommande HTTP-trafik till det nya webbprogrammet som du kommer att skapa.

    ```azurecli
    az vm open-port --port 80 --resource-group <rgn>[sandbox resource group name]</rgn> --name MeanDemo
    ```

    Det här kommandot öppnar HTTP-porten på den virtuella datorn, som fick namnet ”MeanDemo” när den skapades.

## <a name="summary"></a>Sammanfattning

Nu när den nya virtuella Ubuntu Linux-datorn är klar kan vi ansluta till den för att börja installera de olika komponenterna i MEAN-stacken.
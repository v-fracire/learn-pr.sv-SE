I den här kursdelen installerar du Node.js – **N** i förkortningen MEAN – på en Azure-värdbaserad virtuell Ubuntu Linux-dator. Node.js kommer att fungera som CLR-stöd för hanteringen av vår HTTP-trafik och som värd för webbappen.

## <a name="install-nodejs"></a>Installera Node.js

1. **Om du fortfarande inte är ansluten med SSH till den virtuella datorn från den föregående övningen**, ska du ansluta nu.

    ```bash
    ssh <vm-admin-username>@<vm-public-ip>
    ```

1. Registrera Node.js-paketlagringsplatsen så att **apt-get** kan hitta rätt paket att installera på den virtuella datorn.

    ```bash
    curl -sL https://deb.nodesource.com/setup_8.x | sudo -E bash -
    ```

1. Installera Node.js-paketet på Linux-datorn.

    ```bash
    sudo apt-get install -y Node.js
    ```

1. Verifiera att Node.js-installationen lyckades genom att köra följande enkla Node.js-kommando.

    ```bash
    node -v
    ```

    Utdata bör se ut ungefär som `v8.11.4`, där versionen speglar den senast tillgängliga Node.js-versionen när du installerar paketet.

## <a name="summary"></a>Sammanfattning

Med Node.js installerat på den virtuella datorn kan vi börja skapa ett webbprogram som att det blir ansvarigt för att köra.
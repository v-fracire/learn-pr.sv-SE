I den här övningen installerar du Node.js – **N** i förkortningen MEAN – på en Azure-värdbaserad virtuell Ubuntu Linux-dator. Node.js kommer att fungera som runtime för att hantera vår HTTP-trafik och som värd för webbappen.

## <a name="connect-to-the-vm"></a>Ansluta till den virtuella datorn

För att kunna installera Node.js måste du ansluta till den virtuella datorn med hjälp av **ssh**. Om du inte fortfarande är ansluten till den virtuella datorn kör du följande kommando. Ange administratörsanvändarnamnet och den virtuella datorns offentliga IP-adress i platshållarna `<vm-admin-username>` och `<vm-public-ip>`.

```bash
ssh <vm-admin-username>@<vm-public-ip>
```

## <a name="install-nodejs"></a>Installera Node.js

> [!Important]
> Ubuntu tillhandahåller ett inofficiellt paket som heter **Node.js-legacy**, men det här paketet underhålls inte av Node.js och är inaktuellt.

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

    Utdata bör se ut ungefär som `v8.11.4`, där versionen speglar den senast tillgängliga Node.js-versionen i när du installerar paketet.

## <a name="summary"></a>Sammanfattning

Med Node.js installerat på den virtuella datorn kan vi börja skapa ett webbprogram som att det blir ansvarigt för att köra.

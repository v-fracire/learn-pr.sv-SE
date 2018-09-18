I den här kursdelen installerar du MongoDB på din virtuella Ubuntu Linux-dator, som ska fungera som ett datalager för ditt kommande exempel på ett webbprogram.

## <a name="connect-to-the-vm"></a>Ansluta till den virtuella datorn

För att kunna installera MongoDB måste du ansluta till den virtuella datorn med hjälp av **ssh**. Ange administratörsanvändarnamnet och den virtuella datorns offentliga IP-adress i platshållarna `<vm-admin-username>` och `<vm-public-ip>`.

```bash
ssh <vm-admin-username>@<vm-public-ip>
```

## <a name="install-mongodb"></a>Installera MongoDB

> [!Important]
> Ubuntu tillhandahåller ett inofficiell paket som heter **mongodb**. Det här paketet underhålls inte av MongoDB Inc.

1. Importera krypteringsnyckeln för MongoDB-lagringsplatsen. Då kan pakethanteraren verifiera att de mongodb-paketen som du installerar kommer från MongoDB Inc.

    ```bash
    sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv 9DA31620334BD75D9DCB49F368818C72E52529D4
    ```

    Kommandot **sudo** innebär att vi vill köra det angivna kommandot med administratörsprivilegier.

1. Registrera MongoDB Ubuntu-lagringsplatsen så att pakethanteraren kan hitta mongodb-paketen.

    > [!NOTE]
    > Det här kommandot är olika för olika versioner av Ubuntu. För att ta reda på vilken version av Ubuntu du använder så kör du: `uname -v`.
    > Utdatan ser ut ungefär så här: `#21~16.04.1-Ubuntu SMP Fri Aug 10 12:36:09 UTC 2018`.
    >
    > Den visar att vi kör Ubuntu version 16.04.1.
    > Se dokumentationen om att [Installera MongoDB Community Edition på Ubuntu](https://docs.mongodb.com/manual/tutorial/install-mongodb-on-ubuntu/) för att se det exakta kommandot för din version.

    På Ubuntu 16.04 kör vi det här kommandot:

    ```bash
    echo "deb [ arch=amd64,arm64 ] https://repo.mongodb.org/apt/ubuntu xenial/mongodb-org/4.0 multiverse" | sudo tee /etc/apt/sources.list.d/mongodb-org-4.0.list
    ```

1. Läs in paketdatabasen igen så att vi har den senaste paketinformationen.

    ```bash
    sudo apt-get update
    ```

1. Installera MongoDB-paketet till vår virtuella dator.

    ```bash
    sudo apt-get install -y mongodb-org
    ```

1. Starta MongoDB-tjänsten så att du kan ansluta till den senare.

    ```bash
    sudo service mongod start
    ```

## <a name="summary"></a>Sammanfattning

Nu har vi MongoDB installerat på vår virtuella Ubuntu Linux-dator. MongoDB fungerar som reservdatalager för den information du sparar och hämtar i ditt webbprogram.
Vår virtuella Linux-dator har distribuerats och körs men den har inte konfigurerats att utföra arbete. Nu ska vi ansluta till den med SSH och konfigurera Apache, så att vi har en aktiv server.

## <a name="connect-to-the-vm-with-ssh"></a>Ansluta till den virtuella datorn med SSH

Om du vill ansluta till en virtuell Azure-dator med en SSH-klient behöver du:

- SSH-klientprogramvaran (finns på de flesta moderna operativsystem)
- Den offentliga IP-adressen för den virtuella datorn (eller privat om den virtuella datorn är konfigurerad för att ansluta till nätverket)

### <a name="get-the-public-ip-address"></a>Hämta den offentliga IP-adressen

1. Se till att [översiktspanelen](https://portal.azure.com/learn.docs.microsoft.com?azure-portal=true) för den virtuella dator du skapade tidigare är öppen i **Azure Portal**. Du hittar den virtuella datorn under **Alla resurser** om du behöver öppna den. Översiktspanelen har mycket information om den virtuella datorn.

    - Du kan se om den virtuella datorn körs
    - Stoppa eller starta om den
    - Hämta den offentliga IP-adressen för att ansluta till den virtuella datorn
    - Visa aktiviteten för CPU, disk och nätverk

1. Klicka på knappen **Anslut** högst upp i fönstret.

1. På bladet **Anslut till virtuell dator** noterar du inställningarna för **IP-adress** och **portnummer**. På fliken **SSH** fliken finns också kommandot som du behöver köra lokalt för att ansluta till den virtuella datorn. Kopiera detta till Urklipp.

## <a name="connect-with-ssh"></a>Ansluta med SSH

1. Klistra in den kommandorad som du fick från SSH-fliken i Azure Cloud Shell. Det bör se ut ungefär så här. Men den har en annan IP-adress (och kanske ett annat användarnamn om du inte använde **jim**!):

    ```bash
    ssh jim@137.117.101.249
    ```

1. Det här kommandot öppnar en Secure Shell-anslutning och placerar dig i en traditionell kommandotolk för Linux.

1. Prova att köra några Linux-kommandon
    - `ls -la /` för att visa diskens rot
    - `ps -l` för att visa alla processer som körs
    - `dmesg` för att lista alla kernelmeddelanden
    - `lsblk` för att lista alla blockenheter – här ser du dina enheter

Mer intressant att observera i listan över enheter är vad som _saknas_. Observera att vår **Data**-enhet (`sdc`) finns men har inte monterats i filsystemet. Azure har lagt till en VHD men har inte initierat den.

## <a name="initialize-data-disks"></a>Initiera datadiskar

Alla ytterligare enheter som du skapar från början måste initieras och formateras. Processen för att gör detta är identisk med en fysisk disk:

1. Först identifierar du disken. Vi gjorde det ovan. Du kan också använda `dmesg | grep SCSI` som listar alla meddelanden från kärnan för SCSI-enheter.

1. När du känner till enheten (`sdc`) måste du initiera, vilket du kan göra med `fdisk`. Du måste köra kommandot med `sudo` och ange den disk du vill partitionera. Vi kan använda följande kommando för att skapa en ny primär partition:

    ```bash
    (echo n; echo p; echo 1; echo ; echo ; echo w) | sudo fdisk /dev/sdc
    ```

1. Nu måste vi skriva ett filsystem till partitionen med kommandot `mkfs`.

    ```bash
    sudo mkfs -t ext4 /dev/sdc1
    ```

1. Slutligen måste vi montera enheten till filsystemet. Anta att vi har en `data`-mapp. Nu ska vi skapa monteringspunktmappen och montera enheten.

    ```bash
    sudo mkdir /data & sudo mount /dev/sdc1 /data
    ```

    > [!TIP]
    > Vi har initierats disken och monterat den. Om du vill veta mer om den här processen kan du gå igenom modulen **Lägg till diskar i Azure Virtual Machines**. Den här uppgiften gås igenom mer ingående där.

## <a name="install-software-onto-the-vm"></a>Installera programvara på den virtuella datorn

Som du ser kan du med SSH arbeta med den virtuella Linux-datorn precis som en lokal dator. Du kan administrera den här virtuella datorn precis som andra Linux-datorer: installera programvara, konfigurera roller, justera funktioner och andra dagliga uppgifter. Nu ska vi fokusera ett tag på att installera programvara.

Det finns flera alternativ för att installera programvara på den virtuella datorn. Först kan du, som nämnt, använda `scp` till att kopiera lokala filer från din dator till den virtuella datorn. Då kan du kopiera över de data eller anpassade program som du vill köra.

Du kan även installera programvara via Secure Shell. Azure-datorer är som standard anslutna till Internet. Du kan använda standardkommandon för att installera populära programvarupaket direkt från standardlagringsplatser. Vi använder den här metoden för att installera Apache.

### <a name="install-the-apache-web-server"></a>Installera Apache-webbservern

Apache är tillgänglig i Ubuntus standardlagringsplatser för programvara, så vi installerar det med hjälp av konventionella pakethanteringsverktyg:

1. Starta genom att uppdatera det lokala paketindexet för att spegla de senaste överordnade ändringarna:

    ```bash
    sudo apt-get update
    ```

1. Installera sedan Apache:

    ```bash
    sudo apt-get install apache2 -y
    ```

1. Det bör starta automatiskt och statusen kan kontrolleras med `systemctl`:

    ```bash
    sudo systemctl status apache2 --no-pager
    ```

    Något sådant här bör returneras:

    ```output
    apache2.service - The Apache HTTP Server
       Loaded: loaded (/lib/systemd/system/apache2.service; enabled; vendor preset: enabled)
      Drop-In: /lib/systemd/system/apache2.service.d
               └─apache2-systemd.conf
       Active: active (running) since Mon 2018-09-03 21:00:03 UTC; 1min 34s ago
     Main PID: 11156 (apache2)
        Tasks: 55 (limit: 4915)
       CGroup: /system.slice/apache2.service
               ├─11156 /usr/sbin/apache2 -k start
               ├─11158 /usr/sbin/apache2 -k start
               └─11159 /usr/sbin/apache2 -k start

    test-web-eus-vm1 systemd[1]: Starting The Apache HTTP Server...
    test-web-eus-vm1 apachectl[11129]: AH00558: apache2: Could not reliably determine the server's fully qua
    test-web-eus-vm1 systemd[1]: Started The Apache HTTP Server.
    ```
    > [!NOTE]
    > Det är enkelt att köra den här typen av kommandon, men det är en manuell process. Om vissa program alltid behöver installeras kan du automatisera processen med hjälp av skript.

1. Slutligen kan vi försöka hämta standardsidan via den offentliga IP-adressen. Men även om servern körs på den virtuella datorn, får du inte en giltig anslutning eller ett svar. Vet du varför?

Vi måste utföra ytterligare ett steg för att kunna interagera med webbservern. Våra virtuella nätverk blockerar inkommande begäranden – det är standardbeteendet. Vi kan ändra det i konfigurationen. Låt oss titta på det härnäst.
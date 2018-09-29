Innan vi går igenom några av de vanliga sätten att köra, visa och ta bort containrar ska vi titta på en grundläggande Nginx-konfiguration som körs på en Docker-container som finns på en virtuell Ubuntu-dator (VM).

I den här delen får du lära dig att:

1. Skapa en virtuell Linux-dator som har konfigurerats för att installera Docker när den virtuella datorn först startas.
1. Anslut till den virtuella datorn och starta en Docker-container som kör Nginx.
1. Använd portbindningen för att göra din webbserver åtkomlig utanför den virtuella datorn.

## <a name="what-are-containers"></a>Vad är containrar?

Containrar är en virtualiseringsmiljö, men till skillnad från virtuella datorer har de inget operativsystem. I stället refererar de till operativsystemet i den värdmiljö som kör containern. Till exempel om fem containrar körs på en server med en specifik Linux-kärna så körs alla fem containrarna på samma kärna.

Med containrar kan du paketera ditt program och allt som det kräver för att köras in på något som kallas en _containeravbildning_. Containrar är isolerade vilket innebär att de inte stör andra containrar som körs på samma system. Containeravbildningar är också portabla. Du kan ladda upp dina avbildningar till ett containerregister, till exempel Docker Hub eller Azure Container Registry. Du kan då köra dina containrar i molnet och de fungera på exakt samma sätt som i din utvecklingsmiljö.

> [!VIDEO https://www.microsoft.com/videoplayer/embed/RE2yMhY]

Containrar möjliggör snabb iteration. Eftersom containrar inte behöver inkludera ett operativsystem är de normalt mycket mindre än fullständiga virtuella datorer. Det gör att du kan starta och stoppa containrar snabbt, ofta inom några sekunder.

## <a name="understand-the-setup"></a>Förstå systemet

I utbildningssyfte tillhandahåller vi en sandbox-miljö för dig att arbeta i. Den här miljön innehåller Azure Cloud Shell, en webbläsarbaserad kommandoradsmiljö för att hantera och utveckla Azure-resurser.

Från Cloud Shell skapar du en virtuell Linux-dator som innehåller Docker. Du ansluter till din virtuella Linux-dator via SSH så att du kan skapa och använda Docker-containrar interaktivt.

Betrakta den virtuella Linux-datorn som din utvecklingsmiljö och en plats att lära dig om containrar. I praktiken skulle du installera Docker på den dator som du använder för att utveckla dina appar. Docker körs på Windows, macOS och Linux.

Vi tillhandahåller resurser där du kan lära dig mer om att köra Docker i din lokala utvecklingsmiljö i slutet av den här modulen.

## <a name="what-is-nginx"></a>Vad är Nginx?

Nginx (uttalas ”engine-x”) är en populär webbserver med öppen källkod som körs på Unix, Linux, macOS och Windows. Att konfigurera en webbserver är ett bra sätt att se containrar i arbete. Här använder du Nginx till att leverera en grundläggande webbsida.

## <a name="what-is-cloud-init"></a>Vad är cloud-init?

Med Cloud-init kan du anpassa en virtuell Linux-dator när den startas för första gången. Du kan använda cloud-init till att installera paket och skriva filer eller för att konfigurera användare och säkerhet. Här ska du använda ett cloud-init-skript för att installera Docker på den virtuella datorn.

## <a name="what-is-port-binding"></a>Vad är portbindning?

Med portbindningen kan du vidarebefordra nätverkstrafik till en container från dess värd.

En Docker-container körs i ett lokalt nätverk på containerns värdsystem. I det här fallet är containerns nätverk på din virtuella Linux-dator.

Du vet att webbförfrågningar normalt körs över port 80 (HTTP). Containern är inte tillgänglig för omvärlden eftersom den körs i ett lokalt nätverk i den virtuella datorn.

Med Docker kan du publicera eller exponera en port så att den blir tillgänglig utanför den virtuella datorn. Här ska du konfigurerar din container så att trafik till port 8080 på den virtuella datorn kommer att vidarebefordras till port 80 på containern.

## <a name="create-a-virtual-machine-to-host-docker"></a>Skapa en virtuell dator som värd för Docker

[!include[](../../../includes/azure-sandbox-activate.md)]

### <a name="create-the-cloud-init-script"></a>Skapa cloud-init-skriptet

1. Navigera till mappen **clouddrive**.

    ```azurecli
    cd clouddrive
    ```

1. Skapa en ny mapp med namnet **vm-config**.

    ```azurecli
    mkdir vm-config
    ```

1. Navigera till den nya mappen.

    ```azurecli
    cd vm-config
    ```

1. Skapa en ny fil med Cloud Shells integrerade redigerare.

    ```azurecli
    code .
    ```

1. Klistra in det här cloud-init-skriptet i redigeraren.

    [!include[](../../../includes/azure-cloudshell-copy-paste-tip.md)]

    ```yaml
    #cloud-config
    package_upgrade: true
    write_files:
      - path: /etc/systemd/system/docker.service.d/docker.conf
        content: |
          [Service]
          ExecStart=
          ExecStart=/usr/bin/dockerd
      - path: /etc/docker/daemon.json
        content: |
          {
            "hosts": ["fd://","tcp://127.0.0.1:2375"]
          }
    runcmd:
      - curl -sSL https://get.docker.com/ | sh
      - usermod -aG docker azureuser
    ```

1. Spara filen som **docker-vm-config.txt** (<kbd>Ctrl+S</kbd> eller högerklicka och välj **Spara**).

1. Stäng redigeraren (<kbd>Ctrl+Q</kbd> eller högerklicka och välj **Avsluta**).

### <a name="create-the-vm"></a>Skapa den virtuella datorn

> [!IMPORTANT]
> Normalt sett skulle du skapa en resursgrupp för alla dina relaterade Azure-resurser med kommandot `az group create`, men för de här övningarna har en resursgrupp redan skapats åt dig. Använd ”**<rgn>[resursgruppsnamn för sandbox-miljö]</rgn>**” som din resursgrupp i alla steg.

1. Kör kommandot `az vm create` för att skapa din virtuella Linux-dator.

    ```azurecli
    az vm create \
      --resource-group <rgn>[sandbox resource group name]</rgn> \
      --name DockerVM \
      --image UbuntuLTS \
      --admin-username azureuser \
      --generate-ssh-keys \
      --custom-data docker-vm-config.txt
    ```

`--custom-data`-argumentet anger cloud-init-skriptet som installerar Docker när den virtuella datorn först startas. Processen tar några minuter att slutföra.

## <a name="connect-to-the-vm"></a>Ansluta till den virtuella datorn

Här ska du skapa en SSH-anslutning till den virtuella datorn så att du kan konfigurera den.

1. Kör det här `az vm show`-kommandot för att hämta den virtuella datorns IP-adress.

    ```azurecli
    az vm show \
      --resource-group <rgn>[sandbox resource group name]</rgn> \
      --name DockerVM \
      --show-details \
      --query [publicIps] \
      --o tsv
    ```

1. SSH till den virtuella datorn. Ersätt **IP-adressen** med din adress.

    ```bash
    ssh azureuser@ip-address
    ```

1. Verifiera Docker-versionen.

    ```bash
    docker version
    ```

    > [!NOTE]
    > Om kommandot misslyckas eller om du ser ett felmeddelande om att ansluta till Docker-daemon-socket betyder det att cloud-init-skriptet inte har slutförts ännu. Även fast det finns olika sätt att vänta på att skriptet ska slutföras ska du tills vidare köra `exit` för att lämna din SSH-session och försöka ansluta igen om en minut eller två.

## <a name="start-nginx"></a>Starta Nginx

Här ska du starta en Docker-container som kör Nginx.

1. Kör det här `docker run`-kommandot för att starta en Docker-container som kör Nginx.

    ```bash
    docker run -d -p 8080:80 --name nginx nginx
    ```

    Kommandot laddar ned en Docker-avbildning som är förkonfigurerad med Nginx från [Docker Hub](https://hub.docker.com/_/nginx?azure-portal=true).

    `--name`-argumentet tilldelas namnet ”nginx” till din Docker-container så att du kan arbeta med den senare.

    `-p`-argumentet som vidarekopplar nätverkstrafik till port 8080 på den virtuella datorn till port 80 på containern.

1. Kör `curl` för att verifiera att Nginx är aktiv och tillgänglig.

    ```bash
    curl http://localhost:8080
    ```

1. Avsluta SSH-sessionen.

    ```bash
    exit
    ```

## <a name="access-your-web-server-from-a-web-browser"></a>Öppna din webbserver från en webbläsare

Kom ihåg att du konfigurerade din behållare så att trafik på port 8080 vidarekopplas till port 80 på containern.

Om du vill se portmappning i arbete ska du här öppna port 8080 till den virtuella datorn via Azure-brandväggen. Öppna sedan din webbserver från en webbläsare.

1. Kör det här `az vm open-port`-kommandot för att öppna port 8080 på din virtuella dator via brandväggen.

    ```azurecli
    az vm open-port \
      --resource-group <rgn>[sandbox resource group name]</rgn> \
      --name DockerVM \
      --port 8080 \
      --priority 1001
    ```

1. Öppna en webbläsare och navigera till den virtuella datorns IP-adress. Se till att inkludera port 8080, till exempel, **http://40.121.106.164:8080**.

    Du ser standardstartsidan.

    ![Webbplatsen som körs i en webbläsare](../media/2-nginx-browser.png)

## <a name="stop-the-container"></a>Stoppa containern

Stoppa containern nu.

1. Logga först in på den virtuella datorn. Här är en kort repetition.

    Hämta IP-adressen.

    ```azurecli
    az vm show \
      --resource-group <rgn>[sandbox resource group name]</rgn> \
      --name DockerVM \
      --show-details \
      --query [publicIps] \
      --o tsv
    ```

    SSH till den virtuella datorn. Ersätt **IP-adressen** med din adress.

    ```bash
    ssh azureuser@ip-address
    ```

1. Kör `docker stop nginx` för att stoppa containern med namnet ”nginx”.

    ```bash
    docker stop nginx
    ```

Håll din SSH-anslutning öppen för nästa del.

Grattis! Du har skapat en virtuell dator och använt den som värd för en Nginx-containeriserad webbserver.

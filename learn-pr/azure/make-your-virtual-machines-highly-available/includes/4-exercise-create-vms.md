Vi har en lastbalanserare för att ta emot inkommande Internettrafik och dirigera den. Nu behöver vi några virtuella Azure-datorer att dirigera trafik till. Vi använder Linux för det här exemplet, men exakt samma teknik skulle fungera med alla virtuella datoravbildningar.

## <a name="create-a-set-of-vms"></a>Skapa en uppsättning virtuella datorer

Vi ska skapa tre virtuella datorer och gruppera dem i en tillgänglighetsuppsättning med Azure CLI. Man kan använda portalen till den här uppgiften, men eftersom vi ska skapa flera resurser är det mycket enklare att göra detta via ett skriptverktyg.

En alternativ metod om du _verkligen_ föredrar att använda Azure-portalen är att använda en _mall_. Det här är JSON-instruktioner som kan användas för att distribuera resurser. Vi kan definiera en mall för den virtuella datorn och sedan distribuera mallen flera gånger i Azure-portalen.

### <a name="create-some-azure-cli-defaults"></a>Skapa några standardvärden för Azure CLI

1. Börja med att ange några standardinställningar i Azure CLI. Alla kommandon som vi använder kräver en _resursgrupp_ och en valfri _plats_. I stället för att skriva det varje gång kan vi ställa in det som en standardparameter. Använd den färdiga Azure sandbox-resursgruppen och samma plats som du har valt för Azure-lastbalanseraren. Här är en påminnelse om de tillgängliga platserna:

    [!include[](../../../includes/azure-sandbox-regions-note.md)]

1. Skriv följande kommando i Cloud Shell till höger och ersätt `<location>` platshållare med platsen som du använde för Azure-lastbalanseraren.

    ```azurecli
    az configure --defaults group=<rgn>[sandbox Resource Group</rgn> location=<location>
    ```

    [!include[](../../../includes/azure-cloudshell-copy-paste-tip.md)]

### <a name="create-an-availability-set"></a>Skapa en tillgänglighetsuppsättning

Börja med att skapa en _tillgänglighetsuppsättning_.

Vi behöver en tillgänglighetsuppsättning att gruppera våra virtuella datorer i. Vi kan använda `vm availability-set` kommandot för att skapa en. Den behöver bara ett namn så vi använder **woodgrove-avail-set**.

```azurecli
az vm availability-set create --name woodgrove-avail-set
```

### <a name="create-a-configuration-script"></a>Skapa ett konfigurationsskript

Vi vill använda samma grundläggande konfiguration för var och en av de virtuella datorerna.

    - Ubuntu Linux
    - nginx -webbserver
    - Node.js
    - Grundläggande webbplats med en enda sida för testning

Valet av OS är baserat på den avbildning som vi väljer, men de andra konfigurationselementen kräver en del skript. Nu ska vi skapa en lokal fil i Cloud Shell för att installera en webbserver och konfigurera den med en enkel webbplats.

1. Öppna Cloud Shell-redigeraren genom att skriva `code` i terminalen.

    ```bash
    code
    ```

    Det här är en inbyggd redigerare som du kan använda för att skapa och redigera filer direkt i Cloud Shell-miljön. Filerna sparas i Azure i ett lagringskonto som skapades när du startade Cloud Shell-miljön.

1. Kopiera följande skript och klistra in det i fönstret redigeraren.

    ```script
    #cloud-config
    package_upgrade: true
    packages:
      - nginx
      - nodejs
      - npm
    write_files:
      - owner: www-data:www-data
      - path: /etc/nginx/sites-available/default
        content: |
          server {
            listen 80;
            location / {
              proxy_pass http://localhost:3000;
              proxy_http_version 1.1;
              proxy_set_header Upgrade $http_upgrade;
              proxy_set_header Connection keep-alive;
              proxy_set_header Host $host;
              proxy_cache_bypass $http_upgrade;
            }
          }
      - owner: azureuser:azureuser
      - path: /home/azureuser/myapp/index.js
        content: |
          var express = require('express')
          var app = express()
          var os = require('os');
          app.get('/', function (req, res) {
            res.send('Hello World from host ' + os.hostname() + '!')
          })
          app.listen(3000, function () {
            console.log('Hello world app listening on port 3000!')
          })
    runcmd:
      - service nginx restart
      - cd "/home/azureuser/myapp"
      - npm init
      - npm install express -y
      - nodejs index.js
    ```

1. Spara filen och ge den namnet **cloud-init.txt**. Du kan använda snabbmenyn ”...” i det övre högra hörnet i redigeraren eller använda <kbd>Ctrl+S</kbd> i Windows och Linux, och <kbd>Cmd+S</kbd> på macOS.

1. Avsluta redigeraren – du kan återigen använda snabbmenyn ”...” eller ett kortkommando (<kbd>Ctrl+Q</kbd>).

1. Filen ska vara i filsystemet. Försök att använda `ls` kommandot för att visa innehållet i mappen. Du kan också `cat` filen för att kontrollera innehållet.

### <a name="create-the-virtual-machines"></a>Skapa de virtuella datorerna

Nu ska vi skapa tre virtuella Ubuntu Linux-datorer med Azure CLI. Vi går inte igenom alternativen här, men om du vill veta mer om hantering av virtuella datorer med CLI kan du kolla in modulen **Hantera virtuella datorer med Azure CLI**.

Vi måste också skapa ett virtuellt nätverksgränssnitt för varje virtuell dator. Vi kan använda en loop och skapa dem tillsammans.

1. Använd `vm-create` kommandot för att skapa tre virtuella datorer i en loop. Du kan använda följande kod för att:
    - Skapa ett nätverkskort med namnet _woodgrove_NicX_ där X är [1,2,3].
    - Skapa en virtuell dator med namnet _woodgrove-VMX_ där X är [1,2,3].
    - Associera varje nätverkskort till det skapade virtuella nätverket
    - Associera varje virtuell dator till uppsättningen med nätverkskort och tillgänglighet.

    ```azurecli
    for i in `seq 1 3`; do

        az network nic create --name woodgrove-Nic$i \
            --vnet-name woodgrove-VNET \
            --subnet backendSubnet

        az vm create --name woodgrove-VM$i \
            --availability-set woodgrove-avail-set \
            --nics woodgrove-Nic$i \
            --image UbuntuLTS \
            --admin-username azureuser \
            --generate-ssh-keys \
            --custom-data cloud-init.txt \
            --no-wait
    done
    ```
    > [!TIP]
    > Kommandot ställer in administratörsnamnet till ”azureuser” här, men du kan ändra till något annat. Du kan också ange ett lösenord om du föredrar användarnamn/lösenord istället för SSH-nycklar.

1. Detta kan ta ett tag att köra, och även när loopen är klar kommer de virtuella datorerna inte vara tillgängliga än. Du kan växla till Azure-portalen och använda displayen **Alla resurser** för att se de skapade resurserna.

1. Du kan också använda kommandot `vm list` för att se hur många virtuella datorer som har distribuerats.

    ```azurecli
    az vm list -o table
    ```

Vi kommer sedan att tala om hur man konfigurerar lastbalanseraren.
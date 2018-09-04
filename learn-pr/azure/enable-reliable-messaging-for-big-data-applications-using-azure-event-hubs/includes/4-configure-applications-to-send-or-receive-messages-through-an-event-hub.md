När du har skapat och konfigurerat din händelsehubb måste du konfigurera appar att skicka och ta emot händelsedataströmmar.

Till exempel använder en lösning för bearbetning av betalningar någon form av avsändarapp för att samla in data för kundens kreditkort och en mottagarapp för att verifiera att det registrerade kreditkortet är giltigt.

Det finns skillnader i hur en Java-app konfigureras, jämfört med en .NET-app, men det finns allmänna principer för att aktivera appar för att ansluta till en händelsehubb, och för att skicka eller ta emot meddelanden. Så även om processen för att redigera Java-textfiler för konfiguration skiljer sig från att förbereda en .NET-app med Visual Studio så är principerna är desamma.

## <a name="what-are-the-minimum-event-hub-application-requirements"></a>Vad är minimikraven för händelsehubbappar?

Om du vill konfigurera en app att skicka meddelanden till en händelsehubb måste du ange följande information, så att appen kan skapa autentiseringsuppgifter för anslutning:

- Namnrymdsnamn på händelsehubb
- Namn på händelsehubb
- Namn på princip för delad åtkomst
- Primär delad åtkomstnyckel

Om du vill konfigurera en app att ta emot meddelanden från en händelsehubb anger du följande information, så att appen kan skapa autentiseringsuppgifter för anslutning:

- Namnrymdsnamn på händelsehubb
- Namn på händelsehubb
- Namn på princip för delad åtkomst
- Primär delad åtkomstnyckel
- Namn på lagringskonto
- Anslutningssträng för lagringskonto
- Namn på lagringskontocontainer

Om du har en mottagarapp som lagrar meddelanden i Azure Blob Storage måste du också konfigurera ett lagringskonto.

## <a name="the-azure-cli-commands-for-creating-a-general-purpose-standard-storage-account"></a>Azure CLI-kommandon för att skapa ett allmänt standardlagringskonto

1. Skapa lagringskontot (generell användning V2) i din resursgrupp och på samma Azure-datacenterplats som du använde när du skapade resursgruppen.

    ```azurecli
    az storage account create --name <storage account name> --resource-group <resource group name> --location <location> --sku Standard_RAGRS --encryption blob
    ```
2. För att komma åt den här lagringen behöver du en åtkomstnyckel för lagringskontot. Visa åtkomstnycklarna som är associerade med lagringskontot.

    ```azurecli
    az storage account keys list --account-name <storage account name> --resource-group <resource group name>
    ```
3. Spara värdet som är associerat med **key1**.
4. Du behöver även anslutningsinformationen för lagringskontot. Visa anslutningssträngen för lagringskontot.

    ```azurecli
    az storage account show-connection-string -n <storage account name> -g <resource group name>
    ```
5. Spara värdet som är associerat med **connectionString**.
6. Meddelanden lagras i en container på ditt lagringskonto. Skapa en container på ditt lagringskonto med hjälp av `<connection string>` med anslutningssträngen från föregående steg.

    ```azurecli
    az storage container create -n <container> --connection-string "<connection string>"
    ```

## <a name="shell-command-for-cloning-an-application-github-repository"></a>Shell-kommando för att klona GitHub-lagringsplatsen för en app

Git är ett samarbetsverktyg som använder en kontrollmodell för distribuerad version, och har utformats för samarbete i programvaru- och dokumentationsprojekt. Git-klienter finns för flera plattformar, bland annat Windows, och Git-kommandoraden ingår i Azure Bash Cloud Shell. GitHub är en webbaserad värdtjänst för Git-lagringsplatser. 

Om du har en app som är värdbaserad som ett projekt i GitHub kan du skapa en lokal kopia av projektet genom att klona dess lagringsplats med kommandot **git clone**.

1. Klona ett datalager i hemkatalogen.

    ```azurecli
      git clone https://github.com/<project repository>
    ```

## <a name="shell-commands-for-preparing-an-application"></a>Shell-kommandon för att förbereda en app

1. Nu kan du använda ett redigeringsprogram, till exempel **nano**, till att redigera appen och lägga till händelsehubbens namnrymd, händelsehubbens namn, namnet på principen för delad åtkomst och primärnyckel. Nano är en enkelt textredigeringsprogram som är tillgängligt för Linux och andra Unix-lika operativsystem. Det är även tillgängligt i Azure Bash Cloud Shell. Du kan till exempel använda kommandot till att öppna en Java-programfil i **nano**-redigeraren.

    ```azurecli
    nano <application>.java
    ```

1. Beroende på hur dina appar har utvecklats kan du behöva kompilera eller skapa appen när du har lagt till händelsehubbens konfigurationsinformation. Till exempel måste Java-apparna som används i nästa enhet skapas med ett verktyg som Apache **Maven**. Maven kompilerar .java-filer i .class-filer eller paketerar dem i .jar-filer. Maven är tillgängligt via Bash Cloud Shell. Om du vill skapa Java-appen använder du **mvn**-kommandon. Använd det här mvn-kommandot till att skapa en Java-app.

    ```azurecli
    mvn clean package -DskipTests
    ```

## <a name="summary"></a>Sammanfattning

Avsändar- och mottagarappar måste konfigureras med specifik information om händelsehubbmiljön. Du skapar ett lagringskonto om mottagarappen lagrar meddelanden i Blob Storage. Om appen finns på GitHub måste du klona den i din lokala katalog. Textredigerare, som **nano**, används till att lägga till namnrymden i appen.
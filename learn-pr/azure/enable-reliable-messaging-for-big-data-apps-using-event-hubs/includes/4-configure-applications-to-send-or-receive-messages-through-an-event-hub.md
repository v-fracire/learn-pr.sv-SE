När du har skapat och konfigurerat din händelsehubb så måste du konfigurera appar att skicka och ta emot händelsedataströmmar.

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

## <a name="azure-cli-commands-for-creating-a-general-purpose-standard-storage-account"></a>Azure CLI-kommandon för att skapa ett allmänt standardlagringskonto

Azure CLI tillhandahåller en uppsättning kommandon som du kan använda för att skapa och hantera ett lagringskonto. Vi kommer att arbeta med dem i nästa enhet, men här är en grundläggande sammanfattning av kommandona. 

> [!TIP]
> Det finns flera MS Learn-moduler som täcker lagringskonton, från och med modulen **introduktion till Azure Storage**.

| Kommando | Beskrivning |
|---------|-------------|
| `storage account create` | Skapa ett v2-lagringskonto för generell användning. |
| `storage account key list` | Hämta lagringskontonyckeln. |
| `storage account show-connection-string` | Hämta anslutningssträngen för lagringskontot. |
| `storage container create` | Skapar en container i ett lagringskonto. |

## <a name="shell-command-for-cloning-an-application-github-repository"></a>Shell-kommando för att klona GitHub-lagringsplatsen för en app

Git är ett samarbetsverktyg som använder en kontrollmodell för distribuerad version, och har utformats för samarbete i programvaru- och dokumentationsprojekt. Git-klienter finns för flera plattformar, bland annat Windows, och Git-kommandoraden ingår i Azure Bash Cloud Shell. GitHub är en webbaserad värdtjänst för Git-lagringsplatser. 

Om du har en app som är värdbaserad som ett projekt i GitHub kan du skapa en lokal kopia av projektet genom att klona dess lagringsplats med kommandot **git clone**. Vi gör det i nästa enhet.

## <a name="editing-files-in-the-cloud-shell"></a>Redigera filer i Cloud Shell

Du kan använda någon av de inbyggda redigeringsprogrammen i Cloud Shell för att ändra alla filer som utgör programmet och lägga till ditt namnområde och namn för händelsehubben, namn för principen för delad åtkomst samt primär nyckel. 

Cloud Shell stöder **nano**, **vim** och **emacs** samt ett Visual Studio Code-liknande redigeraringsprogram som heter **code**. Skriv namnet på det redigeringsprogram som du vill ha så startas det i miljön. Vi använder redigeringsprogrammet **code** i nästa enhet.

## <a name="summary"></a>Sammanfattning

Avsändar- och mottagarappar måste konfigureras med specifik information om händelsehubbmiljön. Du skapar ett lagringskonto om mottagarappen lagrar meddelanden i Blob Storage. Om appen finns på GitHub måste du klona den i din lokala katalog. Textredigerare, som **nano**, används till att lägga till din namnrymd i appen.
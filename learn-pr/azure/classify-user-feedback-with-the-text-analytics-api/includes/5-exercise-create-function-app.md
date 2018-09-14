 En funktionsapp ger ett sammanhang för att hantera och köra dina funktioner. Nu ska vi skapa en funktionsapp och sedan lägga till en funktion i den. 

## <a name="create-a-function-app-to-host-our-function"></a>Skapa en Funktionsapp som värd för vår funktion

1. Logga in på [Azure Portal](https://portal.azure.com/?azure-portal=true).

1. Välj den **skapa en resurs** knappen hittades på det övre vänstra hörnet i Azure-portalen, sedan väljer **Compute** > **Funktionsapp**.

1. Ange sedan funktionsappinställningarna enligt tabellen nedan.

    | Inställning      | Föreslaget värde  | Beskrivning                                        |
    | ------------ |  ------- | -------------------------------------------------- |
    | **Appens namn** | Globalt unikt namn | Namn som identifierar din nya funktionsapp. Giltiga tecken är `a-z`, `0-9` och `-`.  | 
    | **Prenumeration** | Din prenumeration | Prenumerationen som den nya funktionsappen skapas under. | 
    | **Resursgrupp**|  <rgn>[Sandbox resursgruppens namn]</rgn> | Namnet på resursgruppen där du kan skapa din funktionsapp.<br/><br/>Se till att välja **Använd befintlig** och använda resursgruppen från föregående övning. På så sätt kan alla resurs som vi har gjort i den här modulen finns på samma ställe. | 
    | **OS** | Windows | Det operativsystem som är värd för funktionsappen.  |
    | **Som är värd för** |   Förbrukningsplan | Värdplan som definierar hur resurser allokeras till din funktionsapp. I **standardförbrukningsplanen** läggs resurser till dynamiskt när de behövs i funktionerna. För den här typen av [serverlösa](https://azure.microsoft.com/overview/serverless-computing/) värdtjänster betalar du bara för den tid som dina funktioner körs.   |
    | **Plats** | Välj i listan | Välj en [plats](https://azure.microsoft.com/regions/) nära dig eller nära andra tjänster som kommer att användas i dina funktioner.<br/><br/>Välj samma region som du använde när du skapar API för textanalys i föregående övning. |
    | **Lagringskonto** |  Globalt unikt namn |  Namnet på det nya lagringskonto som ska användas av funktionsappen. Namnet på ett lagringskonto måste vara mellan 3 och 24 tecken långt och får endast innehålla siffror och gemener. Den här dialogrutan fylls fältet med ett unikt namn som härleds från namnet du gav appen. Dock passa på att använda ett annat namn eller även ett befintligt konto. |

1. Välj **Skapa** för att etablera och distribuera funktionsappen.

1. Välj meddelandeikonen i det övre högra hörnet i portalen och titta efter en **distribution pågår** meddelande som liknar följande meddelande.

1. Distributionen kan ta lite tid. Så Håll dig i meddelandehubben och håll utkik efter en **distributionen lyckades** meddelande som liknar följande meddelande.

1. Grattis! Du har skapat och distribuerat din funktionsapp. Välj **Gå till resurs** att visa den nya funktionsappen.

> [!TIP]
> Är det svårt att hitta dina funktionsappar i portalen kan du prova att [lägga till funktionsappar till dina favoriter i Azure-portalen](https://docs.microsoft.com/en-us/azure/azure-functions/functions-how-to-use-azure-function-app-settings#favorite).

## <a name="create-a-function-to-execute-our-logic"></a>Skapa en funktion för att köra vår logik

Nu när vi har en funktionsapp är det dags att skapa en funktion. En funktion aktiveras via en utlösare. I den här modulen använder vi en kö-utlösare. Körningen ska avsöka en kö och starta den här funktionen för att bearbeta ett nytt meddelande.

1. Expandera den nya funktionsappen och klicka sedan hovra över den **Functions** samling. Välj Lägg till (__+__) knappen när den visas för att starta funktionen skapandeprocessen.

1. I den **Kom igång snabbt** där du nu visas, väljer du **anpassad funktion**, som läser in listan över tillgängliga funktionsmallar.

1. Välj **JavaScript** på den **köutlösare** listpost för mallen.

![Skärmbild av Azure Functions-mallar med JavaScript som valts på posten för kö-utlösare.](../media/quickstart-select-queue-trigger.png)

4. I den **ny funktion** dialogrutan som visas, ange följande värden.

|Egenskap  |Värde  |
|---------|---------|
|Språk     |   **JavaScript**      |
|Namn     |   **discover-sentiment-function**      |
|Könamn     |   **new-feedback-q**      |
|Lagringskontoanslutning        |  **AzureWebJobsDashboard**       |

![Skärmbild av Azure Functions-mallar med JavaScript som valts på posten för kö-utlösare.](../media/new-function-dialog.png)

5. Välj **skapa** att starta processen att skapa funktionen.

1. En funktion skapas i ditt valda språk med hjälp av mallen för Köutlösare-funktionen. Medan vi implementerar funktionen i JavaScript i den här modulen, kan du skapa en funktion i någon [språk som stöds](https://docs.microsoft.com/azure/azure-functions/supported-languages).

När processen är klar Kodredigeraren öppnas i portalen och belastning på *index.js* sidan. Den här filen är kodfilen där vi skriva vår funktion logik.

## <a name="try-it-out"></a>Prova

Nu ska vi testa vad vi har hittills. Vi har inte skriva någon kod ännu, så det här testet är att kontrollera vad vi har konfigurerat hittills körs.

1. Klicka på **kör** överst i kodredigeraren.

1. Notera den **loggar** fliken som öppnas längst ned på skärmen. Om allt fungerar som planerat, visas ett meddelande som liknar följande meddelande.
    ![Skärmbild av svarsmeddelandet av ett genomfört anrop till vår funktion.](../media/func-default-run.PNG)

Den **kör** knappen igång vår funktion och skickats *exempeldata kö*, standardtexten från den **Test** inställningsfönstret till vår funktion.

Bra jobbat! Du har lagt till en funktion som utlöses av kön till din funktionsapp och testats för att kontrollera att den fungerar som förväntat! Vi lägger till fler funktioner till funktionen i nästa övning.

Nu ska vi titta kort på funktionen användarens andra filer i *function.json* konfigurationsfilen. Konfigurationsdata från den här filen visas i följande JSON-lista.

```json
{
  "bindings": [
    {
      "name": "myQueueItem",
      "type": "queueTrigger",
      "direction": "in",
      "queueName": "new-feedback-q",
      "connection": "AzureWebJobsDashboard"
    }
  ],
  "disabled": false
}
```

Som du ser den här funktionen har en utlösare-bindning med namnet **myQueueItem** av typen `queueTrigger`. När ett nytt meddelande anländer i kön vi har med namnet **ny feedback q**, våra funktionen anropas. Vi refererar till det nya meddelandet via bindningsparametern myQueueItem. Bindningar verkligen ta hand om några av tunga jobb för oss!

I nästa steg ska vi lägger till kod för att anropa API för textanalys-tjänsten.

> [!TIP]
> Du kan se index.js och function.json genom att expandera den **visa filer** menyn till höger på panelen funktion i Azure-portalen.

Den här övningen var allt om att få vår Azure Functions-infrastruktur på plats. Vi har en fungerande funktion finns i en funktionsapp som körs när ett nytt meddelande anländer i vår kö som vi har namnet [!INCLUDE [input-q](./q-name-input.md)]. Det verkliga roligt börjar gälla i nästa övning kommer när vi lägger till kod för att anropa en Microsoft Cognitive Service för att göra känsloanalys.
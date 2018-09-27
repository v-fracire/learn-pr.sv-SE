 En funktionsapp ger ett sammanhang för att hantera och köra dina funktioner. Nu ska vi skapa en funktionsapp och sedan lägga till en funktion i den.

## <a name="create-a-function-app-to-host-our-function"></a>Skapa en funktionsapp som ska vara värd för vår funktion

1. Logga in på [Azure-portalen](https://portal.azure.com/learn.docs.microsoft.com?azure-portal=true) med samma konto som du har aktiverat sandbox-miljön med.

1. Klicka på knappen **Skapa en resurs** längst upp till vänster i Azure-portalen och sedan **Beräkning** > **Funktionsapp**.

1. Ange inställningarna för funktionsappen enligt nedanstående tabell.

    | Inställning      | Värde  | Beskrivning                                        |
    | ------------ |  ------- | -------------------------------------------------- |
    | **Appens namn** | Globalt unikt namn | Namn som identifierar din nya funktionsapp. Giltiga tecken är `a-z`, `0-9` och `-`.  |
    | **Prenumeration** | **Concierge-prenumeration** | Prenumerationen som den nya funktionsappen skapas under. |
    | **Resursgrupp**|  **<rgn>[resursgruppsnamn för sandbox]</rgn>** | Namnet på den resursgrupp där du vill skapa funktionsappen.<br/><br/>Se till att välja **Använd befintlig** och använd resursgruppen från föregående övning. På så sätt kommer alla resurser som vi har gjort i den här modulen finnas på samma ställe. |
    | **Operativsystem** | Windows | Det operativsystem som är värd för funktionsappen.  |
    | **Värd** |   Förbrukningsplan | Värdplan som definierar hur resurser allokeras till din funktionsapp. I standardinställningen **Förbrukningsplan** läggs resurser till dynamiskt när de krävs av funktionerna. För den här typen av [serverlösa](https://azure.microsoft.com/overview/serverless-computing/) värdtjänster betalar du bara för den tid som dina funktioner körs.   |
    | **Plats** | Välj samma plats som tidigare. | Välj en region nära dig eller nära andra tjänster som kommer att användas i dina funktioner.<br/><br/>Välj samma region som du använde när du skapade kontot i API för textanalys i föregående övning. |
    | **Lagringskonto** |  Globalt unikt namn |  Namnet på det nya lagringskonto som ska användas av funktionsappen. Namnet på ett lagringskonto måste vara mellan 3 och 24 tecken långt och får endast innehålla siffror och gemener. Den här dialogrutan fyller i fältet med ett unikt namn som härleds från namnet du gav appen. Du kan använda ett annat namn eller ett befintligt konto om du vill. |

1. Välj **Skapa** för att etablera och distribuera funktionsappen.

1. Välj meddelandeikonen i det övre högra hörnet av portalen och titta efter meddelandet **Distribution pågår**.

1. Distributionen kan ta lite tid. Stanna kvar i meddelandehubben och håll utkik efter ett meddelande om att **Distributionen lyckades**.

1. När funktionsappen har distribuerats går du till **Alla resurser** i portalen. Appen visas i listan med typen **Apptjänst** och har det namn du gav den. Välj funktionsappen från listan för att öppna den.

    Grattis! Du har skapat och distribuerat din funktionsapp.

> [!TIP]
> Är det svårt att hitta dina funktionsappar i portalen? Försök med att [lägga till funktionsappar till dina Favoriter i Azure Portal](https://docs.microsoft.com/azure/azure-functions/functions-how-to-use-azure-function-app-settings#favorite).

## <a name="create-a-function-to-execute-our-logic"></a>Skapa en funktion för att verkställa logiken

Nu när vi har en funktionsapp är det dags att skapa en funktion. En funktion aktiveras via en utlösare. I den här modulen använder vi en köutlösare. Körningen avsöker en kö och startar den här funktionen för att bearbeta ett nytt meddelande.

1. Expandera den nya funktionsappen och hovra sedan över samlingen **Funktioner**. Välj knappen Lägg till (__+__) när den visas och börja skapa funktionen.

1. På sidan **Kom igång snabbt** som nu visas väljer du **Anpassad funktion** längst ned på sidan som då läser in listan med tillgängliga funktionsmallar.

1. Välj **JavaScript** i mallistans **Köutlösare**.

    ![Skärmbild av Azure Functions-mallar med JavaScript valt i posten Köutlösare.](../media/quickstart-select-queue-trigger.png)

4. I dialogrutan **Ny funktion** som visas anger du nedanstående värden.

    |Egenskap  |Värde  |
    |---------|---------|
    |Språk     |   **JavaScript**      |
    |Namn     |   **discover-sentiment-function**      |
    |Könamn     |   **new-feedback-q**      |
    |Lagringskontoanslutning        |  **AzureWebJobsDashboard**       |

5. Välj **Skapa** för att börja skapa funktionen.

1. En funktion skapas på ditt valda språk med hjälp av funktionsmallen Köutlösare. Medan vi implementerar funktionen i JavaScript i den här modulen, kan du skapa en funktion på något av de [språk som stöds](https://docs.microsoft.com/azure/azure-functions/supported-languages).

När processen är klar öppnas kodredigeraren i portalen och läser in sidan *index.js*. Den här filen är kodfilen där vi skriver vår funktionslogik.

## <a name="try-it-out"></a>Prova

Nu ska vi testa vad vi har hittills. Vi har inte skrivit någon kod ännu, så det här testet innebär att kontrollera att det vi har konfigurerat hittills körs.

1. Klicka på **Kör** överst i kodredigeraren.

1. Observera fliken **Loggar** som öppnas längst ned på skärmen. Om allt fungerar som planerat, visas ett meddelande som liknar följande meddelande.
    ![Skärmbild av svarsmeddelandet vid ett genomfört anrop till vår funktion.](../media/func-default-run.PNG)

    Knappen **Kör** startade vår funktion och skickade *exempel på ködata* samt standardtexten från fönstret **Test** till vår funktion.

Bra jobbat! Du har lagt till en köutlöst funktion i din funktionsapp och testat att den fungerar som förväntat! Vi ska lägga till fler funktioner i nästa övning.

Nu ska vi ta en snabb titt på funktionens andra fil, konfigurationsfilen *function.json*. Konfigurationsdata från den här filen visas i följande JSON-lista.

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

Som du ser har den här funktionen en utlösarbindning med namnet **myQueueItem** av typen `queueTrigger`. När ett nytt meddelande anländer till kön med namnet **new-feedback-q**, anropas vår funktion. Vi refererar till det nya meddelandet via bindningsparametern myQueueItem. Bindningar tar verkligen hand om en del tidskrävande jobb åt oss!

I nästa steg ska vi lägga till kod för att anropa tjänsten API för textanalys.

> [!TIP]
> Du kan se index.js och function.json genom att expandera menyn **Visa filer** till höger på funktionspanelen i Azure Portal.

Den här övningen handlade om att få vår Azure Functions-infrastruktur på plats. Vi har en fungerande funktion i en funktionsapp som körs när ett nytt meddelande anländer i den kö som vi har gett namnet [!INCLUDE [input-q](./q-name-input.md)]. Det verkligt roliga börjar i nästa övning när vi lägger till kod som anropar Azure Cognitive Services för att göra en attitydanalys.
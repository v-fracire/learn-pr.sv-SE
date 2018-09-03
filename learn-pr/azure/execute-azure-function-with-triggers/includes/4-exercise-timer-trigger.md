I den här övningen ska vi skapa en Azure-funktion som anropas var 20:e sekund med hjälp av en timerutlösare.

> [!NOTE] 
> För att kunna slutföra den här övningen måste du vara inloggad på [Azure-portalen](https://portal.azure.com/) med ett giltigt konto.

## <a name="create-an-azure-function"></a>Skapa en Azure-funktion

Vi ska börja med att skapa en Azure-funktion i portalen.

1. Välj **Skapa en resurs** i det vänstra navigeringsfönstret.

1. Välj **Compute**.

1. Hitta och välj **funktionsapp**. Du kan även använda sökfältet för att hitta mallen.

    ![Välj funktionsapp](../media-drafts/4-click-function-app.png)

1. Ange ett unikt **Appnamn**.

1. Välj en **prenumeration**.

1. Skapa en ny **resursgrupp**.

1. Välj **Windows** som din **OS**.

1. Välj **Förbrukningsplan** som **värdplan**. Du debiteras för varje körning av funktionen. Resurser tilldelas automatiskt baserat på programmets arbetsbelastning.

1. Välj en **plats**.

1. Skapa ett nytt **lagringskonto**. (Det här värdet krävs, men vi kommer inte att använda det.)

1. Stäng av **Application Insights**.

1. Välj **Skapa**.

## <a name="create-a-timer-trigger"></a>Skapa en timerutlösare

Nu ska vi skapa en timerutlösare i vår Azure-funktion.

1. När Azure-funktionen har skapats väljer du **Alla resurser** i det vänstra navigeringsfönstret.

1. Hitta och välj din Azure-funktion.

1. Peka på **Functions** och välj plustecknet (+) på det nya bladet.

    ![Peka på Functions och välj plustecknet](../media-drafts/4-hover-function.png)

1. Välj **Timer**.

1. Välj **CSharp** som språk.

1. Välj **Skapa den här funktionen**.

## <a name="configure-the-timer-trigger"></a>Konfigurera timerutlösaren

Vi har en Azure-funktion med logik som skriver ut ett meddelande till loggfönstret. Vi ska ange timerns schema så att den körs var 20:e sekund.

1. Välj **Integrera**.

1. Ange följande värde i rutan **schema**:

    ```
    */20 * * * * *
    ```

1. Välj **Spara**.

## <a name="start-the-timer"></a>Starta timern

Nu när vi har konfigurerat timern är vi redo att starta den.

1. Välj **TimerTriggerCSharp1**. 

    > [!NOTE]
    > **TimerTriggerCSharp1** är standardnamnet. Det väljs automatiskt när du skapar utlösaren.

1. Välj **Kör**. 

Du bör nu se ett meddelande i loggfönstret var 20:e sekund.

## <a name="clean-up"></a>Rensa

För att säkerställa att du inte debiteras för den här funktionen, väljer du **Pausa** ovanför loggfönstret för att stoppa timern.

![Pausa](../media-drafts/4-pause-timer.png)



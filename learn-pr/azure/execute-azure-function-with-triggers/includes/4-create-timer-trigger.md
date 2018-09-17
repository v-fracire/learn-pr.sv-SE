I den här övningen ska vi skapa en Azure-funktion som anropas var 20:e sekund med hjälp av en timerutlösare.

## <a name="create-an-azure-function"></a>Skapa en Azure-funktion

Vi ska börja med att skapa en Azure-funktion på portalen.

1. Logga in på [Azure-portalen](https://portal.azure.com?azure-portal=true).

1. Välj **Skapa en resurs** i det vänstra navigeringsfönstret.

1. Välj **Beräkna**.

1. Leta upp och välj **Funktionsapp**. Du kan även använda sökfältet för att hitta mallen.

    ![Skärmbild av Azure Portal som visar bladet Skapa ett resursblad med Funktionsapp markerat.](../media/4-click-function-app.png)

1. Ange ett unikt **appnamn**.

1. Välj en **prenumeration**.

1. Skapa en ny **resursgrupp**.

1. Välj **Windows** som **operativsystem**.

1. Välj **Förbrukningsplan** som **värdplan**. Du debiteras för varje körning av funktionen. Resurser tilldelas automatiskt baserat på programmets arbetsbelastning.

1. Välj en **plats**.

1. Skapa ett nytt **lagringskonto**. Du kan ändra namnet om du vill – standardnamnet är en variant av appnamnet

1. Inaktivera **Application Insights**.

1. Välj **Skapa**. Åtgärden tar några minuter. Titta på **meddelandeikonen** i verktygsfältsområdet. När resursen har skapats visas en knapp där som du kan klicka på för att öppna resursen på Azure Portal.

## <a name="create-a-timer-trigger"></a>Skapa en timerutlösare

Nu ska vi skapa en timerutlösare i vår Azure-funktion.

1. När Azure-funktionen har skapats väljer du **Alla resurser** i det vänstra navigeringsfönstret.

1. Leta upp och välj din Azure-funktion.

1. Peka på **Funktioner** och välj plustecknet (+) på det nya bladet.

    ![Skärmbild av Azure Portal som visar ett funktionsappblad med knappen Lägg till (+) för undermenyn Funktioner markerad.](../media/4-hover-function.png)

1. Välj **Timer**.

1. Välj **CSharp** som språk.

1. Välj **Skapa den här funktionen**.

## <a name="configure-the-timer-trigger"></a>Konfigurera timerutlösaren

Vi har en Azure-funktion med logik som skriver ut ett meddelande till loggfönstret. Vi ska ange timerns schema så att den körs var 20:e sekund.

1. Välj **Integrera**.

1. Ange följande värde i rutan **Schema**:

    ```log
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
<!---TODO: Update for sandbox?--->

För att undvika debitering för den här funktionen väljer du **Pausa** ovanför loggfönstret för att stoppa timern.

![Skärmbild av Azure Portal med en utdatapanel för funktionsappsloggar, med knappen Paus markerad.](../media/4-pause-timer.png)

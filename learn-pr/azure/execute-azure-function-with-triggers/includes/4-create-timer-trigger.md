I den här övningen ska vi skapa en Azure-funktion som anropas var 20:e sekund med hjälp av en timerutlösare.

> [!NOTE] 
> För att kunna slutföra den här övningen måste du vara inloggad på [Azure Portal](https://portal.azure.com?azure-portal=true) med ett giltigt konto.

## <a name="create-an-azure-function"></a>Skapa en Azure-funktion

Vi ska börja med att skapa en Azure-funktion på portalen.

1. Välj **Skapa en resurs** i det vänstra navigeringsfönstret.

2. Välj **Beräkna**.

3. Leta upp och välj **Funktionsapp**. Du kan även använda sökfältet för att hitta mallen.

    ![Välj Funktionsapp](../media-drafts/4-click-function-app.png)

4. Ange ett unikt **appnamn**.

5. Välj en **prenumeration**.

6. Skapa en ny **resursgrupp**.

7. Välj **Windows** som **operativsystem**.

8. Välj **Förbrukningsplan** som **värdplan**. Du debiteras för varje körning av funktionen. Resurser tilldelas automatiskt baserat på programmets arbetsbelastning.

9. Välj en **plats**.

10. Skapa ett nytt **lagringskonto**. Du kan ändra namnet om du vill – standardnamnet är en variant av appnamnet

11. Inaktivera **Application Insights**.

12. Välj **Skapa**. Åtgärden tar några minuter. Titta på **meddelandeikonen** i verktygsfältsområdet. När resursen har skapats visas en knapp där som du kan klicka på för att öppna resursen på Azure Portal.

## <a name="create-a-timer-trigger"></a>Skapa en timerutlösare

Nu ska vi skapa en timerutlösare i vår Azure-funktion.

1. När Azure-funktionen har skapats väljer du **Alla resurser** i det vänstra navigeringsfönstret.

2. Leta upp och välj din Azure-funktion.

3. Peka på **Funktioner** och välj plustecknet (+) på det nya bladet.

    ![Peka på Funktioner och välj plustecknet](../media-drafts/4-hover-function.png)

4. Välj **Timer**.

5. Välj **CSharp** som språk.

6. Välj **Skapa den här funktionen**.

## <a name="configure-the-timer-trigger"></a>Konfigurera timerutlösaren

Vi har en Azure-funktion med logik som skriver ut ett meddelande till loggfönstret. Vi ska ange timerns schema så att den körs var 20:e sekund.

1. Välj **Integrera**.

2. Ange följande värde i rutan **Schema**:

    ```
    */20 * * * * *
    ```

3. Välj **Spara**.

## <a name="start-the-timer"></a>Starta timern

Nu när vi har konfigurerat timern är vi redo att starta den.

1. Välj **TimerTriggerCSharp1**. 

    > [!NOTE]
    > **TimerTriggerCSharp1** är standardnamnet. Det väljs automatiskt när du skapar utlösaren.

2. Välj **Kör**. 

Du bör nu se ett meddelande i loggfönstret var 20:e sekund.

## <a name="clean-up"></a>Rensa

För att undvika att debiteras för den här funktionen väljer du **Pausa** ovanför loggfönstret för att stoppa timern.

![Pausa](../media-drafts/4-pause-timer.png)



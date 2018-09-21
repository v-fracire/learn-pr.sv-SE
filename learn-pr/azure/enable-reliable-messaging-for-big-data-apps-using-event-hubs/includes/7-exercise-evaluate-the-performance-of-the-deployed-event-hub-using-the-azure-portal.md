I den här delen använder du Azure-portalen för att kontrollera att din händelsehubb fungerar och att den uppfyller dina förväntningar. Du ska också testa hur meddelanden från händelsehubben fungerar när den är tillfälligt otillgänglig och använda Event Hubs-mått för att kontrollera händelsehubbens prestanda.

## <a name="view-event-hub-activity"></a>Visa aktivitet på händelsehubb

1. Logga in på [Azure-portalen](https://portal.azure.com/triplecrownlabs.onmicrosoft.com?azure-portal=true) med samma konto som du använde för att aktivera sandbox-miljön.

1. Leta upp din händelsehubb med hjälp av sökfältet och öppna den.

1. Kontrollera antalet meddelanden på sidan Översikt.

    ![Skärmbild av Azure-portalen som visar ett Event Hub-namnområde med antal meddelanden](../media/6-view-messages.png)

1. Programmen SimpleSend och EventProcessorSample har konfigurerats att skicka och ta emot 100 meddelanden. Du ser att händelsehubben har bearbetat 100 meddelanden från SimpleSend-programmet och skickat 100 meddelanden till EventProcessorSample-programmet.

## <a name="test-event-hub-resilience"></a>Testa händelsehubbens återhämtningsförmåga

Följ stegen nedan för att se vad som händer när ett program skickar meddelanden till en händelsehubb när den är tillfälligt otillgänglig.

1. Skicka om meddelanden till händelsehubben med hjälp av SimpleSend-programmet. Ange följande kommando:

    ```azurecli
    cd ~
    cd azure-event-hubs/samples/Java/Basic/SimpleSend
    java -jar ./target/simplesend-1.0.0-jar-with-dependencies.jar
    ```

1. Tryck på <kbd>RETUR</kbd> när du ser **Överföringen är klar...**.

1. Välj din händelsehubb på skärmen **översikt** – detta visar information om händelsehubben. Du kan också hämta den här skärmen via posten **Händelsehubbar** på namnområdessidan.

1. Välj **Inställningar** > **egenskaper**.

1. Klicka på **Inaktiverad** under Event Hub state (Event Hub-tillstånd). Spara ändringarna.

    ![Inaktivera händelsehubb](../media/7-disable-event-hub.png)

    **Vänta minst fem minuter.**

1. Klicka på **Aktiv** under Event Hub state (tillstånd för händelsehubb) för att återaktivera händelsehubben och spara dina ändringar.

1. Kör programmet EventProcessorSample igen för att ta emot meddelanden. Använd följande kommando.

    ```azurecli
    cd ~
    cd azure-event-hubs/samples/Java/Basic/EventProcessorSample
    java -jar ./target/eventprocessorsample-1.0.0-jar-with-dependencies.jar
    ```

1. När meddelanden slutar att visas i konsolen trycker du på <kbd>RETUR</kbd>.

1. Gå tillbaka till händelsehubbens namnområde på Azure-portalen. Om du befinner dig på händelseportalsidan, kan du använda länken överst på skärmen för att gå bakåt. Du kan också söka efter namnområdet och välja det.

1. Klicka på måttet **ÖVERVAKNING** >  **(förhandsgranskning)**.

    ![Skärmbild som visar mått för händelseövervakning med där antalet inkommande och utgående meddelanden visas.](../media/7-event-hub-metrics.png)

1. Välj **Inkommande meddelanden** i listan **Mått** och klicka på **Lägg till mått**.

1. Välj **Utgående meddelanden** i listan **Mått** och klicka på **Lägg till mått**.

1. Klicka på **Senaste 24 timmarna (automatiskt)** överst i diagrammet och ändra tidsperioden till **Senaste 30 minuterna** för att expandera datadiagrammet.

Du ser att även om meddelandena skickades innan händelsehubben tillfälligt kopplades från så överfördes alla 100 meddelanden korrekt.

## <a name="summary"></a>Sammanfattning

I den här kursdelen har du använt Event Hubs-mått för att testa att din händelsehubb bearbetar de meddelanden som skickas och tas emot korrekt.

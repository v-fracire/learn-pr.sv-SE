Hittills har du anslutit en kaffebryggare till Azure IoT Central-programmet, vilket aktiverar utbyte av data som gör att du kan övervaka och hantera kaffebryggaren. I den här delen skapar du regler som utlöser åtgärder när vattentemperaturen i kaffebryggaren ligger utanför det normala intervallet. 

## <a name="create-rules-in-iot-central-with-email-as-the-action"></a>Skapa regler i IoT Central med e-postmeddelanden som åtgärd

Azure IoT Central har en inbyggd funktion för att skicka meddelanden. I det här scenariot gäller följande: om kaffebryggaren ligger utanför temperaturintervallet och inte har en gällande garanti skickas ett e-postmeddelande från IoT Central till kundens underhållsavdelning.

1. Navigera till sidan **Regler** för övningarna i den här delen, och börja använda redigeringsläget genom att välja **Redigera mall** till höger. 
1. Välj **+ Ny regel** och sedan **Telemetri**. 

1. Ange namnet `Coffee Maker Water Too Cold (Expired)`

1. Lägg till följande villkor till regeln genom att välja plustecknet (**+**) till höger om **Villkor** och sedan klicka på **Spara**:      
    - Vattentemperaturen är lägre än kaffebryggarens lägsta temperatur
    - Device Warranty Expired (Enhetens garanti har upphört) är lika med 1

    ![Använda regeln](../media/5-flow-a.png)

1. Rulla nedåt i panelen **Konfigurera telemetriregel** och välj **+** bredvid **Åtgärder**. Välj sedan **E-post**.

1. Ange den e-postadress som du använde när du loggade in i IoT Central-programmet och lägg till anteckningen `Coffee maker's water is too cold. Maintenance is required.  Warranty has expired.` (Kaffebryggarens vatten är för kallt. Underhåll krävs. Garantin har gått ut.)

1. Välj **Spara**. Din regel finns på sidan **Regler**.

Nu upprepar vi de här stegen för en situation då vattnet är för hett. 

1. Välj **+ Ny regel** och sedan **Telemetri**.

1. Lägg till en ny regel och ge den namnet `Coffee Maker Water Too Hot (Expired)` (Kaffebryggarens vatten för hett (upphört))

1. Lägg till följande villkor till regeln genom att välja plustecknet (**+**) till höger om **Villkor** och sedan klicka på **Spara**:      
    - Vattentemperaturen är högre än kaffebryggarens högsta temperatur
    - Device Warranty Expired (Enhetens garanti har upphört) är lika med 1

1. Rulla nedåt i panelen **Konfigurera telemetriregel** och välj **+** bredvid **Åtgärder**. Välj sedan **E-post**.

1. Ange den e-postadress som du använde när du loggade in i IoT Central-programmet och lägg till anteckningen `Coffee maker's water is too cold. Maintenance is required.  Warranty has expired.` (Kaffebryggarens vatten är för kallt. Underhåll krävs. Garantin har gått ut.)

1. Välj **Spara**. Din regel listas på sidan **Regler**.

För att utlösa regeln anger du optimal temperatur i **Inställningar** utanför det intervall som du angav under **Egenskaper**. När du är klar med valideringen stänger du av reglerna för att undvika att överbelasta inkorgen med e-postmeddelanden.
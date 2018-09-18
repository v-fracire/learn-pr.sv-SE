Nu har du arbetat med både Azure IoT Central-programmet och anslutit kaffebryggaren till Azure IoT Central. Du är på väg att börja övervaka och hantera din fjärranslutna kaffebryggare. I den här verifierar du din konfiguration och anslutning genom att använda mallen Ansluten kaffebryggare som du definierade tidigare. Du uppdaterar den optimala temperaturen i Inställningar, kör kommandon för att kontrollera tillståndet för datorn och visar din anslutna kaffebryggare i instrumentpanelen. 

## <a name="update-settings-to-sync-your-application-with-the-coffee-machine"></a>Uppdatera inställningarna för att synkronisera programmet med kaffebryggaren

På sidan Inställningar skickar du konfigurationsdata till kaffebryggaren från ditt program. 

I det här scenariot ändrar du den optimala temperaturen och väljer **Uppdatera**. När inställningen ändras markeras inställningen som väntande i användargränssnittet tills kaffebryggaren bekräftar att den har besvarat den ändrade inställningen. 

> [!NOTE]
> Lyckade uppdateringar i inställningen indikerar dataflöde och verifierar anslutningen. Telemetrimätningarna telemetri besvarar uppdateringen av optimal temperatur. Du ser ändringen på sidan Mått. 

## <a name="run-commands-on-the-coffee-machine"></a>Köra kommandon för kaffebryggaren 
Navigera till sidan **Kommandon** för följande övning. För att verifiera kommandokonfigurationen kör du kommandon via en fjärranslutning på kaffebryggaren från IoT Central. Om detta lyckas skickas bekräftelsemeddelanden från kaffebryggaren.

1. Börja brygga via fjärranslutning genom att välja **Kör**. 
    
    Kaffebryggaren startar om följande tre villkor är uppfyllda:
    - En kopp har identifierats
    - Bryggaren är inte i underhållsläge
    - Någon bryggning pågår inte redan  

    > [!NOTE]
    > När du har startat bryggningen ändras bryggarens status gult, vilket indikeras under Mått > Status. 
    
    Sök efter bekräftelsemeddelanden i kaffebryggarens konsollogg. 

    ![Köra kommandon](../images/4-commands-brewing.png)

1. Ställ in underhållsläge genom att välja **Kör**. Kaffebryggaren sätts i underhållsläge om den *inte* redan är i underhåll.
    
    Sök efter bekräftelsemeddelanden i kaffebryggarens konsollogg. 

    > [!NOTE]
    > Liksom i verkligheten när en tekniker tar en maskin offline för att utföra nödvändiga reparationer innan han sätter den online igen, så fortsätter kaffebryggaren att vara i underhållsläge tills du startar om klientkoden.

    ![Köra kommandon](../images/4-commands-maintenance.png)

1. Det rekommenderas att du kör Node.js-program i högst ca 60 minuter för att förhindra att programmet skickar dig oönskade meddelanden/e-postmeddelanden. Genom att stoppa programmet när du inte arbetar med självstudien förhindrar du även att den dagliga meddelandekvoten tar slut.

## <a name="view-the-coffee-machine-in-the-dashboard"></a>Visa kaffebryggaren på instrumentpanelen
Navigera till sidan **Instrumentpanel**, där du ser all samlad information om kaffebryggaren. För följande övning aktiverar du **Designläge** för att konfigurera instrumentpanelen. När du är klar väljer du **Spara**.

1. Välj **Linjediagram** och ange rubriken Telemetri om du vill se telemetrimått. Välj **Senaste 30 minuterna** för **Tidsintervall**.

    ![Visa instrumentpanelen](../images/4-dashboard-a.png)

1. Välj **Inställningar och egenskaper** och ange rubriken Enhetsegenskaper. I **Lägg till/ta bort** väljer du Maxtemperatur kaffebryggare, Minimitemperatur kaffebryggare och Enhetsgaranti utgången. 

1. Välj **Inställningar och egenskaper** och ange rubriken Optimal temperatur. I **Lägg till/ta bort** väljer du Optimal temperatur. 

## <a name="summary"></a>Sammanfattning

I den här enheten har du ägnat viss tid åt att verifiera anslutningen mellan kaffebryggaren och Azure IoT Central. Du uppnått verifiering genom att uppdatera den optimala temperaturen och köra kommandona. Avslutningsvis konfigurerar du instrumentpanelen för övervakning av bryggaren på en plats genom att definiera vilken information om kaffebryggaren som du vill ska visas. De här verifieringsstegen krävs för att du ska kunna gå vidare till uppgifterna i nästa enhet. 
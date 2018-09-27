Nu har du arbetat med Azure IoT Central-programmet och anslutit kaffebryggaren till Azure IoT Central. Du är på väg att börja övervaka och hantera din fjärranslutna kaffebryggare. I den här verifierar du din konfiguration och anslutning genom att använda mallen Ansluten kaffebryggare som du definierade tidigare. Du uppdaterar den optimala temperaturen i Inställningar, kör kommandon för att kontrollera tillståndet för datorn och visar din anslutna kaffebryggare i instrumentpanelen. 

## <a name="update-settings-to-sync-your-application-with-the-coffee-machine"></a>Uppdatera inställningarna för att synkronisera programmet med kaffebryggaren

På sidan **Inställningar** skickar du konfigurationsdata till kaffebryggaren från ditt program. 

I det här scenariot ändrar du den optimala temperaturen och väljer **Uppdatera**. När inställningen ändras markeras inställningen som väntande i användargränssnittet tills kaffebryggaren bekräftar att den har besvarat den ändrade inställningen. 

> [!NOTE]
> Lyckade uppdateringar i inställningen indikerar dataflöde och verifierar anslutningen. Telemetrimätningarna telemetri besvarar uppdateringen av optimal temperatur. Du kan se ändringen på sidan **Mått**. 

## <a name="run-commands-on-the-coffee-machine"></a>Kör kommandon på kaffebryggaren 
Navigera till sidan **Kommandon** för följande övning. För att verifiera kommandokonfigurationen kör du kommandon via en fjärranslutning på kaffebryggaren från IoT Central. Om kommandona lyckas skickas bekräftelsemeddelanden från kaffebryggaren.

1. Börja brygga via fjärranslutning genom att välja **Kör**. 
    
    Kaffebryggaren startar om följande tre villkor är uppfyllda:
    - En kopp har identifierats
    - Bryggaren är inte i underhållsläge
    - Någon bryggning pågår inte redan  

    > [!NOTE]
    > När du har startat bryggningen ändras bryggarens status till gult, vilket indikeras under **Mått** > **Status**. 
    
    Sök efter bekräftelsemeddelanden i den simulerade kaffebryggarens konsollogg för node.js. 

    ![Köra kommandon](../media/4-commands-brewing.png)

1. Ställ in underhållsläge genom att välja **Kör**. Kaffebryggaren sätts i underhållsläge om den *inte* redan är i underhåll.
    
    Sök efter bekräftelsemeddelanden i kaffebryggarens konsollogg. 

    > [!NOTE]
    > Liksom i verkligheten när en tekniker tar en maskin offline för att utföra nödvändiga reparationer innan han sätter den online igen, så fortsätter kaffebryggaren att vara i underhållsläge tills du startar om klientkoden.

    ![Kör kommandon](../media/4-commands-maintenance.png)

> [!IMPORTANT]
> Det rekommenderas att du kör Node.js-program i högst ca 60 minuter för att förhindra att programmet skickar dig oönskade meddelanden/e-postmeddelanden. Genom att stoppa programmet när du inte arbetar med modulen förhindrar du även att den dagliga meddelandekvoten tar slut.

## <a name="view-the-coffee-machine-in-the-dashboard"></a>Visa kaffebryggaren på instrumentpanelen

1. Gå till fliken **Instrumentpanel**.

1. Välj **Redigera mall** för att redigera instrumentpanelen.

1. Välj **Linjediagram** på sidomenyn.

1. Under **Konfigurera diagram** anger du `Telemetry` i fältet **Rubrik**. Vi ska visa telemetridata med det här diagrammet. 

1. Välj **Senaste 30 minuterna** för **Tidsintervall**. 

1. I området **Mått** väljer du synlighetsikonen till höger om varje mått för att göra måttet synligt i diagrammet. 

1. Välj **Spara** för att spara konfigurationen och visa linjediagrammet. 

    ![Visa instrumentpanelen](../media/4-dashboard-a.png)

1. Välj **Inställningar och egenskaper** i den vänstra menyn för att öppna panelen **Configure Device Details** (Konfigurera enhetsinformation). 

1. Ange `Device properties` i fältet **Rubrik**.

1. I **Lägg till/ta bort** väljer du **Coffee Makers Max Temperature** (Kaffebryggarens högsta temperatur), **Coffee Makers Min Temperature** (Kaffebryggarens lägsta temperatur), **Device Warranty Expired** (Enhetsgaranti utgången) och sedan **OK** för att slutföra valen.

1. Välj **Spara** för att skapa en ny panel för *Enhetsegenskaper* på instrumentpanelen. 

1. Välj **Inställningar och egenskaper** igen och ange `Optimal Temperature` som rubrik. I **Lägg till/ta bort** väljer du **Optimal temperatur**.

1. Välj **Spara** för att spara arbetet och visa ett nytt fönster på instrumentpanelen. 

1. Välj **Klar** för att avsluta läget och visa den nya instrumentpanelen. 
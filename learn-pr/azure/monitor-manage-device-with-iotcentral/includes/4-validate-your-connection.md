Nu har du arbetat med både Azure IoT Central-programmet och anslutit kaffebryggaren till Azure IoT Central. Du är på väg att börja övervaka och hantera din fjärranslutna kaffebryggare. I den här modulen får du ägna en stund åt att verifiera din konfiguration och anslutning genom att uppdatera den optimala temperaturen i inställningarna, köra kommandon för att kontrollera maskinens tillstånd och visa den anslutna kaffebryggaren på instrumentpanelen. 

## <a name="edit-setting-to-see-if-the-application-syncs-with-the-coffee-machine"></a>Redigera inställningen och se om programmet synkroniseras med kaffebryggaren

Skicka konfigurationsdata till kaffebryggaren från inställningarna i ditt program. I det här scenariot väljer vi att ändra temperaturen och välja **Uppdatera**. När inställningen ändras markeras inställningen som väntande i användargränssnittet tills kaffebryggaren bekräftar att den har besvarat den ändrade inställningen. Lyckade uppdateringar i inställningen indikerar dataflöde och verifierar anslutningen. Telemetrimätningarna telemetri besvarar uppdateringen av optimal temperatur. Du ser ändringen på sidan Mått. 

  ![Uppdatera inställningar](../images/3-settings-a.png)

## <a name="run-commands-and-receive-confirmation-messages-sent-by-the-coffee-machine"></a>Kör kommandon och få bekräftelsemeddelanden som skickas från kaffebryggaren 
Aktivera **Designläge** och lägg till **Nya kommandon**.

1. Starta fjärrbryggningen. Kaffebryggaren startar om följande tre villkor är uppfyllda:
    - En kopp har identifierats
    - Bryggaren är inte i underhållsläge
    - Någon bryggning pågår inte redan

    När du har startat bryggningen ändras bryggarens status gult, vilket indikeras under Mått > Status. 
    ![Kör kommandon](../images/3-commands-b.png)

    Sök efter bekräftelsemeddelanden i den simulerade kaffebryggarens konsollogg. 
    ![Kör kommandon](../images/3-commands-brewing.png)

2. Ange Underhållsläge. Kaffebryggaren sätts i underhållsläge om den *inte* redan är i underhållsläge.
    ![Kör kommandon](../images/3-commands-c.png)
    
    Sök efter bekräftelsemeddelanden i den simulerade kaffebryggarens konsollogg. 
    > [!NOTE]
    > Liksom i verkligheten när en tekniker tar en maskin offline för att utföra nödvändiga reparationer innan han sätter den online igen, så fortsätter den simulerade kaffebryggaren att vara i underhållsläge tills du startar om klientkoden.
    ![Kör kommandon](../images/3-commands-maintenance.png)

## <a name="view-the-coffee-machine-in-the-dashboard"></a>Visa kaffebryggaren på instrumentpanelen
Det är på instrumentpanelen som du ser all samlad information om kaffebryggaren. 

1. Välj **Linjediagram** och ange rubriken Temperatur om du vill se telemetrin. Välj **De senaste 30 minuter** om du vill se ett **tidsintervall**.
![Visa instrumentpanelen](../images/3-dashboard-a.png)

1. Välj **Inställningar och egenskaper** och ange rubriken Enhetsegenskaper. Välj Maxtemperatur kaffebryggare, Minimitemperatur kaffebryggare, Enhetsgaranti utgången i **Lägg till/ta bort**. 
![Visa instrumentpanelen](../images/3-dashboard-b.png)

1. Välj **Inställningar och egenskaper** och ange rubriken Optimal temperatur. Välj Optimal temerpatur i **Lägg till/ta bort**. 
![Visa instrumentpanelen](../images/3-dashboard-c.png)


## <a name="summary"></a>Sammanfattning

I den här modulen har du ägnat viss tid åt att verifiera anslutningen mellan den simulerade kaffebryggaren och Azure IoT Central. Du uppnått verifiering genom att uppdatera den optimala temperaturen och köra kommandona. Avslutningsvis konfigurerar du instrumentpanelen för övervakning av bryggaren på en plats genom att definiera vilken information om kaffebryggaren som du vill ska visas. De här verifieringsstegen krävs för att du ska kunna gå vidare till uppgifterna i nästa modul. 
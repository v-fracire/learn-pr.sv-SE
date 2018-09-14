Nu har du arbetat med både Azure IoT Central-programmet och anslutit kaffebryggaren till Azure IoT Central. Du är på väg att börja övervaka och hantera din fjärranslutna kaffebryggare. I den här enheten ta en stund att verifiera din konfiguration och anslutning med hjälp av ansluten kaffe Maker-mallen som du angav tidigare. Du uppdatera optimala temperaturen i inställningar, kör kommandon för att söka efter tillståndet för din dator och visa anslutna kaffe datorn i instrumentpanelen. 

## <a name="update-settings-to-sync-your-application-with-the-coffee-machine"></a>Uppdatera inställningarna för att synkronisera dina program med kaffe-dator

På sidan Inställningar skicka konfigurationsdata till kaffe datorn från ditt program. 

I det här scenariot väljer vi att ändra temperaturen och välja **Uppdatera**. När inställningen ändras markeras inställningen som väntande i användargränssnittet tills kaffebryggaren bekräftar att den har besvarat den ändrade inställningen. 

> [!NOTE]
> Lyckade uppdateringar i inställningen ange dataflöde och verifiera din anslutning. Mätningar telemetri kommer att besvara uppdateringen i optimala temperatur. Du ser ändringen på sidan Mått. 

## <a name="run-commands-on-the-coffee-machine"></a>Kör kommandon på datorn kaffe 
Navigera till den **kommandon** för följande övning. För att verifiera installationen kommandon kan köra du via en fjärranslutning kommandon på datorn kaffe från IoT Central. Om detta lyckas skickas bekräftelsemeddelanden från kaffe-datorn.

1. Starta Brewing via fjärranslutning genom att välja **kör**. 
    
    Kaffebryggaren startar om följande tre villkor är uppfyllda:
    - En kopp har identifierats
    - Bryggaren är inte i underhållsläge
    - Någon bryggning pågår inte redan  

    > [!NOTE]
    > När du har startat bryggningen ändras bryggarens status gult, vilket indikeras under Mått > Status. 
    
    Leta efter bekräftelsemeddelanden i konsolloggen på kaffe-datorn. 

    ![Kör kommandon](../images/4-commands-brewing.png)

1. Ange underhållsläge genom att välja **kör**. Anger den kaffe datorn till underhåll om det är *inte* redan i underhållsläge.
    
    Leta efter bekräftelsemeddelanden i konsolloggen på kaffe-datorn. 

    > [!NOTE]
    > Som i verkliga livet är vanligtvis när teknikern tar datorn offline för att utföra nödvändiga reparationer innan du växlar den online igen, fortsätter kaffe datorn vara i underhållsläge förrän du startar om klientkoden.

    ![Kör kommandon](../images/4-commands-maintenance.png)

1. Vi rekommenderar att du kör Node.js-program inte fler än 60 minuter, eller så att programmet från att skicka oönskade meddelanden/e-postmeddelanden. Stoppa programmet när du inte arbetar med självstudien förhindrar även att du få slut den dagliga meddelandekvoten.

## <a name="view-the-coffee-machine-in-the-dashboard"></a>Visa kaffebryggaren på instrumentpanelen
Navigera till den **instrumentpanelen** sidan där du sammantaget kan se den relevanta informationen om din kaffe-dator. För följande övning använder aktivera **designläget** att konfigurera din instrumentpanel. När du är klar kan du välja **spara**.

1. Välj **linjediagram** och ange rubriken som telemetri att se telemetri mätning av faktisk användning. Välj **De senaste 30 minuter** om du vill se ett **tidsintervall**.

    ![Visa instrumentpanelen](../images/4-dashboard-a.png)

1. Välj **Inställningar och egenskaper** och ange rubriken Enhetsegenskaper. Välj Maxtemperatur kaffebryggare, Minimitemperatur kaffebryggare, Enhetsgaranti utgången i **Lägg till/ta bort**. 

1. Välj **Inställningar och egenskaper** och ange rubriken Optimal temperatur. Välj Optimal temerpatur i **Lägg till/ta bort**. 

## <a name="summary"></a>Sammanfattning

I den här enheten du ägnat tid åt att verifiera anslutningen mellan kaffe datorn och Azure IoT Central. Du nått verifieringen genom att uppdatera den optimala temperaturen köra kommandon. Avslutningsvis konfigurerar du instrumentpanelen för övervakning av bryggaren på en plats genom att definiera vilken information om kaffebryggaren som du vill ska visas. De här stegen för validering krävs innan du fortsätter till andra aktiviteter i nästa heltal. 
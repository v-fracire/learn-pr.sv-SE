Du har anslutit en kaffebryggare till programmet Azure IoT Central, vilket aktiverar utbyte av data som gör att du kan övervaka och hantera kaffebryggaren. I den här modulen skapar du regler som utlöser åtgärder när vattentemperaturen i kaffebryggaren ligger utanför det normala området. Åtgärderna är antingen e-postmeddelanden eller mobilmeddelanden beroende på om kaffebryggaren har en gällande garanti. Om du vill lägga till Microsoft Flow som en åtgärd behöver du en Azure-prenumeration. Om du inte har en Azure-prenumeration är det valfritt att lägga till Microsoft Flow som en åtgärd.

## <a name="create-rules-in-iot-central-with-email-as-the-action"></a>Skapa regler i IoT Central med e-postmeddelanden som åtgärd
Azure IoT Central har en inbyggd funktion för att skicka meddelanden. I det här scenariot gäller följande: om kaffebryggaren ligger utanför temperaturintervallet och inte har en gällande garanti, skickas ett e-postmeddelande från IoT Central till kundens underhållsavdelning.

Lägg till följande två regler när kaffebryggarens garanti har upphört att gälla och vattentemperaturen ligger utanför det optimala intervallet. När du är klar väljer du **Spara**. 

> [!NOTE]
> När villkoren tillämpas måste alla instruktioner vara sanna för att reglerna ska köras. Om villkoret är en ”eller”-instruktion som i det här scenariot (t.ex. den optimala temperaturen är lägre än 90 grader Celsius eller högre än 98 grader Celsius, med en garanti som upphört) ska instruktionen delas i två regler, som visas här.

1. Namnge regeln: kaffebryggarens vatten är för kallt (garantin har upphört)

    Lägg till villkor:      
    * Enhetens garanti har upphört är lika med 1
    * Vattentemperaturen är lägre än kaffebryggarens lägsta temperatur ![med hjälp av regel](../images/5-flow-g.png)

1. Namnge regeln: kaffebryggarens vatten är för varmt (garantin har upphört)

    Lägg till villkor:      
    * Enhetens garanti har upphört är lika med 1
    * Vattentemperaturen är högre än kaffebryggarens högsta temperatur ![med hjälp av regel](../images/5-flow-k.png)     

1. För att lägga till en **Åtgärd** rullar du ned på panelen Konfigurera telemetriregel och väljer +-tecknet bredvid Åtgärder. Välj sedan **E-post**

1. Lägg till den e-postadress som du använde för att logga in i programmet IoT Central för att definiera åtgärden. Lägg till aviseringsmeddelandet när vattentemperaturen är för varm: ”Kaffebryggarens vatten är för varmt. Underhåll krävs.  Garanti har upphört att gälla.” Upprepa samma steg för situationen där vattentemperaturen är för kall. Lägg till meddelandet: ”Kaffebryggarens vatten är för kallt. Underhåll krävs.  Garanti har upphört att gälla.”

1. Välj **Spara**. Regeln finns på sidan Regler:

1. För att utlösa regeln väntar du tills temperaturen hamnar utanför intervallet eller så kan du med avsikt ange en temperatur utanför det optimala intervallet i inställningarna. När du får ett e-postmeddelande ska du komma ihåg att inaktivera regeln för att undvika att överbelasta din inkorg med e-postmeddelanden från Azure IoT Central. 

> [!NOTE]
> Inaktivera reglerna när du är klar med verifieringen för att undvika att överbelasta inkorgen med e-postmeddelanden. 

## <a name="create-rules-in-iot-central-with-microsoft-flow-as-the-action"></a>Skapa regler i IoT Central med Microsoft Flow som åtgärd

Microsoft Flow automatiserar arbetsflöden för många program. Det är en av de åtgärder som kan utlösas när en regel utlöses i IoT Central. I det här scenariot skickar Microsoft Flow ett mobilmeddelande till en lokal tekniker när kaffebryggaren når ett visst tröskelvärde för temperaturen och garantin gäller. Gå till **Regler** och konfigurera villkor och lägg till ett flöde som en åtgärd när regeln utlöses. 
 
> [!NOTE]
> Den här övningen är valfri om du inte har en Azure-prenumeration och kan aktivera Microsoft Flow.

Lägg till följande regler när kaffebryggaren har en gällande garanti. 

1. Namnge regeln: kaffebryggarens vatten är för kallt (garanti)

    Lägg till villkor:      
    * Enhetens garanti har upphört är lika med 0
    * Vattentemperaturen är lägre än kaffebryggarens lägsta temperatur ![med hjälp av regel](../images/5-flow-l.png)

1. Namnge regeln: kaffebryggarens vatten är för varmt (garanti)

    Lägg till villkor:      
    * Enhetens garanti har upphört är lika med 0
    * Vattentemperaturen är högre än kaffebryggarens högsta temperatur ![med hjälp av regel](../images/5-flow-h.png)

1. När du har sparat regelvillkoren kan du välja Microsoft Flow som en ny åtgärd när kaffebryggaren har en gällande garanti. En ny flik eller ett fönster bör öppnas i din webbläsare, som tar dig till Microsoft Flow. Du hamnar på en översiktssida som visar en IoT Central-anslutningsapp som ansluter till en anpassad åtgärd. Välj **Fortsätt**. 

    Du kommer till Microsoft Flow-designverktyget och kan skapa arbetsflödet. Arbetsflödet har en IoT Central-utlösare som redan har programmet och regeln ifyllda.

    Nu kan du lägga till alla åtgärder du vill i arbetsflödet. Som ett exempel kan vi skicka ett mobilmeddelande. Sök efter meddelandet och välj Meddelanden – Send me a mobile notification (Skicka ett mobilmeddelande).

    I åtgärden fyller du i fältet Text med vad du vill att meddelandet ska innehålla. Du kan inkludera dynamiskt innehåll från IoT Central-regeln och skicka med viktig information, till exempel enhets-ID och namn.
    ![Använda Microsoft Flow som en åtgärd](../images/5-flow-f.png)

1. När du har konfigurerat arbetsflödet i Microsoft Flow laddar du ned [Flow-appen](https://www.microsoft.com/en-us/p/microsoft-flow/9nkn0p5l9n84?activetab=pivot%3aoverviewtab) från Microsoft Store till din mobila enhet. Logga in med samma konto som du använde för att konfigurera flödet i Flow-webbappen. I testsyfte ställer du in den optimala temperaturen utanför intervallet för att utlösa regeln. 

    > [!NOTE]
    > Enhetsegenskap: Enhetens garanti har upphört (1 för upphört eller 0 för gällande garanti i enhetskoden) genereras slumpmässigt av enheten och skickas sedan av enheten till programmet Azure IoT Central. I testsyfte kanske du vill styra värdet för garantins upphörande (1 eller 0). Starta då om simulatorn tills du får önskat tillstånd för garantin för att utlösa den åtgärd som du testar. För att få ett mobilmeddelande ska du starta om simulatorn tills du ser att enhetens garanti har upphört är lika med 0 i konsolloggen. 

    Efter ett par minuter visas meddelandet i Flow-mobilappen.

    ![Använda Microsoft Flow som en åtgärd](../images/5-flow-a.png)

## <a name="summary"></a>Sammanfattning
Du har lärt dig att skapa regler i IoT Central och utlöst åtgärder som exempelvis ett e-postmeddelande eller ett mobilmeddelande via Microsoft Flow när regeln utlöses. I det här fallet när kaffebryggarens vattentemperatur ligger utanför det optimala intervallet, skickas meddelanden till antingen en reparatör eller kunden beroende på tillståndet för garantin. 



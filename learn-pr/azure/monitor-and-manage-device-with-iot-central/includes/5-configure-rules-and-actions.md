Du har anslutit en kaffebryggare till programmet Azure IoT Central, vilket aktiverar utbyte av data som gör att du kan övervaka och hantera kaffebryggaren. I den här enheten, kan du skapa regler som utlösa åtgärder när water kaffe-datorn är utanför det normala intervallet. Åtgärderna är antingen e-postmeddelanden eller mobilmeddelanden beroende på om kaffebryggaren har en gällande garanti. Om du vill lägga till Microsoft Flow som en åtgärd behöver du en Azure-prenumeration. Om du inte har en Azure-prenumeration är det valfritt att lägga till Microsoft Flow som en åtgärd.

## <a name="create-rules-in-iot-central-with-email-as-the-action"></a>Skapa regler i IoT Central med e-postmeddelanden som åtgärd
Azure IoT Central har en inbyggd funktion för att skicka meddelanden. I det här scenariot gäller följande: om kaffebryggaren ligger utanför temperaturintervallet och inte har en gällande garanti, skickas ett e-postmeddelande från IoT Central till kundens underhållsavdelning.

Navigera till den **regler** för övningarna i den här enheten. Välj **+ ny regel**, sedan **telemetri**. Lägg till följande två regler när kaffebryggarens garanti har upphört att gälla och vattentemperaturen ligger utanför det optimala intervallet. När du är klar väljer du **Spara**. 

> [!NOTE]
> När villkoren tillämpas måste alla instruktioner vara sanna för att reglerna ska köras. Om villkoret är ett ”eller” uttryck som i upp det här scenariot (t.ex. optimala temperaturen är mindre eller större än de fördefinierade värdena när garantin har gått ut), instruktionen i två regler som visas här.

1. Namnge regeln: kaffebryggarens vatten är för kallt (garantin har upphört)

    Lägg till villkor:      
    * Enhetens garanti har upphört är lika med 1
    * Water temperaturen är mindre än kaffe skaparna Min temperatur

    ![Med hjälp av regel](../images/5-flow-a.png)

1. Namnge regeln: kaffebryggarens vatten är för varmt (garantin har upphört)

    Lägg till villkor:      
    * Enhetens garanti har upphört är lika med 1
    * Water temperaturen är större än kaffe skaparna Max temperaturen

1. Att lägga till en **åtgärd**, rulla nedåt i panelen konfigurera telemetri regel och väljer **+** bredvid åtgärder, Välj **e-post**.

1. Lägg till den e-postadress som du använde för att logga in i programmet IoT Central för att definiera åtgärden. Lägg till aviseringsmeddelandet när vattentemperaturen är för varm: ”Kaffebryggarens vatten är för varmt. Underhåll krävs.  Garanti har upphört att gälla.” Upprepa samma steg för situationen där vattentemperaturen är för kall. Lägg till meddelandet: ”Kaffebryggarens vatten är för kallt. Underhåll krävs.  Garanti har upphört att gälla.”

1. Välj **Spara**. Regeln visas på sidan regler.

1. Utlös regeln genom att ställa in optimala temperaturen i inställningarna för utanför det intervall som du angav under Egenskaper. Inaktivera regler för att undvika att överbelasta din inkorg med e-postmeddelanden när du är klar med verifieringen. 

## <a name="create-rules-in-iot-central-with-microsoft-flow-as-the-action"></a>Skapa regler i IoT Central med Microsoft Flow som åtgärd

Microsoft Flow automatiserar arbetsflöden för många program. Det är en av de åtgärder som kan utlösas när en regel utlöses i IoT Central. I det här scenariot skickar Microsoft Flow ett mobilmeddelande till en lokal tekniker när kaffebryggaren når ett visst tröskelvärde för temperaturen och garantin gäller. Gå till **Regler** och konfigurera villkor och lägg till ett flöde som en åtgärd när regeln utlöses. 
 
> [!NOTE]
> Den här övningen är valfri om du inte har en Azure-prenumeration och kan aktivera Microsoft Flow.


### <a name="extend-your-iot-central-trial-to-30-days"></a>Förläng din utvärdering med IoT Central och 30 dagar

1. Om du vill aktivera Microsoft Flow, måste du utöka din utvärdering med 30 dagar. Om du vill göra det, Välj **utöka utvärderingsversionen till 30 dagar** på sidan fakturering väljer du en Azure Active Directory och Azure-prenumeration. Med en Azure-prenumeration kan du skapa instanser av Azure-tjänster. Azure IoT Central hittar automatiskt alla Azure-prenumerationer som du har åtkomst till och visar dem i listrutan.
    
1. Om du inte har en Azure-prenumeration, kan du skapa ett en på [Registreringssidan för Azure](https://aka.ms/createazuresubscription). När du skapar Azure-prenumeration kan du gå tillbaka till den **Programhanterare** sidan. Din nya prenumeration visas i listrutan **Azure-prenumeration**.
        

### <a name="add-the-following-rules-when-the-coffee-machine-is-under-warranty"></a>Lägg till följande regler när kaffebryggaren har en gällande garanti. 

1. Namnge regeln: kaffebryggarens vatten är för kallt (garanti)

    Lägg till villkor:      
    * Enhetens garanti har upphört är lika med 0
    * Water temperaturen är mindre än kaffe skaparna Min temperatur

1. Namnge regeln: kaffebryggarens vatten är för varmt (garanti)

    Lägg till villkor:      
    * Enhetens garanti har upphört är lika med 0
    * Water temperaturen är större än kaffe skaparna Max temperaturen

1. När du har sparat regelvillkoren kan du välja Microsoft Flow som en ny åtgärd när kaffebryggaren har en gällande garanti. En ny flik eller ett fönster bör öppnas i din webbläsare, som tar dig till Microsoft Flow. Du hamnar på en översiktssida som visar en IoT Central-anslutningsapp som ansluter till en anpassad åtgärd. Välj **Fortsätt**. 

    Du kommer till Microsoft Flow-designverktyget och kan skapa arbetsflödet. Arbetsflödet har en IoT Central-utlösare som redan har programmet och regeln ifyllda.

    Nu kan du lägga till alla åtgärder du vill i arbetsflödet. Som ett exempel kan vi skicka ett mobilmeddelande. Sök efter meddelandet och välj Meddelanden – Send me a mobile notification (Skicka ett mobilmeddelande).

    I åtgärden fyller du i fältet Text med vad du vill att meddelandet ska innehålla. Du kan inkludera dynamiskt innehåll från IoT Central-regeln och skicka med viktig information, till exempel enhets-ID och namn.
    
    ![Använda Microsoft Flow som en åtgärd](../images/5-flow-b.png)

1. När du har konfigurerat arbetsflödet i Microsoft Flow laddar du ned [Flow-appen](https://www.microsoft.com/en-us/p/microsoft-flow/9nkn0p5l9n84?activetab=pivot%3aoverviewtab) från Microsoft Store till din mobila enhet. Logga in med samma konto som du använde för att konfigurera flödet i Flow-webbappen. Ställa in optimala temperaturen utanför intervallet för att utlösa regeln i testsyfte. 

    > [!NOTE]
    > Enhetsegenskap: Enhetens garanti har upphört (1 för upphört eller 0 för gällande garanti i enhetskoden) genereras slumpmässigt av enheten och skickas sedan av enheten till programmet Azure IoT Central. För testning, om du vill att styra enheten garanti upphört att gälla (1 eller 0), starta om datorn kaffe tills du får tillståndet avsedda garanti för att utlösa den åtgärd som du testar. För att få ett mobilmeddelande, startar du om datorn kaffe tills du ser att enheten garantin inte längre är 0 i konsolloggen för. 

    Efter ett par minuter visas meddelanden i Flow-mobilappen.

    ![Använda Microsoft Flow som en åtgärd](../images/5-flow-c.png)

## <a name="summary"></a>Sammanfattning
Du har lärt dig att skapa regler i IoT Central och utlöst åtgärder som exempelvis ett e-postmeddelande eller ett mobilmeddelande via Microsoft Flow när regeln utlöses. I det här fallet när water kaffe-datorn är utanför det optimala intervallet, skickas meddelanden till en reparation tekniker eller klienten beroende på status för garantin. 



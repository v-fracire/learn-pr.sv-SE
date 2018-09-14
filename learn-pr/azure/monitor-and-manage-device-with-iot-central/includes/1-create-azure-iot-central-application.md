 I den här självstudien kan du följa scenariot där en fjärransluten kaffe dator är ansluten till Azure IoT Central för övervakning och hantering av problem. Du kan övervaka telemetri, till exempel water temperatur och fuktighet, undersöka statusen på din dator, ställa in optimala temperatur, ta emot garantistatus och skicka kommandon. När vattentemperaturen i kaffebryggaren överskrider ett viss tröskelvärde medan garantin är giltig skickar Microsoft Flow ett mobilmeddelande till en fjärrteknikers mobil. På samma sätt, om garantin har gått ut när vattentemperaturen är utanför det förväntade intervallet skickas ett e-postmeddelande från IoT-centralen till kundens underhållsavdelning.

Om du vill implementera scenariot kan börja du genom att skapa en mall för enheten i Azure IoT Central definiera mått (telemetri och tillstånd), inställningar, egenskaper och kommandon. Du kan sedan ansluta kaffebryggaren till Azure IoT Central, följt av att konfigurera regler för underhållsmeddelanden när vattentemperaturen är utanför det optimala området.

I den här modulen kommer att:
- Skapa en anpassad App för Azure IoT Central 
- Skapa och definiera din enhetsmall
- Anslut datorn kaffe till programmet
- Verifiera din anslutning och ditt dataflöde
- Konfigurera regler för meddelanden om underhåll
 
## <a name="sign-in-to-azure-iot-central"></a>Logga in på Azure IoT Central
I den här enheten logga du in till IoT Central att skapa en ny anpassad App. En 7 dagars utvärderingsversion är tillräcklig för att slutföra enheter 1 – 4. Om du vill i valfritt övningen om hur du använder Microsoft Flow för att skicka ett SMS i enhet 5 måste du utöka IoT Central-utvärdering och 30 dagar. Tillägget är aktiverad om du har en Azure-prenumeration.  

1. Gå till sidan [Application Manager](https://aka.ms/iotcentral) (Programhanterare) i Azure IoT Central. 

1. På inloggningssidan, anger du e-postadress och lösenord som användes för att få åtkomst till ditt Microsoft-konto.

## <a name="create-a-new-custom-application"></a>Skapa ett nytt anpassat program

1. Om du vill skapa ett nytt program för Azure IoT Central, Välj **nytt program**. 

1. På sidan Skapa program: 
    * Välj **kostnadsfri** för betalningsplanen
    * Välj **anpassade program** som mall för program
    * Välj ett eget programnamn, till exempel **kaffe Maker-01**
    * Azure IoT Central genererar ett unikt URL-prefix du Välj **skapa**
    
   > [!NOTE]
   > Utöka din utvärdering med 30 dagar är valfritt, men det är ett krav om du vill slutföra den här övningen på Skicka ett SMS enhet 5 med Microsoft Flow. 30 dagars förlängning är aktiverad om du har en Azure-prenumeration. Anvisningar om hur du aktiverar tillägget finns i enhet 5 om hur du konfigurerar regler och åtgärder för att övervaka datorn kaffe.

I den här enheten skapade du ett anpassat program i Azure IoT. Du kan även ha valt att registrera dig för en Azure-prenumeration. I nästa enhet fortsätter du att bygga vidare på application framework som du skapade. 
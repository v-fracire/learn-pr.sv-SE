 I den här självstudien får du följa ett scenario där en fjärransluten kaffebryggare ansluts till Azure IoT Central för övervakning och hantering av problem. Du kan övervaka telemetri, till exempel vattentemperatur och fuktighet, observera status för maskinen, ställa in optimal temperatur, se garantistatus och skicka kommandon. Om vattentemperaturen i kaffebryggaren överskrider ett visst tröskelvärde och garantin är giltig, skickar Microsoft Flow en mobilavisering till en fjärrteknikers mobil. Om garantin har gått ut när vattentemperaturen är utanför det förväntade intervallet, skickas ett e-postmeddelande från IoT Central till kundens underhållsavdelning.

Om du vill implementera scenariot kan du börja med att skapa en mall för enheten i Azure IoT Central, där du definierar mått (telemetri och status), inställningar, egenskaper och kommandon. Du kan sedan ansluta kaffebryggaren till Azure IoT Central, följt av att konfigurera regler för underhållsmeddelanden när vattentemperaturen är utanför det optimala området.

I den här modulen kommer du att:
- Skapa ett anpassat Azure IoT Central-program 
- Skapa och definiera din enhetsmall
- Ansluta din kaffebryggare till programmet
- Kontrollera din anslutning och ditt dataflöde
- Konfigurera regler för underhållsmeddelanden
 
## <a name="sign-in-to-azure-iot-central"></a>Logga in på Azure IoT Central
I den här kursdelen loggar du in på IoT Central för att skapa ett nytt anpassat program. En utvärderingsversion på 7 dagar räcker för att slutföra kursdelarna 1–4. Om du vill slutföra den valfria övningen där du använder Microsoft Flow till att skicka en mobilavisering i kursdel 5, måste du utöka utvärderingen av IoT Central till 30 dagar. Tillägget är aktiverat om du har en Azure-prenumeration.  

1. Gå till sidan [Application Manager](https://aka.ms/iotcentral) (Programhanterare) i Azure IoT Central. 

1. På inloggningssidan anger du den e-postadress och det lösenord som du använder för att få åtkomst till ditt Microsoft-konto.

## <a name="create-a-new-custom-application"></a>Skapa ett nytt anpassat program

1. Börja skapa ett nytt Azure IoT Central-program genom att välja **Nytt program**. 

1. På sidan Skapa program: 
    * Välj **Kostnadsfri** som betalningsplan
    * Välj programmallen **Anpassat program** som programmall
    * Välj ett eget programnamn, exempelvis **Kaffebryggare 01**
    * Azure IoT Central skapar ett unikt URL-prefix åt dig. Välj **Skapa**
    
   > [!NOTE]
   > Du kan välja att utöka din prenumeration med 30 dagar om du vill, men det är ett krav om du vill slutföra övningen om att använda Microsoft Flow till att skicka mobilaviseringar i kursdel 5. 30-dagarstillägget är aktiverat om du har en Azure-prenumeration. Anvisningar om hur du aktiverar tillägget finns i kursdel 5 om hur du konfigurerar regler och åtgärder för att övervaka kaffebryggaren.

I den här kursdelen har du skapat ett anpassat Azure IoT-program. Du kan även ha valt att registrera dig för en Azure-prenumeration. I nästa kursdel ska vi fortsätta att utveckla det programramverk som du skapade. 
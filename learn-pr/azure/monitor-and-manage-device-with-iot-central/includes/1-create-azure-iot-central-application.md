Azure IoT Central är en fullständigt hanterad Sakernas Internet-lösning (ioT) som gör det enkelt att ansluta, övervaka och hantera dina globala IoT-tillgångar.

Här får du följa ett scenario där en fjärransluten kaffemaskin ansluts till Azure IoT Central för övervakning och hantering av problem. Du kan övervaka telemetri, till exempel vattentemperatur och fuktighet, observera status för maskinen, ställa in optimal temperatur, se garantistatus och skicka kommandon. Om garantin har gått ut när vattentemperaturen är utanför det förväntade intervallet skickas ett e-postmeddelande från IoT Central till kundens underhållsavdelning för ytterligare åtgärder.

Du börjar genom att skapa en enhet i Azure IoT Central som definierar vilka data och kommandon som kan utbytas med IoT-enheter.

I den här modulen kommer du att göra följande:
  - Skapa ett anpassat Azure IoT Central-program
  - Skapa och definiera din enhetsmall
  - Anslut en kaffemaskinssimulator till ditt program i Azure IoT Central
  - Kontrollera din anslutning och ditt dataflöde
  - Konfigurera regler för underhållsmeddelanden
 
## <a name="sign-in-to-azure-iot-central"></a>Logga in på Azure IoT Central
I den här enheten loggar du in på IoT Central för att skapa ett nytt anpassat program. En utvärderingsversion på 7 dagar räcker för att slutföra kursdelarna 1–4. 

1. Gå till sidan [Application Manager](https://aka.ms/iotcentral?azure-portal=true) (Programhanterare) i Azure IoT Central. 

1. På inloggningssidan anger du den e-postadress och det lösenord som du använder för att få åtkomst till ditt Microsoft-konto.

## <a name="create-a-new-custom-application"></a>Skapa ett nytt anpassat program

1. Börja skapa ett nytt Azure IoT Central-program genom att välja **Nytt program**. 

1. På sidan **Skapa program**: 
    * Välj **Kostnadsfri** som betalningsplan
    * Välj programmallen **Anpassat program** som programmall
    * Välj ett eget programnamn, exempelvis **Kaffebryggare 01-A**
    * (Valfritt) Redigera URL:en  – det här måste utföras om det angivna namnet redan används
    * Välj **Skapa**

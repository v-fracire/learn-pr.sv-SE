 I den här självstudien får du följa ett scenario där en fjärransluten kaffebryggare ansluts till Azure IoT Central för övervakning och hantering av problem. Du kan övervaka telemetri, till exempel vattentemperatur och fuktighet, tillståndet för din dator, ställa in optimal temperatur, ta emot garantistatus och skicka kommandon, till exempel starta bryggning. När vattentemperaturen i kaffebryggaren överskrider ett viss tröskelvärde medan garantin är giltig skickar Microsoft Flow ett mobilmeddelande till en fjärrteknikers mobil. På samma sätt, om garantin har gått ut när vattentemperaturen är utanför det förväntade intervallet skickas ett e-postmeddelande från IoT-centralen till kundens underhållsavdelning.

Om du vill implementera scenariot kan du börja med att skapa en mall för enheten i Azure IoT Central, en SaaS-lösning, för att definiera mått (telemetri och tillstånd), inställningar, egenskaper och kommandon. Du kan sedan ansluta kaffebryggaren till Azure IoT Central, följt av att konfigurera regler för underhållsmeddelanden när vattentemperaturen är utanför det optimala området.

![Dataflödet från kaffebryggaren till IoT Central](../images/1-data-flow.png)

I den här självstudien får du lära dig hur man:
> [!div class="checklist"]
> * Skapa en anpassad mall för ett Azure IoT Central-program
> * Skapa och definiera din enhetsmall
> * Anslut ditt program till kaffebryggarsimulatorn
> * Verifiera din anslutning och ditt dataflöde
> * Konfigurera regler för meddelanden om underhåll
 
## <a name="sign-in-to-azure-iot-central"></a>Logga in på Azure IoT Central

Azure IoT Central är en fullständigt hanterad SaaS-lösning (programvara som en tjänst) som gör det enkelt att ansluta, övervaka och hantera IoT-tillgångar i skala. I den här modulen loggar du in på IoT Central för att skapa ett nytt anpassat program. Du kan välja att utöka utvärderingsversionen till 30 dagar när du registrerar dig för ett Azure-konto. En 30-dagars utvärderingsversion är en förutsättning för att slutföra modul 5 där du använder Microsoft Flow för att skicka en mobilavisering.

1. Gå till sidan [Application Manager](https://aka.ms/iotcentral) (Programhanterare) i Azure IoT Central. 

1. Ange den e-postadress och det lösenord som du använder för att få åtkomst till ditt Microsoft-konto.
![Logga in på ditt Microsoft-konto](../images/1-create-app-a.png)

## <a name="create-a-new-custom-application"></a>Skapa ett nytt anpassat program

1. Börja skapa ett nytt Azure IoT Central-program genom att välja **Nytt program**. 
![Skapa ett Central-program](../images/1-create-app-b.png)

1. Skapa ett nytt Azure IoT Central-program:
* Välj **Ledigt** som betalningsplan.
* Välj programmallen **Custom Application** som programmall.
* Välj ett eget programnamn, exempelvis **Kaffebryggare 01**. 
* Du får ett unikt URL-prefix från Azure IoT Central. Välj **Skapa**.

![Välj en plan för ditt Central-program](../images/1-create-app-c.png)

* Det är valfritt att utöka din prenumeration med 30 dagar men det är ett förhandskrav om du vill slutföra modulen om att integrera Microsoft Flow för att utlösa en åtgärd för att skicka mobilaviseringar. Välj en Azure Active Directory och en Azure-prenumeration som ska användas för att utöka din prenumeration till 30 dagar. Med en Azure-prenumeration kan du skapa instanser av Azure-tjänster. Azure IoT Central hittar automatiskt alla Azure-prenumerationer som du har åtkomst till och visar dem i listrutan.
        
![Förläng din utvärderingsversion](../images/1-create-app-d.png)
    
* Om du inte har en Azure-prenumeration, kan du skapa ett en på [Registreringssidan för Azure](https://aka.ms/createazuresubscription). När du har skapat en Azure-prenumeration kan du gå tillbaka till sidan **Skapa program**. Din nya prenumeration visas i listrutan **Azure-prenumeration**.
        
![Registrera dig för en prenumeration](../images/1-create-app-e.png)

## <a name="summary"></a>Sammanfattning

I den här modulen har du skapat ett anpassat Azure IoT-program. Du kan även ha valt att registrera dig för en Azure-prenumeration. I nästa modul ska vi fortsätta att utveckla det programramverk som du skapade. 
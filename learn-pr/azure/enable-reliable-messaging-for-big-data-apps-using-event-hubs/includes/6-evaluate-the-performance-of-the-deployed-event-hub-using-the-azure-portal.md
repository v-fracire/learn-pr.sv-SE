När du använder Event Hubs är det viktigt att du övervakar din hubb för att säkerställa att den fungerar och presterar som förväntat.

Som en fortsättning på bankexemplet: låt oss anta att du har distribuerat Azure Event Hubs och konfigurerat avsändar- och mottagarprogrammen. Dina program är redo att testa lösningen för betalningsbearbetning. Avsändarprogrammet samlar in data om kundens kreditkort och mottagarprogrammet kontrollerar att det registrerade kreditkortet är giltigt. Eftersom din arbetsgivares organisation är känslig är det viktigt att betalningsbearbetningen är robust och tillförlitlig, även om den inte är tillgänglig för tillfället.

Du måste utvärdera din händelsehubb genom att testa att den bearbetar data som förväntat. Med de mått som är tillgängliga i Event Hubs kan du kontrollera att den fungerar som den ska.

## <a name="how-do-you-use-the-azure-portal-to-view-your-event-hub-activity"></a>Hur kan du använda Azure-portalen för att visa aktiviteter på din händelsehubb?

Azure-portalen > sidan Översikt för din händelsehubb visar antalet meddelanden. Detta värde representerar data (händelser) som tas emot och skickas av händelsehubben. Du kan välja tidsram för att visa dessa händelser.

![Skärmbild av Azure-portalen som visar ett Event Hub-namnområde med antal meddelanden.](../media/6-view-messages.png)

## <a name="how-can-you-test-event-hub-resilience"></a>Hur kan man testa händelsehubbens återhämtningsförmåga?

Azure Event Hubs tar emot meddelanden från avsändarprogrammet även om den inte är tillgänglig. De meddelanden som tagits emot under denna period överförs så fort hubben blir tillgänglig.

Du kan använda Azure-portalen för att inaktivera din händelsehubb för att testa den här funktionen.

När du återaktiverar händelsehubben kör du mottagarprogrammet och använder måtten i händelsehubben för din namnrymd för att kontrollera att alla meddelanden som avsändaren har skickat har överförts och tagits emot.

Andra användbara mått som finns tillgängliga i händelsehubben är:

- Arbetsbelastningsstyrning av begäranden: antalet begäranden som har begränsats på grund av att användningen av dataflödesenheten överskreds.
- ActiveConnections: antalet aktiva anslutningar på en namnrymd eller händelsehubb.
- Inkommande/utgående byte: antalet byte som skickats till/tagits emot från händelsehubben under en angiven period.

## <a name="summary"></a>Sammanfattning

Azure-portalen visar antalet meddelanden och andra mått som du kan använda som en hälsokontroll för händelsehubben.

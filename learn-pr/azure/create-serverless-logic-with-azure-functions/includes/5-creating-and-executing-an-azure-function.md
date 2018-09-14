Nu när vi har skapat en funktionsapp ska vi visa hur du skapar, konfigurerar och kör en Azure-funktion.

### <a name="triggers"></a>Utlösare

Funktioner är aktivitetsdrivna, vilket innebär att de körs som svar på en händelse.

Den typ av händelse som initierar funktionen kallas för *utlösare*. Du konfigurerar en funktion med exakt en utlösare.

 Azure har stöd för utlösare för följande tjänster.


|Tjänst  |Beskrivning av utlösare  |
|---------|---------|
|Blob Storage     |  Starta en funktion när en ny eller uppdaterad blob identifieras.       |
|Cosmos DB     |  Starta en funktion när infogningar och uppdateringar identifieras.      |
|Event Grid     |   Starta en funktion när en händelse tas emot från Event Grid.       |
|HTTP     |   Starta en funktion med en HTTP-begäran.      |
|Microsoft Graph-händelser     |  Starta en funktion som svar på en inkommande webhook från Microsoft Graph. Varje instans av den här utlösaren kan reagera på en resurstyp för Microsoft Graph.       |
|Queue Storage     |    Starta en funktion när ett nytt objekt tas emot i en kö. Kömeddelandet anges som indata till funktionen.      |
|Service Bus     |  Starta en funktion som svar på meddelanden från en Service Bus-kö.       |
|Timer     |  Starta en funktion efter ett schema.       |
|Webhooks     |  Starta en funktion med en webhook-begäran.       |

### <a name="bindings"></a>Bindningar

Bindningar för Azure Functions är ett deklarativt sätt att koppla data och tjänster till din funktion. Du behöver inte skriva kod i din funktion för att ansluta till datakällor och hantera anslutningar. Plattformen tar hand om den komplexiteten åt dig. En bindning har en riktning. Din kod läser data från *indatabindningar* och skriver data till *utdatabindningar*. Låt oss titta på ett exempel.

Om du vill läsa data från Blob Storage konfigurerar du en indatabindning av typen *blob*. Om du vill skriva ett meddelande till Queue Storage konfigurerar du en utdatabindning av typen *kö*.

Mer information om vilka bindningar och utlösare som stöds finns i [dokumentationen om Azure](https://docs.microsoft.com/azure/azure-functions/functions-triggers-bindings#supported-bindings).

### <a name="a-sample-trigger-and-binding-functionjson"></a>Exempel på utlösare och bindning (function.json)

Följande JSON är en exempeldefinition av en utlösare och en bindning för en funktion.

```javascript
{
  "bindings": [
    {
      "name": "order",
      "type": "queueTrigger",
      "direction": "in",
      "queueName": "myqueue-items",
      "connection": "MY_STORAGE_ACCT_APP_SETTING"
    },
    {
      "name": "$return",
      "type": "table",
      "direction": "out",
      "tableName": "outTable",
      "connection": "MY_TABLE_STORAGE_ACCT_APP_SETTING"
    }
  ]
}
```

Som vi nämnde tidigare har alltså varje bindning en riktning som definierar om det är en indata- eller utdatabindning. Utlösare är alltid indatabindningar. I exemplet ovan visas en funktion som utlöses av ett meddelande som läggs till i en kö som heter **myqueue-items**. Den skickar sedan funktionens returvärde till tabellen **outTable** i Azure Table Storage.

## <a name="creating-a-function-in-the-azure-portal"></a>Skapa en funktion i Azure Portal

Azure tillhandahåller förkonfigurerade mallar för vanliga Azure-funktionsscenarier.

### <a name="quickstart-templates"></a>Snabbstartsmallar

Om du vill lägga till en Azure-funktion måste du välja en funktionsapp. Funktionsappen kan identifieras av blixten inom vinkelparenteser.  
![Skärmbild som visar en funktionsapp som markerats i en resursgrupp.](../images/5-function-icon.png)

När du lägger till din första Azure-funktion visas skärmen Snabbstart. På den här skärmen kan du välja utlösartyp och programmeringsspråk. Baserat på dina val genererar Azure därefter funktionens kod och konfiguration. 
 
![Snabbstart för funktionen](../images/5-quickstart-form.png)

### <a name="function-templates"></a>Funktionsmallar

Utbudet av mallar är inte begränsat till de mallar som visas i snabbstarten. Du har också möjlighet att välja bland 30 olika mallar. Du kan få åtkomst till skärmen med mallistan när du skapar efterföljande funktioner eller genom att välja alternativet **Anpassad funktion** på snabbstartskärmen.  
![Skärmbild av användargränssnittet i Snabbstart i Azure Functions, som hjälper dig att komma igång snabbt med en fördefinierad funktion.](../images/5-template-list.png)

## <a name="navigating-to-your-function-and-files"></a>Gå till dina funktioner och filer

När du skapar en funktion från en mall skapas flera filer. Om du till exempel har valt att använda Webhook + API-snabbstart med JavaScript, skulle det skapas en konfigurationsfil, **function.json**, och en källkodsfil, **index.js**. De funktioner som du skapar i en funktionsapp visas under menyalternativet Funktioner i funktionsapportalen.

När du väljer en funktion i din funktionsapp öppnas en kodredigerare som visar koden för din funktion, som illustreras på följande skärmbild.

![Skärmbild som visar gränssnittet för val av funktionsmall, som visar en uppsättning mallar som du kan använda för att snabba upp utvecklingen av funktionen.](../images/5-file-navigation.png)

Som du ser i föregående skärmbild finns det en utfällbar meny till höger som innehåller en flik för att **visa filer**. Om du väljer den här fliken visas filstrukturen som utgör din funktion.  

## <a name="execution-options-for-testing-your-azure-function"></a>Körningsalternativ för att testa Azure-funktionen

När du har skapat en funktion vill du nog testa den. Det finns ett par olika metoder: manuell körning och testning från själva Azure-portalen.

### <a name="manual-execution-of-a-function"></a>Manuell körning av en funktion

Du kan starta en funktion genom att manuellt utlösa den konfigurerade utlösaren. Om du till exempel använder en HttpTrigger – du kan använda ett verktyg som Postman eller cURL för att initiera en HTTP-begäran till funktionsslutpunktens URL.  

![Del av skärmbild av gränssnittet Postman med en funktions-URL inskriven i url-fältet url och begärandetexten ifylld med en exempeltext. ](../images/5-postman-execution.png)

Du hämtar slutpunkts-URL genom att välja HTTP-utlösaren i det vänstra navigeringsfönstret och sedan välja knappen för att **hämta funktions-URL**.  

![Skärmbild av portalen Funktionsappar med en funktion markerad och åtgärden *Get function URL* vald högst upp på sidan.](../images/5-get-function-url.png)

### <a name="testing-a-function-in-the-azure-portal"></a>Testa en funktion i Azure Portal

Portalen erbjuder även ett bekvämt sätt att testa dina funktioner. På höger sida av kodfönstret finns en utfällbar navigeringsmeny med flikar. Menyn innehåller ett **testobjekt**. Om du expanderar menyn och väljer den här fliken får du ett annat sätt att köra din funktion och visa resultatet. I linje med HttpTrigger-scenariot kan du ange HTTP-metoden och lägga till parametrar för frågesträng och HTTP-huvuden för begäran. Du kan också ändra begärandetexten för att testa fler scenarier. I följande exempel har testet konfigurerats som en HTTP POST-begäran med en begärandetext som har en parameter med namnet *name*.
 
![Skärmbild av testfönstret i portalen Funktionsappar som visar en HTTP POST-begäran och en exempelbegärandetexten. Utdatafönstret innehåller resultatet av ett funktionsanrop med orden ”Hello Azure”.](../images/5-portal-execution.png)

När du klickar på **Kör** i det här testfönstret visas resultatet i utdatafönstret, tillsammans med en statuskod. 

## <a name="monitoring-an-azure-function"></a>Övervaka en Azure-funktion

Möjligheten att logga meddelanden och övervaka dina funktioner är viktig vid utveckling och i produktion. Azure-portalen tillhandahåller en instrumentpanel för övervakning och ett sätt att granska körningsloggar och undantag som hämtats från dina Azure-funktioner.

### <a name="log-window"></a>Loggfönster

Du kan också lägga till logginstruktioner för din funktion. Instruktionerna visas i funktionens loggfönster. Loggfönstret finns på en utfällbar meny med flikar längst ned i kodfönstret. Följande JavaScript-kodstycke visar hur du loggar ett meddelande med hjälp av metoden `context.log`.

```javascript
  context.log('Enter your logging statement here');
```  

![Skärmbild av kodredigeraren i användargränssnittet för Azure Functions. En rad med kod som använder metoden context.log()för att logga ett meddelande i loggfönstret som också är markerat.](../images/5-log-window.png)

### <a name="errors-and-warnings-window"></a>Fönstret Fel och varningar

Du kan hitta fliken med fönstret för fel och varningar på samma utfällbara meny som den för loggfönstret. Det här fönstret visar kompileringsfel och varningar i din kod. Eftersom JavaScript är ett dynamiskt och tolkat språk visar följande bild ett kompilationsfel i en C#-funktion.  

![Skärmbild av kodredigeraren i användargränssnittet för Azure Functions med fönstret **Loggar** markerat längst ned på skärmen.](../images/5-errors-window.png)

### <a name="monitoring-dashboard"></a>Instrumentpanel för övervakning

I funktionsappens navigeringsmeny ser du ett **skärmmenyalternativ** när du har expanderat funktionens nod. Via instrumentpanelen för övervakning får du ett snabbt sätt att visa historiken för funktionskörningarna. I den här vyn ser du tidsstämpel, resultatkod, längd och åtgärds-ID samt om den har slutförts eller inte. Data fylls i via Azure Application Insights.  

![Skärmbild av övervakningsinstrumentpanelen som startas via funktionens menyalternativ **Övervaka** som visar en lista över lyckade och misslyckade anrop till funktionen.](../images/5-monitor-function.png)

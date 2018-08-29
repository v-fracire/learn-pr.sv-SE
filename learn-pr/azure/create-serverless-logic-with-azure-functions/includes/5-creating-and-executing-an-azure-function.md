Nu när vi har skapat en funktionsapp ska vi visa hur du skapar, konfigurerar och kör en Azure-funktion.

### <a name="triggers"></a>Utlösare
Azure-funktioner är aktivitetsdrivna och körs som svar på en händelse, till exempel för att ta emot en HTTP-begäran eller när ett meddelande läggs till i en kö. 

Typen av händelse som initierar funktionen kallas för utlösare. Azure-funktioner kräver att minst en utlösare konfigureras. Annars kan vi inte köra funktionen.

 Azure stöder en mängd olika utlösare, däribland:
* HTTPTrigger
* TimerTrigger
* GitHub-webhook
* CosmosDBTrigger
* BlobTrigger
* QueueTrigger
* EventHubTrigger
* ServiceBusQueueTrigger
* ServiceBusTopicTrigger

### <a name="bindings"></a>Bindningar
Bindningar för Azure-funktioner är ett deklarativt sätt att ansluta till data till din funktion programmässigt.

Bindningar är anslutningen till dina datatjänster. Med en Azure-funktion med noll, en eller flera bindningar kan du få åtkomst till flera datakällor. Du definierar dem även som indata- och utdatabindningar, eller så kan du tänka på dem som läs- eller skrivbindningar.

Tänk dig att du har en QueueTrigger som initierar en funktion när en blob läggs till i en kö. En indatablob-bindning skulle användas för att ansluta till Azure Blob Storage. 

Utdatabindningar används för att lagra data. De skickar exempelvis returvärdet för funktionen till Azure Table Storage.

En fullständig lista över tillgängliga bindningar finns i [Azure-dokumentationen](https://docs.microsoft.com/azure/azure-functions/functions-triggers-bindings#supported-bindings).

### <a name="a-sample-trigger-and-binding-functionjson"></a>Exempel på utlösare och bindning (function.json)
Det här är en exempeldefinition av en utlösare och en bindning för en funktion. Du kommer att märka att beroende på vilken typ av bindning du beskriver finns det olika egenskapsvärden som måste anges. Varje bindning har också en riktning som visar om det är en indata- eller utdatabindning. Utlösare är alltid indatabindningar. I det här exemplet visas en funktion som utlöses av ett meddelande som läggs till i en kö som heter **myqueue-items**. Den skickar sedan funktionens returvärde till tabellen **outTable** i Azure Table Storage.

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
## <a name="premade-functions-and-templates"></a>Fördefinierade funktioner och mallar
Azure tillhandahåller förkonfigurerade mallar för vanliga Azure-funktionsscenarier.

### <a name="quickstart-templates"></a>Snabbstartsmallar
Om du vill lägga till en Azure-funktion måste du välja en funktionsapp. Funktionsappen kan identifieras av blixten inom vinkelparenteser.  
![Ikon för funktion](../images/5-function-icon.png)

När du lägger till din första Azure-funktion visas skärmen Snabbstart. På den här skärmen kan du göra att val utifrån önskad utlösartyp och programmeringsspråk. Baserat på dina val genererar Azure därefter funktionens kod och konfiguration.  
![Snabbstart för funktionen](../images/5-quickstart-form.png)

### <a name="function-templates"></a>Funktionsmallar
Utbudet av mallar är inte begränsat till dem som är tillgängliga i snabbstarten. Du har också möjlighet att välja bland 30 olika mallar. Du kan få åtkomst till skärmen med mallistan när du skapar efterföljande funktioner eller genom att välja alternativet **Anpassad funktion** på snabbstartskärmen.  
![Funktionsmallar](../images/5-template-list.png)

## <a name="navigating-to-your-function-and-files"></a>Gå till dina funktioner och filer
När du har skapat en funktion från en mall skapas flera filer. Vi utgår ifrån att du har valt att använda Webhook + snabbstarten för API med JavaScript. Filerna som genereras blir en konfigurationsfil, **function.json**, och en källkodfil, **index.js**. När du öppnar funktionsappen ser du en trädmeny som visar funktionerna som har skapats i appen. På det här **Functions**-menyalternativet kan du även lägga till ytterligare funktioner till funktionsappen (en plusikon visas när du håller musen över menyalternativet).  
![Knappen Lägg till funktion](../images/5-function-add-button.png) 

När det gäller den här snabbstarten som nämns ovan och när du expanderar **Functions** i trädvyn ser du en funktion med standardnamnet **HttpTriggerJS1** (som även anges av funktionens f-ikon). När du väljer den är funktionen öppnas ett kodfönster som visar källfilen **index.js**. På höger sida av kodfönstret finns det en utfällbar meny som innehåller en flik för att **visa fler**. Om du väljer den här fliken ser du filstrukturen (efterliknas i Storage) som utgör funktionen.  
![Funktionsmallar](../images/5-file-navigation.png)

## <a name="execution-options-for-testing-your-azure-function"></a>Körningsalternativ för att testa Azure-funktionen
När du har skapat en Azure-funktion måste du veta hur du testar den. Det finns ett par olika metoder: manuell körning och testning från själva Azure-portalen.

### <a name="manual-execution-of-a-function"></a>Manuell körning av en funktion
Du kan starta körningen av en funktion genom att manuellt utlösa den konfigurerade utlösaren. Om du till exempel använder en HttpTrigger – du kan använda ett verktyg som Postman eller cURL för att initiera en HTTP-begäran till funktionsslutpunktens URL.  
![Postman-körning av en funktion](../images/5-postman-execution.png) Du kan hämta slutpunktens URL genom att välja din HTTP-utlösare från vänster navigering och sedan välja knappen **Hämta funktionswebbadress**.  
![Hämta funktionswebbadress](../images/5-get-function-url.png)

### <a name="testing-a-function-in-the-azure-portal"></a>Testa en funktion i Azure Portal
Azure-portalen kan även erbjuda ett bekvämt sätt att testa dina funktioner. På höger sida av kodfönstret finns en utfällbar navigeringsmeny med flikar. Menyn innehåller ett **testobjekt**. Om du expanderar menyn och väljer den här fliken får du ett annat sätt att köra din funktion och visa resultatet. I linje med HttpTrigger-scenariot kan du ange HTTP-metoden och lägga till parametrar för frågesträng och HTTP-huvuden för begäran. Du kan också ändra begärandetexten för att testa fler scenarier. I utdatafönstret visas resultatet från funktionen.  
![Portal-körning av en funktion](../images/5-portal-execution.png)

## <a name="monitoring-an-azure-function"></a>Övervaka en Azure-funktion
Under utvecklingen av en Azure-funktion är det avgörande att kunna logga meddelanden och avgöra undantagsscenarier för att du garanterat ska få dina funktioner produktionsklara. Det är precis lika viktigt i ett produktionsscenario när du spårar källan till en defekt. I Azure-portalen får du ett användargränssnitt som tillhandahåller en instrumentpanel för övervakning och ett sätt att granska körningsloggar och undantag som hämtats från dina Azure-funktioner.

### <a name="monitoring-dashboard"></a>Instrumentpanel för övervakning
I funktionsappens navigeringsmeny ser du ett **skärmmenyalternativ** när du har expanderat funktionens nod. Via instrumentpanelen för övervakning får du ett snabbt sätt att visa historiken för funktionskörningarna. I den här vyn ser du tidsstämpel, resultatkod, längd och åtgärds-ID samt om den har slutförts eller inte. Data fylls i via Azure Application Insights.  
![Funktionsövervakning](../images/5-monitor-function.png)

### <a name="log-window"></a>Loggfönster
Du kan också logga instruktioner för din funktion. Instruktionerna visas i funktionens loggfönster. Loggfönstret finns på en utfällbar meny med flikar längst ned i kodfönstret. När du använder en JavaScript-baserad funktion kan du lägga till en logginstruktion med följande syntax:
```javascript
  context.log('Enter your logging statement here');
```  
![Loggfönster](../images/5-log-window.png)

### <a name="errors-and-warnings-window"></a>Fönstret Fel och varningar
Du kan hitta fliken med fönstret för fel och varningar på samma utfällbara meny som den för loggfönstret. Det här fönstret visar kompileringsfel och varningar i din kod. Eftersom JavaScript är ett dynamiskt och tolkat språk visar följande bild ett kompilationsfel i en C#-funktion.  
![Fönstret Fel och varningar](../images/5-errors-window.png)

Du måste ofta uppdatera flera dokument i dina databaser samtidigt. 

När en användare gör en beställning i ditt onlinebutiksprogram och vill använda en kupongkod, kredit eller en återbäring (eller alla tre samtidigt) behöver du fråga på användarens konto efter de alternativen, göra uppdateringar av kontot som visar att de har använts, uppdatera beställningens summa och behandla beställningen.

Alla dessa åtgärder måste ske samtidigt, inom en enda transaktion. Om användaren väljer att annullera beställningen så måste du återställa ändringarna och inte ändra kontoinformationen – så att kupongkoder, krediter och återbäringar är tillgängliga för nästa köp.

Sättet att utföra dessa transaktioner på i Azure Cosmos DB är genom att använda lagrade procedurer och användardefinierade funktioner (UDF:er). Lagrade procedurer är det enda sättet att säkerställa ACID-transaktioner (atomicitet, konsekvens, isolering, varaktighet) eftersom de körs på servern och därför refereras till som programmering på serversidan. UDF:er lagras också på servern och används vid frågor för att utföra databaserad logik på värden eller dokument i frågan. 

I den här modulen får du lära dig om lagrade procedurer och UDF:er och får sedan köra några i portalen.

## <a name="stored-procedure-basics"></a>Grunderna om lagrade procedurer

Lagrade procedurer utför komplexa transaktioner på dokument och egenskaper. Lagrade procedurer skrivs i JavaScript och lagras i en samling i Azure Cosmos DB. Genom att utföra de lagrade procedurerna på databasmotorn och nära data kan du förbättra prestanda över klientprogrammering.

Lagrade procedurer är det enda sättet att åstadkomma atomiska transaktioner i Azure Cosmos DB, SDK:er på klientsidan stöder inte transaktioner.

Att utföra batchåtgärder i lagrade procedurer rekommenderas också på grund av det minskade behovet att skapa separata transaktioner.

<!--TODO: Ideally I'd like to list some cases where a stored procedure is not the best option.-->

## <a name="stored-procedure-example"></a>Exempel på lagrad procedur

Följande exempel är en enkel lagrad HelloWorld-procedur som hämtar den aktuella kontexten och skickar ett svar som visar ”Hello, World”. Observera att den lagrade proceduren har ett ID-värde, precis som Azure Cosmos DB-dokument.

```java
var helloWorldStoredProc = {
    id: "helloWorld",
    serverScript: function () {
        var context = getContext();
        var response = context.getResponse();

        response.setBody("Hello, World");
    }
}
```

## <a name="user-defined-function-basics"></a>Grunderna om användardefinierade funktioner

UDF:er används till att utöka Azure Cosmos DB SQL-frågegrammatiken och implementera anpassad affärslogik som beräkningar i egenskaper och dokument. UDF:er kan bara anropas inifrån frågor och till skillnad från lagrade procedurer så har de inte åtkomst till kontextobjektet, så de kan inte läsa eller skriva dokument.

I ett scenario med onlinehandel kan en UDF användas för att fastställa om momsen ska tillämpas på beställningens totalsumma eller om en procentuell rabatt ska tillämpas på produkter eller beställningar.

## <a name="user-defined-function-example"></a>Exempel på en användardefinierad funktion

I följande exempel skapas en UDF för att beräkna rabatter baserat på en beställnings totalsumma och sedan returneras den ändrade beställningssumman baserat på rabatten:

```java
var discountUdf = {
    id: "discount",
    serverScript: function discount(orderTotal) {

        if(orderTotal == undefined) 
            throw 'no input';

        if (orderTotal < 50) 
            return orderTotal * 0.9;
        else if (orderTotal < 100) 
            return orderTotal * 0.8;
        else
            return orderTotal * 0.7;
    }
}
```

## <a name="create-a-stored-procedure-in-the-portal"></a>Skapa en lagrad procedur i portalen

Nu ska vi skapa en ny lagrad procedur i portalen. Portalen fyller automatiskt i en enkel lagrad procedur som hämtar det första objektet i samlingen, så vi kör den här lagrade proceduren först.

1. I Datautforskaren klickar du på **Ny lagrad procedur**.

    Datautforskaren visar en ny flik med ett exempel på lagrad procedur.

  <!--TODO: Insert animated .gif of creating the stored procedure.-->

2. I rutan **ID för lagrad procedur** anger du namnet *sample*, klickar på **Spara** och klickar sedan på **Kör**.


3. I rutan **Indataparametrar** skriver du namnet på partitionsnyckeln, *33218896* och klickar sedan på **Kör**. Observera att lagrade procedurer fungerar i en enda partition.

    ![Kör en lagrad procedur i portalen](../media-draft/6-stored-procedure.gif)

    **Resultat**fönstret visar flödet från det första dokumentet i samlingen.

## <a name="create-a-stored-procedure-that-creates-documents"></a>Skapa en lagrad procedur som skapar dokument

Nu ska vi skapa en lagrad procedur som skapar dokument.

1. I Datautforskaren klickar du på **Ny lagrad procedur**. Ge den här lagrade proceduren namnet *createDocuments*, klicka på **Spara** och klicka sedan på **Kör**.

    ```java
    var createDocumentStoredProc = {
        id: "createMyDocument",
        productid: "5"
        serverScript: function createMyDocument(documentToCreate) {
            var context = getContext();
            var collection = context.getCollection();
    
            var accepted = collection.createDocument(collection.getSelfLink(),
                  documentToCreate,
                  function (err, documentCreated) {
                      if (err) throw new Error('Error' + err.message);
                      context.getResponse().setBody(documentCreated.id)
                  });
            if (!accepted) return;
        }
    }
    ```

<!--TODO: Need to fix code above.-->

2. Ange partitionsnyckelvärdet *3* och klicka sedan på **Kör**.

    Datautforskaren visar det nya dokumentet. 

## <a name="create-a-user-defined-function"></a>Skapa en användardefinierad funktion

Nu ska vi skapa en UDF i Datautforskaren.

Klicka på **Ny UDF** i Datautforskaren. Kopiera följande kod i fönstret, namnet UDF:en *tax* och klicka sedan på **Spara**. Du kan inte köra UDF:en från portalen men vi använder den i en senare modul.

```java
function userDefinedFunction(){
    var taxUdf = {
        id: "tax",
        serverScript: function tax(income) {

            if(income == undefined) 
                throw 'no input';

            if (income < 1000) 
                return income * 0.1;
            else if (income < 10000) 
                return income * 0.2;
            else
                return income * 0.4;
        }
    }
}
```


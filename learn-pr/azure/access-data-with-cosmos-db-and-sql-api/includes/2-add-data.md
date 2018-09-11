Att lägga till data i din Azure Cosmos DB-databas ska vara enkelt. Öppna Azure Portal, navigera till databasen och använd **Datautforskaren** till att lägga till json-dokument i databasen. Det finns mer avancerade sätt att lägga till data, men vi börjar här eftersom Datautforskaren är ett bra verktyg för att bekanta sig med de egenskaper och funktioner som tillhandahålls av Azure Cosmos DB.

## <a name="what-is-the-data-explorer"></a>Vad är Datautforskaren?
Datautforskaren i Azure Cosmos DB är en verktyg som ingår i Azure Portal som används för att hantera data som lagras i en Azure Cosmos DB. Den har ett användargränssnitt för att visa och navigera bland datasamlingar samt redigera dokument i databasen.

## <a name="add-data-using-the-data-explorer"></a>Lägga till data med Datautforskaren

1. Om du fortsätter från föregående modul klicka du på **Datautforskaren** I Azure Portal-fönstret och klicka sedan på **Öppna i helskärmsläge**.

    Logga annars in på [Azure Portal](https://portal.azure.com/), klicka på **Alla tjänster** > **Databaser** > **Azure Cosmos DB**. Välj sedan ditt konto, klicka på **Datautforskaren** och klicka sedan på **Öppna i helskärmsläge**
 
   ![Skapa nya dokument i Datautforskaren i Azure Portal](../media-draft/2-add-data/azure-cosmosdb-data-explorer-full-screen.png)

2. I rutan **Öppna i helskärmsläge** klickar du på **Öppna**.

    Webbläsaren visar den nya Datautforskaren i helskärm, vilket ger dig mer utrymme och en dedikerad miljö för att arbeta med databasen.

3. Om du vill skapa ett nytt json-dokument klickar du på **Nytt dokument**.

   ![Skapa nya dokument i Datautforskaren i Azure Portal](../media-draft/2-add-data/azure-cosmosdb-data-explorer-new-document.png)

4. Lägg nu till ett dokument i samlingen med följande struktur, kopiera bara och klistra in följande kod på fliken **Dokument**.

     ```
    {
        "id": "1",
        "productId": "33218896",
        "category": "Women's Clothing",
        "manufacturer": "Contoso Sport",
        "description": "Quick dry crew neck t-shirt",
        "shipping": {
            "weight": 1,
            "dimensions": {
            "width": 6,
            "height": 8,
            "depth": 1
           }
        }
     ```

5. När du har lagt till json på fliken **Dokument** klickar du på **Spara**.

    ![Kopiera in json-data och klicka på Spara i Datautforskaren i Azure Portal](../media-draft/2-add-data/azure-cosmosdb-data-explorer-save-document.png)

6. Skapa och spara ett dokument till genom att kopiera följande json-objekt i Datautforskaren och klicka på **Spara**.

     ```
    {
        "id": "2",
        "productId": "33218897",
        "category": "Women's Outerwear",
        "manufacturer": "Contoso",
        "description": "Black wool pea-coat",
        "shipping": {
            "weight": 2,
            "dimensions": {
            "width": 8,
            "height": 11,
            "depth": 3
           }
        }
    }
     ```

7. Bekräfta att dokumenten har sparats genom att klicka på **Dokument** i den vänstra menyn. 

    Datautforskaren de två dokumenten på fliken **Dokument**.

## <a name="summary"></a>Sammanfattning

I den här modulen har du lagt till två dokument, som var och en representerar en produkt i din produktkatalog, i databasen med hjälp av Datautforskaren. Datautforskaren är ett bra sätt att skapa dokument, ändra dokument och komma igång med Azure Cosmos DB.  

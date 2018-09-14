Lägga till data i Azure Cosmos DB är database enkel. Öppna Azure Portal, navigera till databasen och använd Datautforskaren till att lägga till JSON-dokument i databasen. Det finns mer avancerade sätt att lägga till data, men vi börjar här eftersom Datautforskaren är ett bra verktyg för att bekanta sig med de egenskaper och funktioner som tillhandahålls av Azure Cosmos DB.

## <a name="what-is-the-data-explorer"></a>Vad är Datautforskaren?
Datautforskaren i Azure Cosmos DB är ett verktyg som ingår i Azure Portal som används för att hantera data som lagras i en Azure Cosmos DB. Det innehåller ett användargränssnitt för att visa och navigera datasamlingar, samt för att redigera dokument i databasen, fråga efter data, och skapa och köra lagrade procedurer.

## <a name="add-data-using-the-data-explorer"></a>Lägga till data med Datautforskaren

1. Logga in på den [Azure-portalen](https://portal.azure.com/?azure-portal=true), klickar du på **alla tjänster** > **databaser** > **Azure Cosmos DB**. Välj ditt konto, klicka på **Datautforskaren**, och klicka sedan på **öppna helskärm**.
 
   ![Skapa nya dokument i Datautforskaren i Azure Portal](../media/3-azure-cosmosdb-data-explorer-full-screen.png)

2. I rutan **Öppna i helskärmsläge** klickar du på **Öppna**.

    Webbläsaren visar den nya Datautforskaren i helskärm, vilket ger dig mer utrymme och en dedikerad miljö för att arbeta med databasen.

3. Om du vill skapa ett nytt JSON-dokument i SQL API-rutan, expanderar den **produkter** > **kläder**, klickar du på **dokument**, klicka sedan på **nytt dokument** .

   ![Skapa nya dokument i Datautforskaren i Azure Portal](../media/3-azure-cosmosdb-data-explorer-new-document.png)

4. Lägg nu till ett dokument i samlingen med följande struktur. Kopiera och klistra in följande kod på fliken **Dokument**:

     ```
    {
        "id": "1",
        "productId": "33218896",
        "category": "Women's Clothing",
        "manufacturer": "Contoso Sport",
        "description": "Quick dry crew neck t-shirt",
        "price": "14.99",
        "shipping": {
            "weight": 1,
            "dimensions": {
            "width": 6,
            "height": 8,
            "depth": 1
           }
        }
    }
     ```

5. När du har lagt till JSON på fliken **Dokument** klickar du på **Spara**.

    ![Kopiera in JSON-data och klicka på Spara i Datautforskaren i Azure Portal](../media/3-azure-cosmosdb-data-explorer-save-document.png)

6. Skapa och spara en mer dokumentet att klicka på **nytt dokument** igen, och kopiera följande JSON-objekt till Data Explorer och klicka på **spara**.

     ```
    {
        "id": "2",
        "productId": "33218897",
        "category": "Women's Outerwear",
        "manufacturer": "Contoso",
        "description": "Black wool pea-coat",
        "price": "49.99",
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

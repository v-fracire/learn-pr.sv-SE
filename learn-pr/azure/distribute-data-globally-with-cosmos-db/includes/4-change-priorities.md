När du har replikerat dina data i flera regioner kan du dra nytta av de lösningar för automatisk redundans som Azure Cosmos DB tillhandahåller. Automatisk redundans är en funktion som är aktuell vid ett haveri eller andra händelser gör att en av dina läs- eller skrivregioner går offline. Det omdirigerar begäranden från den regionen som är offline till den näst mest prioriterade regionen. 

För din webbaserade klädbutik, som du just replikerade till USA, östra, USA, västra och Storbritannien, västra kan du prioritera att om USA, östra går offline så kan du omdirigera läsningar till USA, västra i stället för Storbritannien, västra för att begränsa svarstiden. 

I den här enheten lär du dig om hur redundans fungerar och anger prioritet för de regioner där ditt företags data har replikerats.

## <a name="failover-basics"></a>Grunderna för redundans

I det sällsynta fall då det förekommer avbrott i en Azure-region eller ett datacenter utlöser Azure Cosmos DB automatiskt redundans för alla AZure Cosmos DB-konton med närvaro i den berörda regionen.

**Vad händer om en läsregion har ett avbrott?**

Azure Cosmos DB-konton med en läsregion i någon av de berörda regionerna kopplas automatiskt bort från deras skrivregion och markeras som offline. Cosmos DB SDK implementerar ett protokoll för regional identifiering som gör att de automatiskt kan identifiera när en region är tillgänglig och omdirigera anrop till nästa tillgängliga region i listan över föredragna regioner. Om ingen av regionerna i listan över föredragna regioner är tillgänglig kan återgår anrop automatiskt till den aktuella skrivregionen. Det krävs inga ändringar i din programkod för att hantera regionala redundanser. Under hela processen fortsätter konsekvensgarantier att tillämpas av Azure Cosmos DB.

När den berörda regionen återställs från avbrottet så återställs alla berörda Azure Cosmos DB-konton i regionen automatiskt av tjänsten. Azure Cosmos DB-konton som hade en läsregion i den berörda regionen synkroniseras sedan automatiskt med den aktuella skrivregionen och går online. Azure Cosmos DB SDK:er identifierar tillgängligheten för den nya regionen och utvärderar huruvida regionen ska väljas som aktuell läsregion baserat på listan över föredragna regioner som konfigurerats av programmet. Efterföljande läsningar omdirigeras till den återställda regionen utan att det krävs några ändringar i din programkod.

**Vad händer om en skrivregion har ett avbrott?**

Om den berörda regionen är den aktuella skrivregionen och automatisk redundans är aktiverat för Azure Cosmos DB-kontot markeras regionen automatiskt som offline. Sedan höjs en alternativ region upp som skrivregionen för det berörda Azure Cosmos DB-kontot.

Under automatisk redundans väljer Azure Cosmos DB automatiskt nästa skrivregion för ett visst Azure Cosmos DB-konto baserat på den angivna prioritetsordningen. Program kan använda egenskapen WriteEndpoint för klassen DocumentClient för att identifiera ändringen i skrivregionen.

När den berörda regionen återställs från avbrottet så återställs alla berörda Cosmos DB-konton i regionen automatiskt av tjänsten.

Nu ändrar vi läsregionen för din databas.

## <a name="set-read-region-priorities"></a>Ange prioriteringar för läsregion

1. I Azure-portalen går du till skärmen **Replikera data globalt** och klickar på **Automatisk redundans**. Automatisk redundans är endast aktiverat om databasen redan har replikerats till flera regioner.
2. På skärmen **Automatisk redundans** växlar du **Aktivera automatisk redundans** till **PÅ**.
3. I avsnittet **Läsregioner** klickar du på den vänstra delen av raden **USA, östra** och drar sedan och släpper på den översta platsen.
4. Klicka på den vänstra delen av raden **Japan, östra** och dra och släpp till den andra positionen.
5. Klicka på **OK**.

    ![Ändra läsregioner i portalen](../media/4-change-priorities/change-read-priorities.gif)

## <a name="summary"></a>Sammanfattning

I den här enheten har du lärt dig vad automatisk redundans gör, hur du kan använda det för att skydda mot oförutsedda avbrott och hur du ändrar prioriteringarna för läsregion.
### <a name="exercise-4-test-the-model"></a>Övning 4: Testa modellen

I [övning 5](../5-build-app.yml) ska du skapa en Node.js-app som använder modellen till att känna igen konstnären bakom ett verk. Du behöver inte skriva någon ny app för att testa modellen. Du kan göra dina tester i portalen och förfina modellen med hjälp av testbilderna. I den här övningen testar du modellens förmåga att känna igen konstnären bakom verket med utgångspunkt från testbilderna.

1. Klicka på **Quick Test** (Snabbtest) högst upp på sidan.
 
    ![Testa modellen](../images/portal-click-quick-test.png)

    _Testa modellen_ 

1. Klicka på **Bläddra bland lokala filer** och bläddra till mappen Quick Tests (Snabbtest) bland labbresurserna. Välj **PicassoTest_01.jpg** och klicka på **Öppna**.

    ![Välja en testbild av Picasso](../images/portal-select-test-01.png)

    _Välja en testbild av Picasso_ 

1. Granska testresultatet i dialogrutan Quick Test (Snabbtest). Vad är sannolikheten för att målningen är en Picasso? Vad är sannolikheten att den målats av Rembrandt eller Pollock?

1. Stäng dialogrutan Quick Test (Snabbtest). Klicka sedan på **Predictions** (Förutsägelser) överst på sidan.
 
    ![Visa tester som utförts](../images/portal-select-predictions.png)

    _Visa tester som utförts_ 

1. Klicka på den testbild som du laddade upp för att visa detaljer i den. Tagga bilden som en ”Picasso” genom att välja **Picasso** i den nedrullningsbara listan och klicka på **Save and close** (Spara och stäng).

    > Genom att tagga bilder på det här sättet kan du förfina modellen utan att behöva ladda upp fler träningsbilder.
 
    ![Tagga testbilden](../images/tag-test-image.png)

    _Tagga testbilden_ 

1. Gör ännu ett snabbtest med filen som heter **FlowersTest.jpg** i mappen Quick Test (Snabbtest). Bekräfta att den här bilden har tilldelats en låg sannolikhet för att vara målad av Picasso, Rembrandt eller Pollock.

Modellen är nu tränad och beredd på att identifiera konstverk av utvalda konstnärer. Nu ska vi ta det hela ett steg längre och införliva modellens intelligens i en app.
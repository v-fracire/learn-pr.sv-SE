I den här enheten ska du använda appen Artworks för att skicka in bilder till Custom Vision Service och få dem klassificerade. Appen använder JSON-information som returneras från metoden [PredictImage](https://southcentralus.dev.cognitive.microsoft.com/docs/services/eb68250e4e954d9bae0c2650db79c653/operations/58acd3c1ef062f0344a42814) i Custom Visions Prediction-API för att avgöra om en bild föreställer en tavla målad av Picasso, Rembrandt, Pollock – eller ingen av dessa. Den visar också sannolikheten för att klassificeringen är korrekt.

1. Klicka på knappen **Browse (...)** (Bläddra (...)) i appen Artworks.

    ![Bläddra efter lokala bilder i appen Artworks](../media/6-app-click-browse.png)

1. Bläddra till mappen Quick Tests (Snabbtest) bland modulresurserna. Välj en fil som heter **PicassoTest_02.jpg** och klicka sedan på **Öppna**.

1. Klicka på knappen **Predict** (Förutsäg) för att skicka bilden till Custom Vision Service.

    ![Skicka bilden till Custom Vision Service](../media/6-app-click-predict.png)

1. Kontrollera att appen identifierar tavlan som en målning av Picasso.

    ![Klassificera en bild som en Picasso](../media/6-app-prediction-01.png)

1. Upprepa steg 1 till 3 för **RembrandtTest_01.jpg** och **PollockTest_01.jpg** och kontrollera att appen identifierar dessa som konstverk målade av Rembrandt och Pollock.

    ![Klassificera en bild som en Rembrandt](../media/6-app-prediction-02.png)

1. Upprepa steg 1 till 3 för **VanGoghTest_01.png** och **VanGoghTest_02.png** och kontrollera att appen inte identifierar dessa som konstverk målade av Picasso, Rembrandt eller Pollock.

    ![Inte Picasso, Rembrandt eller Pollock](../media/6-app-prediction-03.png)

1. Som du ser är tillförlitligheten lika stor när du använder Prediction-API:t i appen som den är när du går via Custom Vision Service-portalen. Men det är roligare att använda appen! Om du går till sidan Predictions (Förutsägelser) i portalen hittar du alla bilder som laddades upp i appen där med.

    ![Bild som skickats till Custom Vision Service](../media/6-portal-all-predictions.png)

Passa på att experimentera med egna bilder och testa modellens förmåga att identifiera olika konstnärer eller bara att avgöra att en bild inte målats av Picasso, Rembrandt eller Pollock. Om du vill att modellen även ska kunna känna igen målningar av Van Gogh laddar du upp några Van Gogh-bilder, taggar dem som ”Van Gogh” och tränar om modellen. Det finns ingen gräns för hur mycket intelligens du kan tillföra – bara du är villig och har tid till att utföra träningen. Kom ihåg att ju fler bilder du tränar modellen med, desto smartare blir den.
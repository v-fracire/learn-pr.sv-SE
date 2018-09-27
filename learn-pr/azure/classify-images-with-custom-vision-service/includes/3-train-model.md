I den här enheten kommer du träna modellen med hjälp av bilder som laddats upp och taggats i den föregående övningen. När du har tränat en modell kan du förfina den genom att ladda upp fler taggade bilder och göra om träningen.

1. Klicka på knappen **Train** (Träna) längst upp på sidan för att träna modellen. Varje gång du tränar modellen skapas en ny iteration. Custom Vision Service lagrar flera iterationer, så att du kan jämföra förloppet över tid.

    ![Träna modellen](../media/2-portal-click-train.png)

1. Vänta tills träningen har slutförts. (Det tar bara några sekunder.) Granska träningsstatistiken för iteration 1. 

    Resultatet visar två mått i en modells noggrannhet, **Precision** och **Recall** (Träffsäkerhet). Anta att tre Picasso-bilder och tre av Van Gogh har presenterats för modellen. Säg att modellen korrekt har identifierat två av Picasso-exemplen som Picasso-bilder men felaktigt identifierat två av Van Gogh-exemplen som Picasso. I det här fallet skulle värdet för **Precision** vara 50 %, eftersom två av fyra bilder har identifierats korrekt. Resultatet för **Recall** (Träffsäkerhet) skulle vara 67 % eftersom två av tre Picasso-bilder har identifierats korrekt.

    ![Träningsresultat](../media/2-portal-train-complete.png)

I nästa övning ska vi testa modellen med portalens snabbtestfunktion. Skicka in bilder till modellen och se hur den försöker klassificera dem utifrån de kunskaper som den tillägnat sig från träningsbilderna.

> [!TIP]
> Utöver att träna modellen med Custom Vision-portalens användargränssnitt kan du även anropa metoden [TrainProject](https://southcentralus.dev.cognitive.microsoft.com/docs/services/d9a10a4a5f8549599f1ecafc435119fa/operations/58d5835bc8cb231380095bed) i [Custom Vision Training API](https://southcentralus.dev.cognitive.microsoft.com/docs/services/d9a10a4a5f8549599f1ecafc435119fa/operations/58d5835bc8cb231380095be3).
I den här enheten kommer du träna modellen med hjälp av bilder som laddats upp och taggats i den föregående övningen. Du startar träningen genom att klicka på en knapp i portalen eller genom att anropa metoden [TrainProject](https://southcentralus.dev.cognitive.microsoft.com/docs/services/d9a10a4a5f8549599f1ecafc435119fa/operations/58d5835bc8cb231380095bed) i [Custom Vision Training API](https://southcentralus.dev.cognitive.microsoft.com/docs/services/d9a10a4a5f8549599f1ecafc435119fa/operations/58d5835bc8cb231380095be3). När du har tränat en modell kan du förfina den genom att ladda upp fler taggade bilder och göra om träningen.

1. Klicka på knappen **Train** (Träna) längst upp på sidan för att träna modellen. Varje gång du tränar modellen skapas en ny iteration. Custom Vision Service lagrar flera iterationer, så att du kan jämföra förloppet över tid.

    ![Träna modellen](../media/2-portal-click-train.png)

1. Vänta tills träningen har slutförts. (Det tar bara några sekunder.) Granska träningsstatistiken för iteration 1. **Precision** och **träffsäkerhet** är separata men närbesläktade mått på modellens tillförlitlighet. Föreställ dig att modellen ska titta på tre verk av Picasso och tre verk av Van Gogh. Den identifierar två verk av Picasso som målade av just Picasso, men tar sedan miste och hävdar att två verk av Van Gogh i själva verket gjordes av Picasso. I det här fallet är precisionen 50 % (två av de fyra bilder som klassificerades som målade av Picasso var faktiskt verk av Picasso) medan träffsäkerheten är 67 % (två av de tre bilderna av Picasso identifierades korrekt).

    ![Träningsresultat](../media/2-portal-train-complete.png)

Nu kan vi testa modellen med portalens snabbtestfunktion. Skicka in bilder till modellen och se hur den försöker klassificera dem utifrån de kunskaper som den tillägnat sig från träningsbilderna.
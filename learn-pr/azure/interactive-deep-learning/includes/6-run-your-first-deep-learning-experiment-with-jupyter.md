![PyTorch-logotyp](../media/5-image1.png) 

Vanligtvis brukar djupinlärningsingenjörerna inte implementera matrisalgebraoperationerna helt och hållet för hand. I stället använder de ramverk som PyTorch eller TensorFlow.  

PyTorch är ett Python-baserat ramverk som ger flexibilitet som utvecklingsplattform för djupinlärning. Det bygger på NumPy, biblioteket för vetenskaplig databehandling i Python. 

Nu undrar du kanske varför vi ska använda PyTorch för att skapa modeller för djupinlärning?  

- Lättanvänt API – om du kan Python kommer du igång snabbt.
- Stöd för Python – PyTorch integreras smidigt med stacken för vetenskaplig databehandling.
- Dynamiska beräkningsdiagram – i stället för fördefinierade diagram med specifika funktioner bygger PyTorch databaserade diagram dynamiskt, vilka kan ändras under körning. Dynamiska beräkningsdiagram är värdefulla för kapslad batchbearbetning och när vi inte vet hur mycket minne som krävs för att skapa ett visst nätverk.

Mer information om PyTorch finns i [den officiella dokumentationen på PyTorch.org](https://pytorch.org/about/).

## <a name="run-your-first-pytorch-model"></a>Köra din första PyTorch-modell

Nu när du har en Docker-container som etablerats från en PyTorch-avbildning är det dags att experimentera. Som du kanske kommer ihåg laddade vi ned en notebook från [python.org](https://python.org). Denna exempel-notebook vägleder dig genom att träna ett nätverk för att klassificera bilder i olika kategorier. Det definierar ett djupt CNN (Convolutional Neural Network).

1. Navigera i din lokala webbläsare till den Jupyter Notebook-server som du skapade i föregående övning. URL:en ska vara i formatet:

    `<HOSTNAME>.<REGION>.cloudapp.azure.com:8888/?token={sometoken}`

1. Välj notebook `first_pytorch_classifier.ipynb` i instrumentpanelen.

    ![välj the first_pytorch_classifier.ipynb](../media/5-image2.PNG)

    Följ anvisningarna i anteckningsboken för att träna din första PyTorch-klassificerare.

    ![skärmbild av ”Träna en notebook-klassificerare”](../media/5-image3.PNG)

2. Börja längst upp i notebook och kör varje cell i ordning. Observera följande:

    - Vissa av cellerna tar lång tid att köra. Notera den lilla punkten längst upp till höger i notebook intill orden ”Python 3”. När kerneln är upptagen med en åtgärd blir punkten en fylld, mörkare cirkel. Den fortsätter se ut så tills åtgärden har slutförts. 
    - Du tränar en CNN till att klassificera bilder. När nätverket har tränats kommer notebook att testa märkta bilder mot modellen. Den registrerar den förutsägelse som görs för varje bild och beräknar korrektheten i modellen. Du ser resultaten i följande format.

    ![Träningsresultat som visar korrektheten i modellen](../media/accuracy.png)
    
    - Du kan läsa mer om notebook i [dokumentationen om PyTorch-självstudier](https://pytorch.org/tutorials/beginner/blitz/cifar10_tutorial.html) online.
    
    - Vid slutet av notebook går anteckningarna igenom träning av en GPU. Om du har följt övningarna i den här modulen har du konfigurerat en CPU-baserad virtuell dator. Detta går bra för en modell av den här storleken, och du ser kanske inte några betydande förbättringar i träningstid med en GPU. Om du vill prova modulen med hjälp av en virtuell dator med GPU:er finns det två ändringar som du behöver göra:
    - Etablera DSVM på en GPU-aktiverad VM-storlek i N-serien.
    - Skapa en container med `nvidia-docker` i stället för `docker` i den föregående övningen.
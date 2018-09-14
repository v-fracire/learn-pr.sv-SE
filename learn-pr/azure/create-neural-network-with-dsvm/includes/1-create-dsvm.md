### <a name="create-an-ubuntu-data-science-vm"></a>Skapa en Ubuntu Data Science VM

Data Science Virtual Machine för Linux är en VM-avbildning som gör det lättare att komma igång med datavetenskap. Många verktyg har redan skapats, installerats och konfigurerats av oss för att du ska kunna komma igång så snabbt som möjligt. NVIDIA GPU-drivrutinen, [NVIDIA CUDA](https://developer.nvidia.com/cuda-downloads) och [NVIDIA CUDA Deep Neural Network](https://developer.nvidia.com/cudnn)-biblioteket (cuDNN) ingår, liksom [Jupyter](http://jupyter.org/), flera exempel på Jupyter Notebook och [TensorFlow](https://www.tensorflow.org/). Alla förinstallerade ramverk är GPU-kompatibla, men fungerar även bra tillsammans med vanliga processorer. I den här enheten ska du skapa en instans av Data Science Virtual Machine (DVSM) för Linux på Azure.

1. Öppna [Azure-portalen](https://portal.azure.com/?azure-portal=true) i webbläsaren.

1. Klicka på **Skapa en resurs** i menyn på vänster sida av portalen och skriv sedan ”data science” (utan citattecken) i sökfältet. Välj **Data Science Virtual Machine for Linux (Ubuntu)** i resultatlistan.

    ![Hitta en Ubuntu Data Science VM](../media-draft/1-new-data-science-vm.png)

1. Titta igenom listan med verktyg som medföljer den virtuella datorn. Klicka sedan på **Skapa** längst ned på bladet.

1. Fyll i bladet med grundinställningar enligt nedan. Ange ett lösenord som består av minst 12 tecken med en blandning av versaler, gemener, siffror och specialtecken. *Var noga med att komma ihåg det användarnamn och det lösenord som du anger, eftersom du behöver dem senare i modulen.*

    ![Ange grundläggande information om den virtuella datorn](../media-draft/1-create-data-science-vm-1.png)

1. På bladet Choose a size (Välj en storlek) väljer du **DS1_V2 Standard**, som ger dig möjlighet att experimentera med virtuella datorer för datavetenskap till en låg kostnad. Klicka på knappen **Välj** längst ned på bladet.

    ![Välja storlek på den virtuella datorn](../media-draft/1-create-data-science-vm-2.png)

1. I den **inställningar** bladet, acceptera standardinställningarna och klicka på den **OK** knappen.

1. Titta igenom de alternativ som du har valt för den virtuella datorn på bladet **Skapa** och klicka sedan på **Skapa** för att starta framställningen av virtuella datorer.

    ![Skapa den virtuella datorn](../media-draft/1-create-data-science-vm-4.png)

1. Klicka på **Resursgrupper** i menyn som visas till vänster i portalen. Klicka sedan på resursgruppen **data-science-rg**.

    ![Öppna resursgruppen](../media-draft/1-open-resource-group.png)

  
1. Vänta tills statusen ”Distribuerar” ändras till ”Lyckades”. Detta är en indikation på att DSVM-datorn och alla stödjande Azure-resurser har skapats. Distributionen tar högst fem minuter. Klicka med jämna mellanrum på **Uppdatera** överst på bladet för att uppdatera distributionens status.

    ![Övervaka distributionsstatus](../media-draft/1-deployment-succeeded.png)

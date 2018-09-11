### <a name="exercise-1-create-an-ubuntu-data-science-vm"></a>Övning 1: Skapa en Ubuntu Data Science VM

Data Science Virtual Machine för Linux är en VM-avbildning som gör det lättare att komma igång med datavetenskap. Många verktyg har redan skapats, installerats och konfigurerats av oss för att du ska kunna komma igång så snabbt som möjligt. NVIDIA GPU-drivrutinen, [NVIDIA CUDA](https://developer.nvidia.com/cuda-downloads) och [NVIDIA CUDA Deep Neural Network](https://developer.nvidia.com/cudnn)-biblioteket (cuDNN) ingår, liksom [Jupyter](http://jupyter.org/), flera exempel på Jupyter Notebook och [TensorFlow](https://www.tensorflow.org/). Alla förinstallerade ramverk är GPU-kompatibla, men fungerar även bra tillsammans med vanliga processorer. I den här övningen ska du skapa en instans av Data Science Virtual Machine för Linux på Azure.

1. Öppna [Azure-portalen](https://portal.azure.com) i webbläsaren. Om du ombeds logga in ska du göra det med autentiseringsuppgifterna för ditt Microsoft-konto.

1. Klicka på **+ Skapa en resurs** i menyn på vänster sida av portalen och skriv sedan ”data science” (utan citattecken) i sökfältet. Välj **Data Science Virtual Machine for Linux (Ubuntu)** i resultatlistan.

    ![Hitta en Ubuntu Data Science VM](../images/new-data-science-vm.png)

    _Hitta en Ubuntu Data Science VM_

1. Titta igenom listan med verktyg som medföljer den virtuella datorn. Klicka sedan på **Skapa** längst ned på bladet.

1. Fyll i bladet med grundinställningar enligt nedan. Ange ett lösenord som består av minst 12 tecken med en blandning av versaler, gemener, siffror och specialtecken. *Var noga med att komma ihåg det användarnamn och det lösenord som du anger, eftersom du behöver dem senare i labbuppgiften.*

    ![Ange grundläggande information om den virtuella datorn](../images/create-data-science-vm-1.png)

    _Ange grundinställningar_

1. På bladet Choose a size (Välj en storlek) väljer du **DS1_V2 Standard**, som ger dig möjlighet att experimentera med virtuella datorer för datavetenskap till en låg kostnad. Klicka på knappen **Välj** längst ned på bladet.

    ![Välja storlek på den virtuella datorn](../images/create-data-science-vm-2.png)

    _Välja storlek på den virtuella datorn_

1. På bladet Inställningar markerar du **SSH (22)** i listan över ingående portar så att klienterna kan ansluta till den virtuella datorn med hjälp av protokollet [Secure Shell](https://en.wikipedia.org/wiki/Secure_Shell) (SSH) på port 22. Klicka sedan på **OK**.

    ![Skapa den virtuella datorn](../images/create-data-science-vm-3.png)

    _Skapa den virtuella datorn_

1. Titta igenom de alternativ som du har valt för den virtuella datorn på bladet Skapa och sedan klicka på **Skapa** att starta framställningen av virtuella datorer.

    ![Skapa den virtuella datorn](../images/create-data-science-vm-4.png)

    _Skapa den virtuella datorn_

1. Klicka på **Resursgrupper** i menyn som visas till vänster i portalen. Klicka sedan på resursgruppen ”data-science-rg”.

    ![Öppna resursgruppen](../images/open-resource-group.png)

    _Öppna resursgruppen_

1. Vänta tills statusen ”Distribuerar” ändras till ”Lyckades”. Detta är en indikation på att DSVM-datorn och alla stödjande Azure-resurser har skapats. Distributionen tar högst 5 minuter. Med jämna mellanrum klickar du på **Uppdatera** överst på bladet för att uppdatera distributionens status.

    ![Övervaka distributionsstatus](../images/deployment-succeeded.png)

    _Övervaka distributionsstatus_

När distributionen är klar kan du gå vidare till nästa övning.
Nu när din Data Science Virtual Machine är igång beslutar du dig för att träna din första djupinlärningsmodell. Du vill köra dina experiment avskilt från allt annat på servern. För att göra det kör du dem i en Docker-container.

## <a name="connect-to-the-vm-with-ssh"></a>Ansluta till den virtuella datorn med SSH

Kontrollera att du är fortfarande ansluten till den virtuella datorn via SSH. Om inte så kör du det här kommandot igen för att fjärransluta tillbaka till den virtuella datorn.

1. Kör följande kommando i Azure Cloud Shell för att logga in på den virtuella datorn.

    ```azurecli 
    ssh <USERNAME>@<IP>
    ``` 
    
    Ersätt `<USERNAME>` med det administratörskontonamn som du definierade när du skapade den virtuella datorn. Ersätt `<IP>` med IP-adressen för den virtuella dator som du sparade i föregående övning.  

1. När du uppmanas anger du lösenordet för administratörskontot för att slutföra inloggningen.

## <a name="run-a-jupyter-notebook-server-in-a-docker-container-in-the-vm"></a>Köra en Jupyter Notebook-server i en Docker-container i den virtuella datorn

> [!NOTE]
> Eftersom vi bara har ett administratörskonto, eller rotkonto, på den virtuella datorn måste vi köra alla Docker-kommandon som rot med hjälp av `sudo`

1. Om du vill se vilka Docker-containrar som finns på den virtuella datorn kör du följande kommando i kommandotolken.

    ```azurecli 
    sudo docker ps
    ```

1. Kör följande kommando i kommandotolken för att skapa en ny container för våra experiment.

    ```azurecli 
    sudo docker run --rm -it --entrypoint '/bin/sh' -p 8888:8888 pytorch/pytorch -c \
    'conda install jupyter matplotlib -y &&\
    curl https://pytorch.org/tutorials/_downloads/cifar10_tutorial.ipynb > first_pytorch_classifier.ipynb &&\
    jupyter notebook --ip=0.0.0.0 --no-browser --allow-root'
    ``` 

    Det här kommandot kommer att köras ett tag. Därför passar vi på att diskutera vad kommandot gör. 
    - `docker run` kör ett kommando i en ny container. Den Docker-avbildning som används är pytorch/pytorch. Den skapar först ett skrivbart containerlager över den angivna avbildningen och startar det sedan med hjälp av det angivna kommandot.
    - `--rm` tar bort containern när den avslutas. Om du vill behålla containern tar du bort det här argumentet. 
    - `--entrypoint 'bin/sh'` skriver över standardstartpunkten för den avbildning som ska vara Bash-gränssnittet
    - `-c` definierar vilket kommando som ska köras när containern startas. I det här fallet körs tre kommandon:
        - Installerar Jupyter och matplotlib
        - Kopierar en notebook (cifar10_tutorial.ipynb) från pytorch.org till en fil i containern som heter `first_pytorch_classifier.ipynb`
        - Startar notebook-servern i containern på samma sätt som i den föregående övningen.  Det startas ingen webbläsare. Notebook tillåts kommas åt av roten och kan lyssna på alla portar. 
    
    Notebook-servern lyssnar på alla portar för den containern. Men hur kommer trafik utifrån containern in? Argumentet `-p 8888:8888` bilder porten `8888` i containern till TCP-port 8888 på värddatorn. Trafik till den virtuella datorn via port 8888 kommer därför att hämtas av containern. 

## <a name="connect-to-the-jupyter-notebook-server-from-a-remote-browser"></a>Ansluta till Jupyter Notebook-servern från en fjärransluten webbläsare 

När Jupyter Notebook körs i containern visas ett meddelande som liknar följande meddelande. 

> *Kopiera och klistra in URL:en i webbläsaren när du ansluter för första gången för att logga in med en token: http://(5b8783e7911d or 127.0.0.1):8888/?token={sometoken}*

1. Ersätt **http://(5b8783e7911d or 127.0.0.1)** av URL:en med `<HOSTNAME>.<REGION>.cloudapp.azure.com` eller IP-adressen för den virtuella datorn och gå till adressen i en ny en flik i webbläsaren.

    ![Skärmbild som visar instrumentpanelen för Jupyter Notebooks. ](../media/notebook-in-docker.png)
    
    Den här gången ser vi bara en enskild notebook. Det beror på vi är i en container och endast kopierade den här notebook. I nästa övning experimenterar vi med den här notebook. 
    
    > [!TIP]
    > Stäng inte av notebook-servern än. Vi tittar på `first_pytorch_classifier.ipynb`-notebook i nästa övning.
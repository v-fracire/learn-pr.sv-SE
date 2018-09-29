Fram tills nu har du laddat ned och kört Docker-avbildningar som skapats av andra. Här ska du skapa dina egna containeravbildningar. Du lär dig följande:

* Skapa en avbildning med hjälp av en manuell process.
* Skapa samma avbildning med hjälp av en mer automatiserad process som kallas Dockerfile.
* Publicera en avbildning i Docker Hub, ett offentligt containerregister.

## <a name="what-is-a-dockerfile"></a>Vad är en Dockerfile?

En Dockerfile är en textfil som beskriver allt som containern ska köra.

En instruktion i en Dockerfile kan definiera:

* Den överordnade avbildning som din avbildning baseras på.
* Ett programvarupaket som ska installeras.
* En konfigurationsfil eller en annan fil som din app ska köra.
* Nätverksinställningar, till exempel en port som du vill göra tillgänglig.
* Kommandot som ska köras när containern startas.

Du kan skapa en containeravbildning med hjälp av en manuell process eller använda en Dockerfile för att automatisera processen. Ett bra sätt att få förståelse för processen är att skapa en Docker-avbildning manuellt. Om du använder en Dockerfile blir processen snabbare och enklare att upprepa.

## <a name="what-is-docker-hub"></a>Vad är Docker Hub?

Docker Hub är en plats där du kan lagra och ladda ned Docker-avbildningar. Du kan ladda ned Docker-avbildningar som har skapats av Docker och Docker-communityn, till exempel Nginx-avbildningen som du använde tidigare. Du kan också dela dina egna avbildningar i Docker-communityn eller bara med ditt team.

## <a name="create-an-image-manually"></a>Skapa en avbildning manuellt

Här ska du skapa en Docker-avbildning som kör ett grundläggande Python-program.

1. Starta genom att hämta en containerinstans från [python](https://hub.docker.com/_/python/?azure-portal=true)-avbildningen.

    ```bash
    docker run --name python-demo -it python bash
    ```

    Det tar en liten stund för Docker att hämta avbildningen från Docker Hub. Medan du väntar kan vi titta på vad kommandot gör.

    * Argumentet `--name` anger din containers namn, ”python-demo”.
    * Argumenten `-i` och `-t` kombineras till ett argument, `-it`. Dessa argument skapar en interaktiv session med den container som körs.
      * `-i` skapar en interaktiv session med containern.
      * `-t` skapar en pseudoterminal som förblir i ett körningstillstånd.
    * `python` anger basavbildningen. Docker laddar ned den här avbildningen från Docker Hub. Avbildningen är förkonfigurerad med en miljö för Python-programmering.
    * `bash` anger vilket program ska köras.

    Tänk på den här konfigurationen som en interaktiv miljö där du kan skriva Python-appar. När din app är i drift kan du skapa en avbildning, eller en ögonblicksbild, av din miljö. När du förbättrar din app kan du skapa ytterligare avbildningar åt dig eller ditt team.

    Din terminalsession växlar över till containerns pseudoterminal. Frågan ser ut ungefär så här:

    ```docker
    root@d8ccada9c61e:/#
    ```

1. Från Docker-sessionen kör du detta för att skapa en katalog med namnet `./app` och flytta till den.

    ```bash
    mkdir ./app && cd ./app
    ```

    Även om det inte krävs ger katalogen `./app` dig en plats där du lägga till Python-koden.

1. Kör detta om du vill skapa ett grundläggande Python-program.

    ```bash
    echo 'print("Hello World!")' > hello.py
    ```

    Det här programmet skriver ut ”Hello World!” på terminalen.

    > [!TIP]
    > I praktiken kan du _montera_ en katalog på din arbetsstation till en katalog i din container. På så sätt kan du använda din favoritredigerare på din arbetsstation för att skriva kod. När du sparar filen visas filen även i din container.

1. Kör följande för att testa ditt Python-program.

    ```bash
    python hello.py
    ```

    Du ser det här.

    ```output
    Hello World!
    ```

1. Avsluta den interaktiva sessionen.

    ```bash
    exit
    ```

1. Från din Linux-VM kör du `docker ps` för att lista alla containrar som körs:

    ```bash
    docker ps
    ```

    Du ser att inget körs. 

    ```output
    CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS               NAMES
    ```

    Det beror på att om containern avslutas så avslutas Bash-sessionen. När programmet eller tjänsten som containern kör avslutas, stoppar Docker containern.

1. Kör `docker ps` en andra gång och inkludera argumentet `-a`. Det här kommandot returnerar alla containrar, som körs eller har stoppats.

    ```bash
    docker ps -a
    ```

    Du ser något som liknar följande:

    ```output
    CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS                      PORTS               NAMES
    cf6ac8e06fd9        python              "bash"              27 seconds ago      Exited (0) 12 seconds ago                       python-demo
    ```

   Din container, **python-demo**, har statusen **Avslutades**.

1. Kör `docker commit` för att skapa en avbildning från din container.

   ```bash
   docker commit python-demo python-custom
   ```

   Här skapar Docker en avbildning med namnet **python-custom** från din container med namnet **python-demo**. Avbildningens namn kan vara vad som helst.

1. Kör `docker images` för att lista dina avbildningar.

    ```bash
    docker images
    ```

    Du ser avbildningen **python-custom**.

    ```output
    REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
    python-custom       latest              1f231e7127a1        6 seconds ago       922MB
    python              latest              638817465c7d        24 hours ago        922MB
    ```

1. Använd `docker run` för att testa avbildningen.

    ```bash
    docker run python-custom python app/hello.py
    ```

    Kom ihåg att `docker run` tar kommandot för att köra det som sitt sista argument. Här är kommandot `python app/hello.py`.

    Du ser det här.

    ```output
    Hello World!
    ```

    Eftersom du inte kör den här containern interaktivt, körs Python-programmet och containern stoppas.

## <a name="use-a-dockerfile-to-create-an-image-automatically"></a>Använda en Dockerfile för att skapa en avbildning automatiskt

Här ska du använda en mer automatiserad process för att skapa samma Docker-avbildning som kör ditt Python-program.

Att skapa en avbildning manuellt är ett bra sätt att experimentera och utforska nya funktioner. Men anta att du vill dela processen med ditt team eller göra den upprepningsbar. Anta exempelvis att du vill skapa en ny avbildning varje kväll som kör den senaste versionen av programvaran som ditt team bygger upp.

Det är där Dockerfile kommer in. Tänk på en Dockerfile som ett sätt att beskriva instruktioner som du annars skulle utföra manuellt.

1. Kör detta kommando från din virtuella Linux-dator för att skapa en Dockerfile som kör ditt Python-program.

    ```bash
    cat << EOF > Dockerfile
    FROM python
    
    WORKDIR ./app
    
    RUN echo 'print("Hello World!")' > hello.py
    
    CMD python hello.py
    EOF
    ```

    Det här exemplet är ett enkelt sätt för dig att skapa filen. I vanliga fall skulle du använda valfri kodredigerare för att skapa den.

    Skriv ut din Dockerfile till konsolen.

    ```bash
    cat Dockerfile
    ```

    Du ser det här.

    ```output
    FROM python
    
    WORKDIR ./app
    
    RUN echo 'print("Hello World!")' > hello.py
    
    CMD python hello.py
    ```

    Lås oss ta en titt på vad en Dockerfile gör.

    * `FROM` anger den överordnade avbildningen som du baserar den här avbildningen på. Här är den överordnade avbildningen **python**.
    * `WORKDIR` anger arbetskatalogen för `RUN`- och `CMD`-instruktioner som visas senare i Dockerfile. Docker skapar den här katalogen åt dig, i det här fallet `./app`.
    * `RUN` anger ett kommando som ska köras i containern. `RUN` kan användas för att installera programvara, göra ändringar i konfigurationen och rensa containern före överföringen. Här ska vi skriva ett grundläggande Python-program till `./app/hello.py`.
    * `CMD` anger processen som ska köras när containern startas. Här kör vi `python hello.py` från katalogen `/.app`.

    Du har antagligen lagt märke till att dessa steg är nästan exakt desamma som du använde för att skapa avbildningen manuellt. Genom att ange de här stegen som kod blir processen enklare att dela och upprepa.

1. Kör `docker build` för att skapa avbildningen med hjälp av de instruktioner som anges i Dockerfile.

    ```bash
    docker build -t python-dockerfile .
    ```

    Det tar vanligtvis bara några sekunder för Docker för att skapa din avbildning.

    Du bör se utdata enligt följande.

    ```output
    Sending build context to Docker daemon  2.048kB
    Step 1/4 : FROM python
     ---> 638817465c7d
    Step 2/4 : WORKDIR ./app
     ---> Running in 990d17e86466
    Removing intermediate container 990d17e86466
     ---> 59a074a092cc
    Step 3/4 : RUN echo 'print("Hello World!")' > hello.py
     ---> Running in aed707c53bc5
    Removing intermediate container aed707c53bc5
     ---> d7f55a9d0e85
    Step 4/4 : CMD python hello.py
     ---> Running in e87ec55a8d36
    Removing intermediate container e87ec55a8d36
     ---> 98c39b91770f
    Successfully built 98c39b91770f
    Successfully tagged python-dockerfile:latest
    ```

1. Använd `docker images` för att lista dina avbildningar.

    ```bash
    docker images
    ```

    Du ser avbildningen **python-dockerfile** som du precis skapat.

    ```output
    REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
    python-dockerfile   latest              8ed5f85c5e5b        24 seconds ago      923MB
    python-custom       latest              6789cbe2cceb        4 minutes ago       923MB
    python              latest              a9d071760c82        2 weeks ago         923MB
    ```

1. Använd `docker run` för att testa containern.

    ```bash
    docker run python-dockerfile
    ```

    Du ser det här.

    ```output
    Hello World!
    ```

    Till skillnad från när du testade avbildningen du skapade manuellt anger du inte här kommandot som ska köras, `python app/hello.py`. Det anges i din Dockerfile, så att kommandot byggs in i avbildningen.

## <a name="publish-your-image-to-docker-hub"></a>Publicera avbildningen till Docker Hub

När du ska publicera en avbildning till Docker Hub behöver du ett konto. Här kan du konfigurera ett Docker Hub-konto och publicera din **python-dockerfile**-avbildning till Docker Hub.

1. [Skapa ditt Docker Hub-konto](https://hub.docker.com?azure-portal=true).

1. Exportera namnet på ditt Docker Hub-konto som en miljövariabel. Ersätt **account-name** med ditt kontonamn.

    ```bash
    export docker_account=account-name
    ```

    Den här miljövariabeln gör det enklare för dig att köra kommandona som följer.

1. Kör `docker tag` för att tagga avbildningen med ditt Docker Hub-kontonamn.

    ```bash
    docker tag python-dockerfile $docker_account/python-dockerfile
    ```

1. Kör `docker login` för att logga in på Docker Hub.

    ```bash
    docker login
    ```

    Ange användarnamn och lösenord när du uppmanas att göra det.

1. Kör `docker push` för att push-överföra, eller ladda upp, din **python-dockerfile**-avbildning till Docker Hub.

    ```bash
    docker push $docker_account/python-dockerfile
    ```

    Du ser utdata som liknar följande:

    ```output
    The push refers to repository [docker.io/account/python-dockerfile]
    f39073ca4d5a: Pushed
    9dfcec2738a9: Pushed
    ffab8273c674: Mounted from account/python
    e6f6cbe5e14e: Mounted from account/python
    3eb255f8a6ac: Mounted from account/python
    b860b6c48eec: Mounted from account/python
    1fa8778eb779: Mounted from account/python
    fa0c3f992cbd: Mounted from account/python
    ce6466f43b11: Mounted from account/python
    719d45669b35: Mounted from account/python
    3b10514a95be: Mounted from account/python
    latest: digest: sha256:b816c32382d06ac74530d56f2a7b83000c86174218f430c6bb5039fdcfba38aa size: 2631
    ```

    Docker-avbildningen lagras nu på Docker Hub. Du kan använda `docker pull` eller `docker run` från en annan dator för att ladda ned eller köra avbildningen från Docker Hub. Här följer två exempel:

    1. Med det här exemplet hämtas den senaste avbildningen från Docker Hub.

        ```bash
        docker pull $docker_account/python-dockerfile
        ```

    1. Med det här exemplet körs containern.

        ```bash
        docker run $docker_account/python-dockerfile
        ```

1. Testa din container.

    Gå till [ditt Docker Hub-konto](https://hub.docker.com?azure-portal=true). Du ser Docker-avbildningen på fliken **Centrallager**.

    ![Docker-avbildningen på Docker Hub](../media/4-docker-hub-python-dockerfile.png)

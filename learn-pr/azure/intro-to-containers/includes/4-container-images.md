Här kan du skapa anpassade avbildningar, överför dessa avbildningar till en offentlig behållarregister och kör behållare från dessa avbildningar.

Behållaravbildningar kan skapas manuellt eller använda en Dockerfile för att automatisera processen. Den bästa metoden använder en Dockerfile, men denna enhet visar båda riktningarna. Använder den manuella processen hjälper dig att bättre förstå vad det som händer när du använder en Dockerfile för automation.

## <a name="manual-image-creation"></a>Skapa avbildning manuellt

När du skapar en containeravbildning manuellt vidtas följande åtgärder:

- Starta en instans av en container.
- Upprätta en terminalsession med containern.
- Ändra containern genom att installera programvara och göra ändringar i konfigurationen.
- Spara containern till en ny avbildning med kommandot `docker capture`.

I det första exemplet startar du en instans av en container som kör Python, skapar ett Hello World-program och sparar sedan containern till en ny avbildning.

Kör först en container från NGINX-avbildningen. Det här kommandot lite ser annorlunda ut än de kommandon som du körde i den föregående enheten. Eftersom du ska upprätta en terminalsession med den container som körs anges argumenten `-t` och `-i`. De här argumenten instruera tillsammans Docker för att allokera en pseudo terminal som finns kvar i ett fungerande tillstånd. Med andra ord skapar argumenten `-t` och `-i` en interaktiv session med den container som körs.

Du anger sedan att `python`-containeravbildningen används och att den process som ska köras inuti containern är `bash`.

```bash
docker run --name python-demo -ti python bash
```

När kommandot har körts bör terminalsessionen växla till containerpseudoterminalen. Detta kan ses av uppmaningen, som bör ha ändrats till något som liknar följande:

```output
root@d8ccada9c61e:/#
```

I det här skedet arbetar du inuti container. Du kan lista, skapa, ta bort filer, installera programvara och göra ändringar i konfigurationen. För det här enkla exemplet skapas ett Python-baserat Hello World-skript. Detta kan göras med följande kommando:

```bash
echo 'print("Hello World!")' > hello.py
```

Testa skriptet medan du fortfarande är i containern genom att köra följande kommando:

```bash
python hello.py
```

Detta genererar följande utdata:

```output
Hello World!
```

När du är nöjd som skriptet fungerar som förväntat, avsluta behållaren genom att skriva `exit`:

```bash
exit
```

När du är tillbaka i terminalen för det lokala systemet använder du kommandot `docker ps` för att lista alla containrar som körs:

```bash
docker ps
```

Observera att inget körs. När du angav `exit` i den container som körs slutfördes Bash-processen, vilket sedan stoppade containern. Detta är förväntat beteende.

```output
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS               NAMES
```

Använd `docker ps` igen och inkludera argumentet `-a`. Det här kommandot returnerar en lista över alla containrar, oavsett om de körs.

```bash
docker ps -a
```

Observera att en container med namnet *python-demo* har statusen *Exited* (Stängd). Den här behållaren är stoppade instansen på den behållare som du precis har avslutats.

```output
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS                      PORTS               NAMES
cf6ac8e06fd9        python              "bash"              27 seconds ago      Exited (0) 12 seconds ago                       python-demo
```

Du kan skapa en ny containeravbildning från den här containern med kommandot `docker commit`. I följande exempel instruerar *docker commit* att en avbildning med namnet *python-customn* ska skapas från *python-demo*-containrarna.

```bash
docker commit python-demo python-custom
```

När kommandot har slutförts bör du se utdata som liknar följande:

```output
sha256:91a0cf9aa9857bebcd7ebec3418970f97f043e31987fd4a257c8ac8c8418dc38
```

Kör nu `docker images` för att se en lista över containerbildningar:

```bash
docker images
```

Du bör nu se den anpassade Python-avbildningen.

```output
REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
python-custom       latest              1f231e7127a1        6 seconds ago       922MB
python              latest              638817465c7d        24 hours ago        922MB
alpine              latest              11cd0b38bc3c        2 weeks ago         4.41MB
```

Kör en container från den nya avbildningen. Du behöver även ange vilket kommando eller vilken process som ska köras inuti containern. Med det här exemplet kör du `python hello.py`.


```bash
docker run python-custom python hello.py
```

Containern startar och skickar Hello World-meddelandet som utdata. Python-processen är sedan klar, och containern stoppas.

```output
Hello World!
```

## <a name="automated-image-creation"></a>Skapa en avbildning automatiskt

I det sista avsnittet skapar du en behållaravbildning manuellt. Den här metoden fungerar för att skapa en oneoff eller experimentella bild, men det är inte hållbar i en produktionsmiljö är. Det går att automatisera skapandet av containeravbildningar med hjälp av en *Dockerfile* och kommandot `docker build`. Den *docker build* kommando startar en behållare, körs anvisningarna finns i den *Dockerfile*, och avbildar sedan resultaten till en ny behållaravbildning.

Skapa en fil med namnet *Dockerfile* och ange följande text:

```bash
FROM python

WORKDIR ./app

RUN echo 'print("Hello World!")' > hello.py

CMD python hello.py
```

Instruktionen *FROM* anger den basavbildning som ska användas när avbildningar skapas. Instruktionen *RUN* kör kommandon inuti containern. *RUN* kan användas för att installera programvara, göra ändringar i konfigurationen och rensa containern före överföringshändelsen. Instruktionen *CMD* anger den process som ska köras när en container startas.

Använd kommandot `docker build` för att skapa en ny containeravbildning med hjälp av de instruktioner som anges i Dockerfile.

```bash
docker build -t python-dockerfile .
```

Du bör se utdata som liknar följande.

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

Använd kommandot `docker images` för att returnera en lista över containeravbildningar.

```bash
docker images
```

Du bör nu se den anpassade avbildningen.

```output
REPOSITORY          TAG                 IMAGE ID            CREATED              SIZE
python-dockerfile   latest              98c39b91770f        About a minute ago   922MB
python              latest              638817465c7d        26 hours ago         922MB
alpine              latest              11cd0b38bc3c        2 weeks ago          4.41MB
```

Använd kommandot `docker run` för att köra en container från den anpassade avbildningen.

Lägg märkte till att det inte har angetts några argument till kommandot `docker run`. Till skillnad från när du manuellt skapar en containeravbildning gör Dockerfile att du kan inkludera ett kommando som körs när containern startas. I det här fallet är det angivna kommandot `python hello.py`, vilket gör att containern kör Python-skriptet.

```bash
docker run python-dockerfile
```

När du har kört kommandot bör du se containerutdata.

```output
Hello World!
```

## <a name="push-the-image-to-a-public-registry"></a>Överföra avbildningen till ett offentligt register

Docker Hub är ett offentligt containerregister. För att kunna överföra containeravbildningar till Docker Hub måste du ha ett Docker-konto. Vid behov kan du besöka [Docker Hub-webbplatsen](https://hub.docker.com/) för att registrera ett konto.

När du har ett Docker Hub-konto måste containeravbildningen taggas med kontonamnet. Det gör du med kommandot `docker tag`.

I följande exempel är *python-dockerfile*-avbildningen taggad med ett Docker Hub-kontonamn. Ersätt `<account name>` med ditt Docker Hub-kontonamn.

```bash
docker tag python-dockerfile <account name>/python-dockerfile
```

Därefter kontrollerar du att du är inloggad i Docker Hub med hjälp av kommandot `docker login`.

```bash
docker login
```

Slutligen överför *python-dockerfile*-avbildningen till Docker Hub med kommandot `docker push`. Ersätt `<account name>` med ditt Docker Hub-kontonamn.

```bash
docker push <account name>/python-dockerfile
```

Medan behållaravbildningen överförs till Docker Hub, visas utdata som liknar följande:

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
```

Containeravbildningen lagras nu på Docker Hub och kan nås från valfri Internetansluten dator med hjälp av `docker pull` eller `docker run`.

Nu har du skapat två behållaravbildningar. Den första skapades manuellt, och den andra automatiserades med hjälp av en Dockerfile.
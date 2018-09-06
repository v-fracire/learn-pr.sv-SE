### <a name="exercise-3-train-a-tensorflow-model"></a>Övning 3: Träna en TensorFlow-modell

I den här övningen kommer du att träna en bildklassificeringsmodell som byggts med [TensorFlow](https://www.tensorflow.org/) för att identifiera bilder som innehåller varmkorvar. I stället för att skapa modellen från grunden, vilket skulle kräva enorma mängder datorkraft och tiotusentals eller hundratusentals bilder kommer du att anpassa en befintlig modell. Det är en metod som kallas för [transfer learning](https://en.wikipedia.org/wiki/Transfer_learning), överförd inlärning. Med överförd inlärning kan du få hög precision efter bara ett par minuters träningstid på en vanlig bärbar eller stationär dator och med så lite som några dussin bilder.

När det handlar om djupinlärning utgår överförd inlärning från ett djupt neuralt nätverk som har tränats för att utföra bildklassificering. Ett lager som anpassar nätverket efter din problemdomän läggs till; till exempel att dela upp bilder i två grupper: de som innehåller varmkorvar och de som inte gör det. Fler än 20 redan tränade TensorFlow-modeller för bildklassificering är tillgängliga i modellerna<https://github.com/tensorflow/models/tree/master/research/slim#pre-trained-models.> The [Inception](https://arxiv.org/abs/1512.00567) och [ResNet](https://towardsdatascience.com/an-overview-of-resnet-and-its-variants-5281e2f56035). Dessa kännetecknas av högre precision och följaktligen även större resursbehov. MobileNet-modellerna å andra sidan kompromissar när det gäller precision, men väger upp i format och energieffektivitet, och dessa har utvecklats med mobila enheter i åtanke. Alla dessa modeller är välkända bland djupinlärningsexperter och de har använts vid ett antal tävlingar samt i verkliga program. Du kommer att använda en av MobileNet-modellerna som grund för ditt neurala nätverk för att uppnå en rimlig balans mellan precision och träningstid.

Att träna modellen kräver lite mer än att bara köra ett Python-skript som laddar ned basmodellen och lägger till ett lager som tränats med domänspecifika bilder och etiketter. Skriptet du behöver finns på GitHub och bilderna du kommer att använda sätts samman från tusentals matbilder från den offentliga domänen som finns på [Kaggle](https://www.kaggle.com).

1. I Data Science VM klickar du på terminalikonen längst ned på skärmen för att öppna ett terminalfönster.

    ![Öppna ett terminalfönster](../images/launch-terminal.png)

    _Öppna ett terminalfönster_

1. Kör följande kommando i terminalfönstret för att navigera till mappen ”notebooks”:

    ```bash
    cd notebooks
    ```
    Den här mappen innehåller redan exempel på Jupyter-anteckningsböcker som samlats ihop för DSVM.

1. Använd nu följande kommando för att klona lagringsplatsen för ”TensorFlow för Poets” från GitHub:

    ```bash
    git clone https://github.com/googlecodelabs/tensorflow-for-poets-2
    ```
    > **Tips**: Du kan kopiera den här raden till Urklipp och sedan använda **Skift + Ins** för att klistra in den i terminalfönstret.

    Den här lagringsplatsen innehåller skript för att skapa modeller för överförd inlärning, anropa en tränad modell för att kunna klassificera en bild och mycket mer. Det är en del av [Google Codelabs](https://codelabs.developers.google.com/), som innehåller en mängd olika resurser och praktiska övningar för programutvecklare som vill lära sig mer om TensorFlow och andra Google-verktyg och API:er.

1. När kloningen är avslutad navigerar du till mappen som innehåller den klonade modellen:

    ```bash
    cd tensorflow-for-poets-2
    ```

1. Du kan använda följande kommando för att ladda ned de bilder som du ska använda för att träna modellen:

    ```bash
    wget https://topcs.blob.core.windows.net/public/tensorflow-resources.zip -O temp.zip; unzip temp.zip -d tf_files; rm temp.zip
    ```

    Det här kommandot laddar ned en zip-fil som innehåller hundratals matbilder – hälften innehåller varmkorvar och den andra hälften gör det inte – och kopierar dem till undermappen som heter ”tf_files”.

1. Klicka på filhanteringsikonen längst ned på skärmen för att öppna ett fönster med filhanteraren.

    ![Starta filhanteraren](../images/launch-file-manager.png)

    _Starta filhanteraren_

1. I filhanteraren navigerar du till mappen ”notebooks/tensorflow-for-poets-2/tf_files”. Bekräfta att mappen innehåller underkatalogerna ”hot_dog” och ”not_hot_dog”. Den förra innehåller flera hundra bilder som innehåller varmkorvar, medan den senare innehåller samma antal bilder som **inte** innehåller varmkorvar. Bläddra bland bilderna i mappen ”hot_dog” för att få en känsla för hur de ser ut. Titta också på bilderna i mappen ”not_hot_dog”.

    > För att träna ett neuralt nätverk så att det kan avgöra om en bild innehåller en varmkorv kommer du att träna det med bilder med och utan varmkorvar.

    ![Bilder i mappen ”hot_dog”](../images/hot-dog-images.png)

    *Bilder i mappen ”hot_dog”*

    Bekräfta också att mappen innehåller en textfil med namnet **retrained_labels_hotdog.txt**. Den här filen identifierar underkatalogerna som innehåller träningsbilderna. Den används av Python-skriptet som tränar modellen. Skriptet räknar filerna i varje underkatalog som identifierats i textfilen (textfilens namn är en parameter som skickas till skriptet) och använder de filerna för att träna nätverket.

1. Öppna ett andra terminalfönster och navigera till mappen ”notebooks/tensorflow-for-poets-2” – samma mapp som är öppen i det första terminalfönstret. Använd sedan följande kommando för att starta [TensorBoard](https://www.tensorflow.org/programmers_guide/summaries_and_tensorboard), vilket är en uppsättning verktyg som används för att visualisera TensorFlow-modeller och få insyn i processen för överförd inlärning:

     ```bash
     tensorboard --logdir tf_files/training_summaries
     ```

     > Det här kommandot misslyckas om en instans av TensorBoard redan körs. Om du får ett meddelande om att port 6006 redan används använder du ett ```pkill -f "tensorboard"```-kommando för att avsluta den befintliga processen. Kör sedan kommandot ```tensorboard``` igen.

1. Växla tillbaka till det ursprungliga terminalfönstret och kör följande kommandon:

    ```bash
    IMAGE_SIZE=224;
    ARCHITECTURE="mobilenet_0.50_${IMAGE_SIZE}";
    ```

    Dessa kommandon initierar miljövariabler som anger upplösningen för träningsbilderna och basmodellen som det neurala nätverket byggs på. Giltiga värden för IMAGE_SIZE är 128, 160, 192 och 224. Högre värden ökar träningstiden, men ökar också precisionen för klassificeraren.

1. Kör nu följande kommando för att starta processen för överförd inlärning – det vill säga för att träna modellen med bilderna du laddat ned:

    ```bash
    python scripts/retrain.py \
    --bottleneck_dir=tf_files/bottlenecks \
    --how_many_training_steps=500 \
    --model_dir=tf_files/models/ \
    --summaries_dir=tf_files/training_summaries/"${ARCHITECTURE}" \
    --output_graph=tf_files/retrained_graph_hotdog.pb \
    --output_labels=tf_files/retrained_labels_hotdog.txt \
    --architecture="${ARCHITECTURE}" \
    --image_dir=tf_files \
    --testing_percentage=15 \
    --validation_percentage=15
    ```

    **retrain.py** är ett av skripten på lagringsplatsen som du laddade ned. Det är komplext och består av fler än 1 000 rader med kod och kommentarer. Dess uppgift är att ladda ned den angivna modellen med ```--architecture```-växeln och lägga till ett nytt lager som tränats med bilderna i underkatalogerna för den katalog som angetts med ```--image_dir```-växeln. Varje bild är märkt med namnet på underkatalogen där den finns – i det här fallet ”hot_dog” eller ”not_hot_dog” – vilket gör det möjligt för det modifierade neurala nätverket att klassificera bilder som läggs till som bilder av varmkorvar (”hot_dog”) eller bilder som inte innehåller varmkorvar (”not_hot_dog”). Utdata från träningssessionen är en TensorFlow-modellfil med namnet **retrained_graph_hotdog.pb**. Namn och plats anges i ```--output_graph```-växeln.

1. Vänta tills träningen har slutförts. Det bör ta mindre än fem minuter. Kontrollera sedan resultatet för att fastställa hur exakt modellen är. Resultaten kan variera något från det som visas nedan, eftersom träningsprocessen omfattar en liten mängd slumpmässiga uppskattningar.

      ![Mäta modellens precision](../images/running-transfer-learning.png)

      _Mäta modellens precision_

1. Klicka på webbläsarikonen längst ned på skrivbordet för att öppna webbläsaren som installerats på Data Science VM. Navigera sedan till <http://0.0.0.0:6006> för att ansluta till Tensorboard.

    ![Starta Firefox](../images/launch-firefox.png)

    _Starta Firefox_

1. Granska diagrammet ”accuracy_1”. Den blå linjen visar precisionen som uppnåtts över tid efter det att de 500 träningsstegen som angetts med ```how_many_training_steps```-växeln utförts. Det här mätvärdet är viktigt eftersom det visar hur modellens precision utvecklas i takt med att träningen fortskrider. Lika viktigt är avståndet mellan de blå och orangefärgade linjerna. De beräknar mängden överanpassning som sker och bör alltid minimeras. [Överanpassning](https://en.wikipedia.org/wiki/Overfitting) innebär att modellen är bra på att klassificera bilderna den tränats med, men inte lika bra på att klassificera andra bilder som visas för den. Resultaten här är godtagbara eftersom skillnaden är mindre än 10 % mellan den orangefärgade linjen (”träningsprecisionen” som uppnåtts med träningsbilderna) och den blå linjen (”valideringsprecisionen” som uppnåtts när modellen testats med bilder utanför träningsuppsättningen).

    ![TensorBoard Scalars-vyn](../images/tensorboard-scalars.png)

    _TensorBoard Scalars-vyn_

1. Klicka på **DIAGRAM** på TensorBoard-menyn och inspektera diagrammet som visas där. Det primära syftet med det här diagrammet är att skildra det neurala nätverket och lagren som det består av. I det här exemplet är ”input_1” lagret som har tränats med matbilder och lagts till i nätverket. ”MobilenetV1” är det neurala basnätverk som du utgick från. Det innehåller många lager som inte visas. Om du hade skapat ett djupt neuralt nätverk från grunden skulle alla lager ingått i diagrammet här. (Om du vill se de lager som utgör MobileNet dubbelklickar du på blocket MobilenetV1 i diagrammet). Du hittar mer information om diagramvyn och den informationen som visas där i [TensorBoard: diagramvisualisering](https://www.tensorflow.org/programmers_guide/graph_viz).

    ![TensorBoard-diagramvyn](../images/tensorboard-graphs.png)

    _TensorBoard-diagramvyn_

1. Återgå till filhanteraren och navigera till mappen ”notebooks/tensorflow-for-poets-2/tf_files”. Bekräfta att den innehåller en fil med namnet **retrained_graph_hotdog.pb**. *Den här filen skapades under träningsprocessen och innehåller den tränade TensorFlow-modellen*. Du använder den i nästa övning för att anropa modellen från appen NotHotDog.

Skriptet som du körde i steg 10 angav 500 träningssteg, vilket skapar en balans mellan precision och den tid som krävs för träningen. Om du vill kan du prova att träna modellen igen med ett högre ```how_many_training_steps```-värde, till exempel 1 000 eller 2 000. Ett högre antal steg resulterar vanligtvis i högre precision, men på bekostnad av ökad träningstid. Se upp för överanpassning, vilket, som vi redan nämnt, framgår av skillnaden mellan den orangefärgade och den blå linjen i Tensorboard-vyn med skalaxlar.
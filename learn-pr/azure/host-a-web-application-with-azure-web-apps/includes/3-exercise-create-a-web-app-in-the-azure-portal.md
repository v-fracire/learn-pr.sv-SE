I den här övningen ska du använda Azure Portal för att skapa en Azure-webbapp.

## <a name="sign-in-to-the-azure-portal"></a>Logga in på Azure-portalen

Det första steget är att logga in på Azure-portalen:

Öppna webbläsaren och gå till [Azure-portalen](https://portal.azure.com/?azure-portal=true).

## <a name="create-an-azure-web-app"></a>Skapa en Azure-webbapp

Nu när vi är inloggade på Azure-portalen ska vi skapa en Azure-webbapp med hjälp av mallen.

1. Klicka på länken **Skapa en resurs** överst i det vänstra navigeringsfönstret. Allt som du skapar i Azure är en resurs.

1. Bladet **Marketplace** öppnas på portalen. Härifrån kan du söka efter en resurs som du vill skapa eller välja någon av de populära resurser som du kan skapa på Azure-portalen.

1. Klicka på resursen **Webb** > **Webbapp**. Du omdirigeras till bladet **Skapa ny webbapp**.

1. När du skapar en ny webbapp behöver Azure-portalen en del information för att kunna skapa appen åt dig. I det här avsnittet måste du ange följande grundläggande information:

    1. **Appnamn**: Din kund vill att programmet ska ha namnet `BestBike`. Ange namnet i det här fältet. Det här värdet måste vara globalt unikt för alla andra webbappar som körs på Azure, och portalen ser till att ingen annan har använt appnamnet. För att säkerställa att ditt namn är unikt ska du lägga till ett nummer till appens namn tills du hittar en unik kombination.

    2. **Prenumeration**: I det här fältet måste du välja en aktiv Azure-prenumeration i listrutan.

    3. **Operativsystem**: I det här fältet måste du bestämma om du vill använda **Windows** eller **Linux** som värd för din nya webbapp. Den här inställningen påverkar App Service-planen som du ska välja eller skapa nedan. Som du kanske kommer ihåg påminner en App Service-plan om en virtuell dator, som är ett operativsystem med alla resurser (processor, RAM-minne osv.) som behövs på datorn för att köra ditt program. I det här fallet vill din kund använda en Windows-dator som värd för webbappen. Därför väljer du **Windows**.

    4. **Application Insights**: Azure Application Insights hjälper dig att identifiera och diagnostisera kvalitetsproblem i dina webbappar och webbtjänster och hjälper dig att förstå vad användarna faktiskt gör med dem. Ett av din kunds krav är att få rapporter om trafiken på webbplatsen för att identifiera trender, t.ex. när trafiken är hög och när den är låg. I det här fallet väljer du alternativet **På** för att aktivera Application Insights för webbappen. När du har valt alternativet **På** måste du också välja platsen eller regionen där Application Insights-data ska lagras. Observera att Application Insights endast är tillgängligt i ett begränsat antal regioner. I den här demonstrationen väljer du regionen **USA, östra** i fältet **Application Insights-plats**.

## <a name="create-a-new-resource-group"></a>Skapa en ny resursgrupp

En Azure-webbapp måste vara en del av en resursgrupp. Nu ska vi skapa en resursgrupp för att gruppera alla relaterade resurser.

Välj alternativet **Skapa ny** (valt som standard). Azure skapar ett unikt resursgruppsnamn baserat på det appnamn du har valt. Du kan ändra resursgruppens namn eller behålla det som Azure har genererat. I det här fallet behåller du det namn som genererats av Azure.

## <a name="create-an-app-service-plan"></a>Skapa en App Service-plan

I det här fältet måste du välja en App Service-plan för att köra ditt program. Som standard väljer portalen den senaste App Service-plan du tidigare skapat. Klicka på fältet **App Service-plan/plats** för att gå till bladet **App Service-plan**.

Klicka på länken **Skapa ny** för att gå till bladet **Ny App Service-plan**. Portalen behöver en del information av dig för att kunna skapa den nya App Service-planen.

1. **App Service-plan**: I det här fältet anger du ett globalt unikt namn för den nya App Service-planen. För den här appen skriver du samma webbappsnamn som du valde ovan och lägg till suffixet `-app-service-plan` så att du lätt kan skilja den här resursen från andra.

2. **Plats**: I det här fältet måste du välja den region som App Service-planen finns i. Det vill säga, välj den geografiska platsen där App Service-planen ska konfigurera virtuella datorer som krävs för att köra ditt program. I det här fallet kan du välja valfritt alternativ i listan. Din kund föredrar värdservrarna någonstans nära USA:s östkust där merparten av hans klienter finns. Därför väljer du **USA, östra** som värde.

3. **Prisnivå**: I det här fältet måste du välja storleken på den virtuella datorn som ska vara värd för ditt program. Klicka på **>**-tecknet för att öppna bladet **Prisnivå**.

    Här har du många alternativ att välja mellan. Portalen grupperar alternativen baserat på den arbetsbelastningsnivå som krävs. De tre tillgängliga arbetsbelastningskategorierna är: Utveckling/testning, Produktion och Isolerad. Välj relevant arbetsbelastningskategori beroende på kraven för programmet som ska köras i Azure. Eftersom vi fortfarande håller på att utveckla programmet **BestBike** börjar du med den minsta arbetsbelastningskategorin som passar dig. Kom ihåg att ett av kundens krav var att kunna testa nya ändringar i programmet i realtid. Som du ser i de kommande kursdelarna kräver detta att du lägger till **distributionsplatser**. Distributionsplatser är tillgängliga för en minsta prisnivå motsvarande **S1**. Välj därför prisnivån **S1** under kategorin för **produktionsarbetsbelastningar**. Klicka sedan på **Använd** för att bekräfta prisnivån du valt ovan.

    > [!NOTE]
    > Som du kommer att märka i den här modulen kan du endast lägga till **distributionsplatser** till din webbapp med kategorierna **Produktion** och **Isolerad**.

    Nu är du tillbaka på bladet **Ny App Service-plan**.

    ![Skärmbild som visar bladet Ny App Service-plan med exempelvärden för den här övningen i inställningarna](../media/3-new-app-service-plan.PNG)

4. Klicka på knappen **OK** för att använda din nya App Service-plan.

    Du kommer tillbaka till huvudbladet **Skapa webbapp**.

    ![Skärmbild som visar det nya resursbladet i Azure med processen att hitta webbappsresursen markerad.](../media/3-new-web-app.png)

5. Klicka på knappen **Skapa** för att börja skapa webbappen.

    > [!NOTE]
    > Det kan ta några sekunder att skapa webbappen och göra den redo för användning.

Du omdirigeras till sidan Instrumentpanel och meddelas när den nya webbappen har skapats.

När appen är klar, går du till den nya appen i Azure-portalen.

1. Klicka på menyn **Alla resurser** i det vänstra navigeringsfönstret. Bladet **Alla resurser** visar en lista över alla resurser som du har skapat på Azure-portalen.

2. Klicka dig igenom BestBike App Service som skapas åt dig.

    > [!NOTE]
    > Om du söker efter din app med namnet ”BestBike” kan du också hitta Application Insights och App Service-planresurser som skapats för din nya webbapp. Försäkra dig om att du klickar dig igenom resursen av typen **App Service**.

    ![Skärmbild som visar sökresultat i bladet för alla resurser, med den nyligen skapade BestBike123 App Service markerad.](../media/3-web-app.PNG)

Portalen öppnas startsidan för webbtjänsten med avsnittet **Översikt** markerat.

![Skärmbild som visar bladet BestBike App Service med URL:en i översiktsavsnittet markerad.](../media/3-web-app-home.PNG)

Om du vill förhandsgranska din nya webbapps standardinnehåll, klickar du på **URL:en** längst upp till höger på Azure-portalen. Om du ser en platshållarwebbsida innebär det att webbappen fungerar.

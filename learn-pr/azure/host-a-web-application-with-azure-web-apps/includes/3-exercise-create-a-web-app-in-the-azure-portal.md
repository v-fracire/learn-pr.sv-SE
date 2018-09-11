I den här övningen ska du använda Azure Portal för att skapa en Azure-webbapp.

## <a name="log-in-to-the-azure-portal"></a>Logga in på Azure Portal

Första steget är att logga in på Azure Portal:

1. Öppna en webbläsare och gå till [Azure Portal](https://portal.azure.com).

1. Logga in med dina autentiseringsuppgifter.

1. När du har loggat in kommer du till **instrumentpanelen** eller startsidan för Azure Portal.

    ![Azure Portal](../media-draft/3-azure-portal.PNG)

## <a name="create-an-azure-web-app"></a>Skapa en Azure-webbapp

Nu när vi är inloggade på Azure-portalen ska vi skapa en Azure-webbapp med hjälp av mallen.

1. Klicka på länken **Skapa en resurs** överst till vänster i navigeringsfönstret. Allt som du skapar i Azure är en resurs.

    ![Azure Marketplace](../media-draft/3-market-place.PNG)

1. Bladet **Marketplace** öppnas på portalen. Härifrån kan du söka efter en resurs som du vill skapa eller välja någon av de populära resurser som du kan skapa i Azure Portal.

1. Klicka på resursen **Webbapp** i avsnittet **Populära**. Du omdirigeras till bladet **Skapa ny webbapp**.

    ![Bladet Skapa ny webbapp](../media-draft/3-create-new-web-app-page.PNG)

1. När du skapar en ny webbapp behöver Azure-portalen en del information för att kunna skapa appen åt dig. I det här avsnittet måste du ange följande grundläggande information:

    1. **Appnamn**: Din kund vill att programmet ska ha namnet `BestBike`. Ange namnet i det här fältet. Azure Portal kontrollerar och verifierar om webbappens namn är ledigt bland alla webbappar som körs i Azure för att kontrollera att ingen annan har använt namnet `BestBike`.

    1. **Prenumeration**: I det här fältet måste du välja en aktiv Azure-prenumeration i listrutan.

    1. **Operativsystem**: I det här fältet måste du bestämma om du vill använda **Windows** eller **Linux** som värd för din nya webbapp. Den här inställningen påverkar App Service-planen som du ska välja eller skapa nedan. Som du kanske kommer ihåg påminner en App Service-plan om en virtuell dator, som är ett operativsystem med alla resurser (processor, RAM-minne osv.) som behövs på datorn för att köra ditt program. I det här fallet vill din kund använda en Windows-dator som värd för webbappen. Därför väljer du **Windows**.

    1. **Application Insights**: Azure Application Insights hjälper dig att identifiera och diagnostisera kvalitetsproblem i dina webbappar och webbtjänster och hjälper dig att förstå vad användarna faktiskt gör med dem. Ett av din kunds krav är att få rapporter om trafiken på webbplatsen för att identifiera trender, t.ex. när trafiken är hög och när den är låg. I det här fallet väljer du alternativet **På** för att aktivera Application Insights för webbappen. När du har valt alternativet **På** måste du också välja platsen eller regionen där Application Insights-data ska lagras. Observera att Application Insights endast är tillgängligt i ett begränsat antal regioner. I den här demonstrationen väljer du regionen **USA, östra** i fältet **Application Insights-plats**.

## <a name="create-a-new-resource-group"></a>Skapa en ny resursgrupp

En Azure-webbapp måste vara en del av en resursgrupp. Nu ska vi skapa en resursgrupp för att gruppera alla relaterade resurser.

Välj alternativet **Skapa ny** (valt som standard). Observera att Azure redan har skapat ett unikt resursgruppsnamn baserat på det **appnamn** som du har valt. Du kan ändra resursgruppens namn eller behålla det som Azure har genererat. I det här fallet behåller du det namn som genererats av Azure.

## <a name="create-an-app-service-plan"></a>Skapa en App Service-plan

I det här fältet måste du välja en App Service-plan för att köra ditt program. Som standard väljer portalen den senaste App Service-plan du tidigare skapat. Klicka på **>**-tecknet för att gå till bladet **App Service-plan**.

![Bladet App Service-plan](../media-draft/3-create-app-service-plan.PNG)

Klicka på länken **Skapa ny** för att skapa en ny App Service-plan. Bladet **Ny App Service-plan** öppnas på portalen. Portalen behöver en del information av dig för att kunna skapa den nya App Service-planen.

![Bladet Ny App Service-plan](../media-draft/3-new-app-service-plan.PNG)

1. **App Service-plan**: I det här fältet anger du ett unikt namn för den nya App Service-planen. Skriv det webbappsnamn som du valde ovan och lägg till suffixet `-app-service-plan` så att du lätt kan skilja den här resursen från andra. Även här kontrollerar och verifierar Azure Portal att App Service-planens namn är ledigt bland alla App Service-planer som finns i Azure.

1. **Plats**: I det här fältet måste du välja den **region** där App Service-planen finns. Det vill säga, välj den geografiska platsen där App Service-planen ska konfigurera virtuella datorer som krävs för att köra ditt program. I det här fallet kan du välja valfritt alternativ i listan. Din kund föredrar att ha värdservrarna någonstans nära USA:s östkust, där merparten av kundens klienter finns. Därför väljer du **USA, östra** som värde.

1. **Prisnivå**: I det här fältet måste du välja storleken på den virtuella datorn som ska vara värd för ditt program. Klicka på **>**-tecknet för att öppna bladet **Prisnivå**.

    ![Bladet Prisnivå](../media-draft/3-pricing-tier-blade.PNG)

    Här har du många alternativ att välja mellan. Portalen grupperar alternativen baserat på den arbetsbelastningsnivå som krävs. De tre tillgängliga arbetsbelastningskategorierna är: Utveckling/testning, Produktion och Isolerad. Välj relevant arbetsbelastningskategori beroende på kraven för programmet som ska köras i Azure. Eftersom vi fortfarande håller på att utveckla `BestBike`-programmet börjar du med den minsta arbetsbelastningskategorin som passar dig. Kom ihåg att ett av kundens krav var att kunna testa nya ändringar i programmet i realtid. Som du ser i de kommande kursdelarna kräver detta att du lägger till **distributionsplatser**. Distributionsplatser är bara tillgängliga för prisnivåer motsvarande minst **S1**. Välj därför prisnivån **S1** under kategorin för **produktionsarbetsbelastningar**. Klicka sedan på **Använd** för att bekräfta prisnivån du valt ovan.

    > Som du kommer att märka i den här modulen kan du endast lägga till **distributionsplatser** till din webbapp med kategorierna **Produktion** och **Isolerad**.

Nu är du tillbaka på bladet **Ny App Service-plan**.

Klicka på **OK**.

Du kommer tillbaka till huvudbladet **Skapa webbapp**.

![Skapa ny webbapp med data](../media-draft/3-new-web-app-with-data.PNG)

Klicka på knappen **Skapa** för att börja skapa webbappen.

> Vanligtvis tar det några sekunder för Azure Portal att skapa webbappen så att den är redo att användas.

Du omdirigeras till sidan Instrumentpanel och meddelas när den nya webbappen har skapats.

När appen är klar letar du upp och klickar på menyn **Alla resurser** till vänster på instrumentpanelen. Bladet **Alla resurser** visar en lista över alla resurser som du har skapat på Azure-portalen.

Leta upp och klicka på webbappen som du precis har skapat. Du kommer till bladet Webbapp.

![Appen bilal-webapp](../media-draft/3-web-app.PNG)

Startsidan för webbappen öppnas på portalen.

![Startsidan för bilal-webapp](../media-draft/3-web-app-home.PNG)

Klicka på **webbadressen** längst upp till höger i Azure Portal. Om en sida liknande den nedan visas, betyder det att webbappen har skapats korrekt.

![Standardstartsida för webbapp](../media-draft/3-default-home-page.PNG)

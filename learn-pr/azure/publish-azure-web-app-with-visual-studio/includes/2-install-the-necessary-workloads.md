Det första steget för att komma igång med din nya webbplats är att förbereda utvecklingsmiljön. För att skapa och distribuera ASP.NET-webbprogram krävs det att du har de nödvändiga verktygen installerade på den lokala datorn. Här går vi igenom de verktyg du behöver och hur du installerar dem.

## <a name="prepare-your-development-environment"></a>Förbereda utvecklingsmiljön

Visual Studio 2017 har två arbetsbelastningar som du behöver för att skapa, publicera och distribuera din webbplats till Azure. De här arbetsbelastningarna innehåller alla mallar för din ASP.NET-webbplats och ger möjlighet att ansluta och distribuera din webbplats till Azure.

Du behöver se till att du har följande arbetsbelastningar installerade:

- ASP.NET och webbutveckling

Arbetsbelastningen för webbutveckling i Visual Studio 2017 är utformad för att maximera din produktivitet för utveckling av webbprogram med hjälp av ASP.NET och standardbaserade tekniker som HTML och JavaScript.

- Azure development (Azure-utveckling)

Arbetsbelastningen Azure development (Azure-utveckling) i Visual Studio 2017 installerar den senaste Azure SDK:n för .NET och verktyg för Visual Studio. När de har installerats kan du visa resurser i Cloud Explorer, skapa resurser med hjälp av Azure Resource Manager-verktyg, skapa program för Azure Web och Cloud Services samt utföra åtgärder för stordata med hjälp av Azure Data Lake-verktyg.

## <a name="how-to-install-the-required-workloads"></a>Så installerar du de nödvändiga arbetsbelastningarna

Du kommer att använda installationsprogrammet för Visual Studio för att ändra de komponenter som installerats som en del av Visual Studio.

- Starta installationsprogrammet från Start-menyn i Windows genom att rulla ned till **V** och sedan klicka på **Visual Studio Installer** (installationsprogrammet). Alternativt kan du skriva ```Visual Studio Installer``` medan Start-menyn är öppen för att få fram länken till installationsprogrammet. Tryck sedan på **Retur**.

- Fönstret för Visual Studio-installationsprogrammet visas. Klicka på knappen **Ändra**. Om den inte syns väljer du **Ändra** under listrutan **Mer**.

    ![Ändra Visual Studio](../media-draft/3-visual-studio-installer-modify.PNG)

- Se till att arbetsbelastningarna **ASP.NET and web development** (ASP.NET och webbutveckling) och **Azure development** (Azure-utveckling) är markerade under avsnittet **Web & Cloud** (Webb och moln) på fliken **Workloads** (Arbetsbelastningar).   ![Installera arbetsbelastningar](../media-draft/2-select-workloads.png)

Klicka sedan på knappen **Ändra** längst ned till höger i installationsprogrammet. Visual Studio-installationsprogrammet laddar ned och installerar de nödvändiga komponenterna. Du är nu redo att skapa en ASP.NET-webbapp och ladda upp den till Microsoft Azure.

> [!IMPORTANT]
> Visual Studio för Mac _bör_ ha de nödvändiga arbetsbelastningarna förinstallerade. Om du måste installera om dem behöver du ladda ned [Visual Studio för Mac](https://visualstudio.microsoft.com/thank-you-downloading-visual-studio-mac/?sku=communitymac&rel=15_), vilket kör installationsprogrammet. Därifrån kan du välja de arbetsbelastningar som du vill lägga till.

## <a name="summary"></a>Sammanfattning

Du kan skapa, hantera och publicera en ASP.NET-webbplats från Visual Studio 2017 med arbetsbelastningarna **ASP.NET and web development** och **Azure development**

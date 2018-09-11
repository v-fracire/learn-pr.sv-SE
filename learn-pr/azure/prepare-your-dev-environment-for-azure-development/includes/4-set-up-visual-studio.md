Visual Studio är en komplett och omfattande IDE-miljö (Integrated Development Environment) för alla slags programvaruexperter. Visual Studio har en fullständig uppsättning verktyg och funktioner som är specifikt utformade för apputveckling med Microsoft Azure. Integreringen i Visual Studio garanterar att distributions-, felsöknings- och utvecklingsverktygen för Azure är av högsta klass. Och detsamma gäller Visual Studio för Mac. I den här delen tittar vi närmare på båda versionerna och hur de hjälper dig att utveckla program i Azure.

## <a name="visual-studio"></a>Visual Studio

Visual Studio är en komplett IDE-miljö (Integrated Development Environment) som hjälper dig att utveckla program för Windows, Android, iOS, webben och Azure.

När du installerar Visual Studio kan du välja mellan flera arbetsbelastningar. Arbetsbelastningar är samlingar med bibliotek och komponenter som definierar ett funktionsområde som kan installeras. I stället för att installera enskilda komponenter separat, vilket kräver att du känner till och kommer ihåg alla beroenden mellan komponenterna, kan du använda arbetsbelastningar för att utföra ”områdesspecifika” installationer. Detta säkerställer att alla nödvändiga komponenter ingår.

Basinstallationen av Visual Studio innehåller inga verktyg eller bibliotek för Azure-utveckling. För detta måste du lägga till arbetsbelastningen för Azure-utveckling. Den innehåller Azure SDK:er, verktyg och mallprojekt som hjälper dig att komma igång med att skapa program och upplevelser i Azure.

Hämta installationsprogrammet för att installera Visual Studio. Installationsprogrammet uppmanar dig att ange vilka arbetsbelastningar som ska installeras. Välj arbetsbelastningen för Azure-utveckling. Om ytterligare funktioner krävs är dessa troligen tillgängliga via NuGet eller ett Visual Studio-tillägg.

## <a name="visual-studio-for-mac"></a>Visual Studio för Mac

Visual Studio för Mac är en internt utformad och utvecklad IDE för macOS. Det ger en förstklassig upplevelse där utvecklare kan skapa program för mobilappar för Android och iOS, webben och .NET Core-lösningar. Det är även perfekt för att skapa program i Azure.

Basinstallationen av Visual Studio för Mac innehåller kontextbaserad integration av Azure-verktyg. Om du till exempel skapar en Xamarin-app för Android, så tillhandahåller arbetsbelastningen Connected Services en länk för att skapa en serverdel för mobilappar med Azure App Service. Om du vill skapa en Azure-funktion, så är det en projektmall under kategorin Moln.

Om du behöver verktyg för Azure-funktioner som inte ingår i basinstallationen använder du NuGet. NuGet-pakethanteraren har en mängd Azure-paket som utökar funktionerna och verktygen i Visual Studio för Mac.

Ladda ned installationsprogrammet för att installera Visual Studio för Mac. Installationsprogrammet inspekterar ditt system för att avgöra vilka komponenter som behövs eller som måste uppdateras. Du kan anpassa komponenterna som ska installeras från de som saknas på datorn. Basinstallationen innehåller Azure-verktyg. Om ytterligare funktioner krävs är de troligen tillgängliga via NuGet eller ett Visual Studio för Mac-tillägg.

> [!NOTE]
> Du kan uppmanas att ange administratörsautentiseringsuppgifter på datorn för att installera vissa komponenter.

## <a name="summary"></a>Sammanfattning

I den här delen har du installerat Visual Studio för Windows eller på macOS.

För Windows valde du arbetsbelastningen för Azure-utveckling i installationsprogrammet, som installerade alla nödvändiga verktyg som krävs för att skapa Azure-program och generera Azure-resurser. Du kan sedan komma åt alla Azure-resurser för din prenumeration  via utforskarverktyg eller via resursreferenser.

I Visual Studio för Mac är vissa Azure-verktyg redan inbyggda i basinstallationen, och många fler funktioner är tillgängliga via NuGet. Detta ger även åtkomst till resurser och tjänster i din Azure-prenumeration.

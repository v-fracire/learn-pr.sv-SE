Visual Studio är en fullt utrustad integrerad utvecklingsmiljö (IDE) för en mängd olika programmeringstyper och språk. Visual Studio innehåller en fullt utrustad uppsättning verktyg och funktioner som är specifikt utformade för programutveckling i Microsoft Azure. Det innebär att Azures verktyg för utveckling, felsökning och distribution är nära integrerad med IDE.

## <a name="visual-studio-2017"></a>Visual Studio 2017

Visual Studio 2017 är en fullt utrustad IDE-miljö som används för att utveckla program för en mängd olika programtyper, inklusive Windows, Android, iOS, webben och Azure.

När du installerar Visual Studio kan du välja mellan flera *arbetsbelastningar*. Arbetsbelastningar är samlingar med bibliotek och komponenter som definierar ett funktionsområde som kan installeras. I stället för att installera en enskild komponent, vilket kräver att du känner till och kommer ihåg alla beroenden mellan komponenterna, kan du använda arbetsbelastningar för att utföra ”områdesspecifika” installationer. Detta säkerställer att alla nödvändiga komponenter ingår.

Basinstallationen av Visual Studio innehåller inga verktyg eller bibliotek för Azure-utveckling. För detta måste du lägga till arbetsbelastningen för Azure-utveckling, som innehåller Azure SDK:er, verktyg och mallprojekt.

[Hämta installationsprogrammet](https://visualstudio.microsoft.com/) för att installera Visual Studio. Installationsprogrammet uppmanar dig att ange vilka arbetsbelastningar som ska installeras. Välj arbetsbelastningen för Azure-utveckling. Om ytterligare funktioner krävs läggs dessa vanligtvis till via NuGet-paket eller Visual Studio-tillägget.

## <a name="visual-studio-for-mac"></a>Visual Studio för Mac

Visual Studio för Mac är en internt utformad och utvecklad IDE för macOS. Du kan skapa lösningar för mobilappar på Android och iOS, webben och .NET Core.

Basinstallationen av Visual Studio för Mac innehåller sammanhangsbaserad integration av Azure-verktyg. Om du till exempel skapar en Xamarin-app för Android tillhandahåller den inkluderade Connected Services-mallen en länk för att skapa en serverdel för mobilappar med Azure App Service. Om du vill skapa en Azure-funktion finns det en projektmall under kategorin Moln.

Om du behöver verktyg för Azure-funktioner och funktioner som inte finns i basinstallationen behöver du troligen lägga till NuGet-paket eller ett Visual Studio för Mac-tillägg.

[Hämta installationsprogrammet](https://visualstudio.microsoft.com/) för att installera Visual Studio för Mac. Installationsprogrammet inspekterar ditt system för att avgöra vilka komponenter som behövs eller som måste uppdateras.

> [!NOTE]
> Du kan uppmanas att ange administratörsautentiseringsuppgifter på datorn för att installera vissa komponenter.
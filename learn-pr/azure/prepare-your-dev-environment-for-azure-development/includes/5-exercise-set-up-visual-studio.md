Här ska du installera Visual Studio på antingen en Windows- eller macOS-dator.

## <a name="exercise-steps"></a>Övningssteg

::: zone pivot="windows"

### <a name="windows"></a>Windows

1. Hämta installationsprogrammet för Visual Studio från https://visualstudio.microsoft.com/downloads/.

1. Kör installationsprogrammet.

1. I fliken **Arbetsbelastning**, välj arbetsbelastningen **Azure- utveckling**.

    Följande skärmbild visar den arbetsbelastning för installationsprogrammet för Visual Studio som har valts för Azure-utveckling i Visual Studio.

    ![Skärmbild av installationsprogrammet för Visual Studio med arbetsbelastningen Azure Development markerad.](../media/5-select-azure-workload.png)

1. (Valfritt) Installera utvecklingsarbetsbelastningar för ASP.NET och webbutveckling för att kunna skapa webbprogram för Azure.

1. Klicka på **Installera** och vänta tills Visual Studio är installerat. För system där Visual Studio redan har installerats kan det stå **Ändra** på den här knappen.

1. Öppna Visual Studio när installationen är klar.

1. Gå till menyn Visa i Visual Studio och kontrollera att du har alternativet **Cloud Explorer**.

    Följande skärmbild visar menyalternativet Cloud Explorer som är tillgängligt om du har installerat arbetsbelastningen Azure Development.

    ![Skärmbild av menyn Visa i Visual Studio med menyalternativet Cloud Explorer markerat.](../media/5-verify-cloud-explorer.png)

::: zone-end

::: zone pivot="macos"

### <a name="macos"></a>macOS

1. Gå till https://visualstudio.microsoft.com/ och hämta installationsprogrammet för att installera Visual Studio för Mac.

1. Klicka på VisualStudioInstaller.dmg-filen för att montera installationsprogrammet och kör det sedan genom att dubbelklicka på logotypen.

1. Bekräfta sekretess- och licensvillkoren när de visas.

1. Installationsprogrammet kommer fråga vilka komponenter du vill installera. Azure-komponenter ingår redan i Visual Studio för Mac, men du rekommenderas att installera **.NET Core**-plattformen för att utveckla webbfunktioner för Azure.

    Följande skärmbild visar den .NET Core-plattform som krävs för att lägga till funktioner för Azure-utveckling i Visual Studio för Mac.

    ![Skärmbild av installationsprogrammet för Visual Studio för Mac med det valda plattformsalternativet .NET Core markerat.](../media/5-vsmac-install-net-core.png)

1. Klicka på **Installera och uppdatera** när du är nöjd med dina val och vänta tills installationsprogrammet är klart.

1. Om du uppmanas höja behörighetsnivån kan du använda dina administratörsautentiseringsuppgifter för att göra detta.

1. När installationsprogrammet är klart kan du starta Visual Studio för Mac.

::: zone-end
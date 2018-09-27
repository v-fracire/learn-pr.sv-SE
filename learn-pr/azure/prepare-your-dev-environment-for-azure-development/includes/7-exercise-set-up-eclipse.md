Här installerar du Eclipse och Azure Toolkit på en utvecklingsdator. I slutet av övningen har du allt du behöver för att skapa ett Java-program som är anslutet till Azure.

## <a name="install-eclipse-ide"></a>Installera Eclipse IDE

1. Ladda ned lämplig [Eclipse IDE för ditt operativsystem](https://www.eclipse.org/downloads/packages/installer).

1. Starta installationsprogrammet för Eclipse när det har hämtats.

    1. I Windows dubbelklickar du på den nedladdade filen.

    1. I macOS och Linux packar du upp installationsprogrammet från den nedladdade filen och kör det.

        > [!NOTE]
        > Du kan uppmanas att installera Java Development Kit om det saknas.

1. Välj de paket som ska installeras. Om du är Java-utvecklare väljer du alternativet Java eller Java EE Eclipse IDE.

1. Välj installationsmålet på din dator.

1. Starta Eclipse för att kontrollera att det har installerats korrekt.

## <a name="install-azure-toolkit-for-eclipse"></a>Installera Azure Toolkit for Eclipse

Azure Toolkit installeras på samma sätt i Windows, macOS och Linux.

1. Starta Eclipse.

1. Gå till **Hjälp** > **Installera ny programvara...**.

    På följande skärmbild visas menyplatsen för alternativet **Installera ny programvara...**.

    ![Skärmbild av alternativet Installera ny programvara markerat på Hjälp-menyn i Eclipse.](../media/7-eclipse-install-new-software.png)

1. Dialogrutan **Tillgänglig programvara** öppnas. Gå till textrutan **Arbeta med:**, skriv `http://dl.microsoft.com/eclipse/` och tryck på Retur.

1. Markera alternativet **Azure Toolkit för Java** i resultatet. Avmarkera alternativet **Kontakta alla uppdateringsplatser under installationen för att hitta nödvändig programvara** om det inte redan är avmarkerat.

    Följande skärmbild visar installationskonfigurationen **Tillgänglig programvara** som beskrivs ovan.

    ![Skärmbild av fönstret Tillgänglig programvara i Eclipse med rutor som visar den konfigurationen som krävs för att hitta och installera Azure Toolkit för Java.](../media/7-eclipse-download-azure-toolkit-for-java.png)

1. Klicka på **Nästa**.

1. Granska och acceptera licensavtalet när du uppmanas att göra det och klicka sedan på **Slutför**.

1. Eclipse laddar ned och installerar Azure Toolkit.

1. Starta om Eclipse om det behövs.

1. Verifiera installationen av Azure Toolkit genom att kontrollera att menyalternativet **Verktyg** > **Azure** visas i Eclipse.

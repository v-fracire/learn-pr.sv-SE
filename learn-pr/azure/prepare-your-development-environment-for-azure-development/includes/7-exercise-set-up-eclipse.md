I den här övningen ska du installera Eclipse på din lokala dator och sedan installera Azure Toolkit så att du kan börja utveckla Java-program med Azure-integrering. Installationen går snabbt och enkelt. I slutet av den här övningen har du allt som krävs för att skapa ditt första Java-program och dra nytta av funktionerna och tjänsterna i Microsoft Azure.

## <a name="install-eclipse-ide"></a>Installera Eclipse IDE

1. Ladda ned Eclipse-versionen för ditt operativsystem från http://www.eclipse.org/downloads/packages/installer.

    ![Gå till nedladdningsplatsen för installationsprogrammet på eclipse.org och välj installationsprogrammet för ditt operativsystem](../media-draft/7-go-to-eclipse-org.png)

1. Starta installationsprogrammet för Eclipse när det har laddats ned.
    1. I Windows dubbelklickar du på den nedladdade filen.
    1. I macOS och Linux packar du upp installationsprogrammet från den nedladdade filen. Starta sedan installationsprogrammet när det har packats upp.

        > [!NOTE]
        > Du kan uppmanas att installera Java Development Kit om det saknas.

3. Välj de paket som ska installeras. Om du är Java-utvecklare väljer du alternativet Java eller Java EE Eclipse IDE.
4. Välj installationsmålet på din dator.
5. Starta Eclipse för att bekräfta att det har installerats korrekt.

    ![Klicka på knappen Starta för att starta Eclipse efter installationen](../media-draft/7-launch-eclipse-from-installer.png)

## <a name="install-azure-toolkit-for-eclipse"></a>Installera Azure Toolkit for Eclipse

Azure Toolkit installeras på samma sätt i Windows, macOS och Linux.

1. Starta Eclipse.
2. Gå till **Help** (Hjälp)  > **Install New Software** (Installera ny programvara).

    ![Gå till menyalternativet Install New Software (Installera ny programvara) i Eclipse](../media-draft/7-eclipse-install-new-software.png)

3. Dialogrutan **Available Software** (Tillgänglig programvara) öppnas. Skriv `http://dl.microsoft.com/eclipse/` i textrutan **Work with** (Arbeta med) och tryck på Retur.
4. Markera alternativet **Azure Toolkit for Java** (Azure Toolkit för Java) i resultatet. Avmarkera alternativet **Contact all update sites during install to find required software** (Kontakta alla uppdateringsplatser under installationen för att hitta nödvändig programvara) om det inte redan är avmarkerat.

    ![Söka efter och installera Azure Toolkit för Java i Eclipse](../media-draft/7-eclipse-download-azure-toolkit-for-java.png)

5. Klicka på **Nästa**.
6. Granska och acceptera licensavtalet när du uppmanas att göra det och klicka sedan på **Finish** (Slutför).
7. Eclipse laddar ned och installerar Azure Toolkit.
8. Starta om Eclipse om det behövs.
9. Verifiera installationen av Azure Toolkit genom att kontrollera att menyalternativet **Tools** (Verktyg)  > **Azure** visas i Eclipse.

## <a name="summary"></a>Sammanfattning

I den här kursdelen har du installerat Eclipse för Java och förberett det för att dra nytta av integreringen med tjänsterna och produkterna i Microsoft Azure. Installationen är snabb och enkel vilket gör Eclipse perfekt för Java-utveckling med integrering av molntjänster.

Vår virtuella Windows-dator har distribuerats och körs men den har inte konfigurerats att utföra något arbete än.

Som du kanske minns är vårt scenario ett videobearbetningssystem. Vår plattform tar emot filer via FTP. Trafikkamerorna överför videoklipp till en känd URL som mappas till en mapp på servern. Den anpassade programvaran på varje virtuell Windows-dator körs som en tjänst och bevakar mappen och bearbetar varje överfört klipp. Den skickar sedan den normaliserade videon till våra algoritmer som körs på andra Azure-tjänster.

Det finns några saker som vi behöver konfigurera för att det här scenariot ska fungera:

- Installera FTP och öppna de portar som behövs för att kommunicera.
- Installera den tillverkarspecifika video-codecen som är unik för stadens kamerasystem.
- Installera transkodningstjänsten som bearbetar överförda videor.

Många av dessa är typiska administrativa uppgifter som vi inte beskriver här, och vi har inte heller någon programvara att installera. I stället går vi igenom stegen och visar hur du _kan_ installera anpassad programvara eller programvara från tredje part via fjärrskrivbord. Vi börjar med att hämta anslutningsinformationen.

## <a name="connect-to-the-vm-with-remote-desktop-protocol"></a>Ansluta till den virtuella datorn med RDP (Remote Desktop Protocol)

För att ansluta till en virtuell Azure-dator med en RDP-klient behöver du:

- Den virtuella datorns offentliga IP-adress (eller privata om den virtuella datorn har konfigurerats att ansluta till ditt nätverk).
- Portnumret.

Du kan ange den här informationen i RDP-klienten eller hämta en förkonfigurerad **RDP**-fil.

### <a name="download-the-rdp-file"></a>Ladda ned RDP-filen

1. Kontrollera att panelen [Översikt](https://portal.azure.com?azure-portal=true) för den virtuella datorn som du skapade tidigare är öppen på **Azure Portal**. Du hittar den virtuella datorn under **Alla resurser** om du behöver öppna den. Översiktspanelen innehåller mycket information om den virtuella datorn.

    - Du kan se om den virtuella datorn körs.
    - Stoppa eller starta om den.
    - Hämta den offentliga IP-adressen för att ansluta till den virtuella datorn.
    - Visa processor-, disk- och nätverksaktivitet.

1. Klicka på knappen **Anslut** längst upp i fönstret.

1. Skriv ned inställningarna för **IP-adress** och **Portnummer** på bladet **Anslut till virtuell dator**. Klicka sedan på **Ladda ned RDP-fil** och spara filen på datorn.

1. Innan vi ansluter ska vi justera några inställningar. I Windows letar du upp filen med Explorer, högerklickar och väljer **Redigera**. I MacOS måste du först öppna filen med RDP-klienten och sedan högerklicka på objektet i listan som visas och välja **Redigera**.

1. Du kan ändra flera inställningar som styr anslutningen till den virtuella Azure-datorn. De inställningar som är intressanta här är:

    - **Visa**: Standardinställningen är hela skärmen. Du kan ändra detta till en lägre upplösning eller använda alla övervakare om du har fler än en.
    - **Lokala resurser**: Du kan dela lokala enheter med den virtuella datorn, så att du kan kopiera filer från din dator till den virtuella datorn. Klicka på knappen **Mer** under **Lokala enheter och resurser** för att välja vad som ska delas.
    - **Experience** (Upplevelse): Anpassa den visuella upplevelsen baserat på din nätverkskvalitet.

1. Dela din lokala C:-enhet så att det är synlig för den virtuella datorn.

1. Gå tillbaka till fliken **Allmänt** och klicka på **Spara** för att spara ändringarna. Du kan alltid gå tillbaka och redigera den här filen senare om du vill prova andra inställningar.

### <a name="connect-to-the-windows-vm"></a>Ansluta till den virtuella Windows-datorn

1. Klicka på knappen **Anslut** för att ansluta till den virtuella datorn.

1. Notera säkerhetsvarningen och fjärrdatorns IP-adress i dialogrutan **Anslutning till fjärrskrivbord** och klicka sedan på **Anslut**.

1. I dialogrutan **Windows-säkerhet** anger du samma användarnamn och lösenord som i steg 6 och 7.
    
    > [!NOTE]
    > Om du använder en Windows-klient för att ansluta till den virtuella datorn ställs standardinställningen in på kända identiteter på datorn. Du kan klicka på alternativet **Fler alternativ** och välja ”Använd ett annat konto” om du vill ange en annan kombination av användarnamn och lösenord.
    
1. I den andra dialogrutan för **Anslutning till fjärrskrivbord** noterar du certifikatfelen och klickar sedan på **Ja**.

### <a name="install-worker-roles"></a>Installera arbetsroller

Första gången du ansluter till en virtuell Windows-server startas Serverhanteraren. Det gör att du kan tilldela en arbetsroll för vanliga webb- eller datauppgifter. Du kan också starta Serverhanteraren via Start-menyn.

Det är här vi normalt skulle lägga till webbserverrollen till servern. Det gör att IIS installeras, och som en del av konfigurationen skulle du inaktivera HTTP-begäranden och aktivera FTP-servern. Vi skulle också kunna ignorera IIS och installera en FTP-server från en tredje part. Därefter skulle vi konfigurera FTP-servern att tillåta åtkomst till en mapp på Big Data-enheten som vi lade till på den virtuella datorn.

Eftersom vi inte ska konfigurera detta här kan du stänga Serverhanteraren.

## <a name="install-custom-software"></a>Installera anpassad programvara

Vi kan använda någon av två metoder för att installera programvara. Den här virtuella datorn är ansluten till Internet. Om programvaran som du behöver har ett nedladdningsbart installationsprogram kan du öppna en webbläsare i RDP-sessionen, ladda ned programvaran och installera den. Om programvaran är anpassad, som vår anpassade tjänst, kan du kopiera den från din lokala dator till den virtuella datorn för att installera den. Låt oss titta närmare på den sista metoden.

1. Öppna Utforskaren. Klicka på **Den här datorn** på sidopanelen. Du bör se flera enheter:

    - En Windows-enhet (C:) som representerar operativsystemet.
    - En enhet för tillfällig lagring (D:).
    - Din lokala C:-enhet (den har ett annat namn än det som visas nedan).

    ![En lokal enhet som delas med Azure](../media-drafts/6-drive-list.png)

Om du har åtkomst till din lokala enhet kan du kopiera dessa filer för den anpassade programvaran till den virtuella datorn och installera programvaran. Vi ska inte gör det här eftersom detta bara är ett simulerat scenario, men du kan föreställa dig hur det skulle fungera.

Mer intressant att observera i listan över enheter är vad som _saknas_. Observera att enheten **Data** inte visas. Azure lade till en virtuell hårddisk (VHD) men initierade den inte.

## <a name="initialize-data-disks"></a>Initiera datadiskar

Alla ytterligare enheter som du skapar från början måste initieras och formateras. Processen för att gör detta är identisk med en fysisk enhet.

1. Starta verktyget **Diskhantering** från Start-menyn.

1. En varning visas som anger att en oinitierad disk har identifierats.

    ![Initiera datadisken på den virtuella datorn](../media-drafts/6-disk-management.png)

1. Klicka på **OK** för att initiera disken. Nu visas disken i listan med volymer där du kan formatera den och tilldela en enhetsbeteckning.

1. Öppna Utforskaren så bör du se dataenheten.

1. Gå vidare och stäng RDP-klienten för att logga ut från den virtuella datorn. Servern fortsätter att köras.

Med RDP kan du arbeta med den virtuella Azure-datorn på samma sätt som en lokal dator. Med åtkomst till skrivbordsgränssnittet kan du administrera den här virtuella datorn på samma sätt som en vanlig Windows-dator och till exempel installera programvara, konfigurera roller, justera funktioner och utföra andra vanliga aktiviteter. Men det är en manuell process. Om vissa program alltid behöver installeras kan du automatisera processen med hjälp av skript.
Vår virtuella Windows-dator har distribuerats och körs men den har inte konfigurerats att utföra arbete.

Som du kanske minns är vårt scenario ett videobearbetningssystem. Vår plattform tar emot filer via FTP. Trafikkamerorna överför videoklipp till en känd URL som mappas till en mapp på servern. Den anpassade programvaran på varje virtuell Windows-dator körs som en tjänst och bevakar mappen och bearbetar varje överfört klipp. Den skickar sedan den normaliserade videon till våra algoritmer som körs på andra Azure-tjänster.

Det finns några saker som vi behöver konfigurera för att det här scenariot ska fungera:

- Installera FTP och öppna de portar som behövs för att kommunicera.
- Installera den tillverkarspecifika video-codecen som är unik för den stad kamerasystem.
- Installera transkodningstjänsten som bearbetar överförda videor.

Många av dessa är vanliga administrativa uppgifter som vi inte går igenom här, och vi har ingen programvara att installera. I stället går vi igenom stegen och visar dig hur du _kan_ installera anpassad programvara eller programvara från tredje part med hjälp av fjärrskrivbord. Vi börjar med att hämta anslutningsinformationen.

## <a name="connect-to-the-vm-with-remote-desktop-protocol"></a>Ansluta till den virtuella datorn med Remote Desktop Protocol

För att ansluta till en virtuell Azure-dator med en RDP-klient behöver du:

- Den offentliga IP-adressen för den virtuella datorn (eller privat om den virtuella datorn är konfigurerad för att ansluta till nätverket).
- Portnumret.

Du kan ange den här informationen i RDP-klienten eller hämta en förkonfigurerad **RDP**-fil.

### <a name="download-the-rdp-file"></a>Ladda ned RDP-filen

1. Se till att [översiktspanelen](https://portal.azure.com?azure-portal=true) för den virtuella dator du skapade tidigare är öppen i **Azure Portal**. Du hittar den virtuella datorn under **Alla resurser** om du behöver öppna den. Översiktspanelen har mycket information om den virtuella datorn.

    - Du kan se om den virtuella datorn körs.
    - Stoppa eller starta om den.
    - Hämta den offentliga IP-adressen för att ansluta till den virtuella datorn.
    - Visa aktiviteten för CPU, disk och nätverk.

1. Klicka på knappen **Anslut** högst upp i fönstret.

1. På bladet **Anslut till virtuell dator** ska du notera inställningarna för **IP-adress** och **Portnummer**. Klicka sedan på **Ladda ned RDP-fil** och spara filen på datorn.

1. Innan vi ansluter ska vi justera några inställningar. I Windows letar du upp filen med Explorer, högerklickar och väljer **Redigera**. I MacOS måste du först öppna filen med RDP-klienten och sedan högerklicka på objektet i listan som visas och välja **Redigera**.

1. Du kan justera flera inställningar för anslutningen till den virtuella Azure-datorn. De inställningar som är intressanta här är:

    - **Visa**: Standardinställningen är hela skärmen. Du kan ändra detta till en lägre upplösning eller använda alla övervakare om du har fler än en.
    - **Lokala resurser**: Du kan dela lokala enheter med den virtuella datorn, så att du kan kopiera filer från din dator till den virtuella datorn. Klicka på knappen **Mer** under **Lokala enheter och resurser** för att välja vad som ska delas.
    - **Experience** (Upplevelse): Justera den visuella upplevelsen baserat på din nätverkskvalitet.

1. Dela din lokala C:-enhet så att det är synlig för den virtuella datorn.

1. Gå tillbaka till fliken **Allmänt** och klicka på **Spara** för att spara ändringarna. Du kan alltid gå tillbaka och redigera den här filen senare om du vill prova andra inställningar.

### <a name="connect-to-the-windows-vm"></a>Ansluta till den virtuella Windows-datorn

1. Klicka på knappen **Anslut** för att ansluta till den virtuella datorn.

1. I dialogrutan **Anslutning till fjärrskrivbord** noterar du säkerhetsvarningen och fjärrdatorns IP-adress och klickar sedan på **Anslut**.

1. I dialogrutan **Windows-säkerhet** anger du samma användarnamn och lösenord som i steg 6 och 7.
    
    > [!NOTE]
    > Om du använder en Windows-klient för att ansluta till den virtuella datorn ställs standardinställningen in på kända identiteter på datorn. Du kan klicka på alternativet **Fler alternativ** och välja ”Använd ett annat konto” om du vill ange en annan kombination av användarnamn och lösenord.
    
1. I den andra dialogrutan **Anslutning till fjärrskrivbord** noterar du certifikatfelen och klickar sedan på **Ja**.

### <a name="install-worker-roles"></a>Installera arbetsroller

Första gången du ansluter till en virtuell Windows-server startas Serverhanteraren. Det gör att du kan tilldela en arbetsroll för vanliga webb- eller datauppgifter. Du kan också starta Serverhanteraren via Start-menyn.

Det är här som vi normalt lägger till webbserverrollen på servern. När du gör det installeras IIS, och som en del av konfigurationen inaktiverar du HTTP-begäranden och aktiverar FTP-servern. Du kan också välja att ignorera IIS och installera en FTP-server från en tredje part. Därefter konfigurerar du FTP-servern att tillåta åtkomst till en mapp på Big Data-enheten som du har lagt till på den virtuella datorn.

Eftersom vi inte ska konfigurera detta här kan du stänga Serverhanteraren.

## <a name="install-custom-software"></a>Installera anpassad programvara

Vi kan använda någon av två metoder för att installera programvara. Den här virtuella datorn är ansluten till Internet. Om programvaran som du behöver har ett nedladdningsbart installationsprogram kan du öppna en webbläsare i RDP-sessionen, ladda ned programvaran och installera den. Om programvaran är anpassad, som vår anpassade tjänst, kan du kopiera den från din lokala dator till den virtuella datorn för att installera den. Låt oss titta närmare på den sista metoden.

1. Öppna Utforskaren. Klicka på **Den här datorn** på sidopanelen. Du bör se flera enheter:

    - Windows-enheten (C:) som representerar operativsystemet.
    - Enheten för temporär lagring (D:).
    - Din lokala C:-enhet (den har ett annat namn än det som visas nedan).

    ![Lokal enhet som delas med Azure](../media-drafts/6-drive-list.png)

Om du har åtkomst till din lokala enhet kan du kopiera dessa filer för den anpassade programvaran till den virtuella datorn och installera programvaran. Vi ska inte gör det här eftersom detta bara är ett simulerat scenario, men du kan föreställa dig hur det skulle fungera.

Mer intressant att observera i listan över enheter är vad som _saknas_. Observera att **dataenhet** inte visas. Azure har lagt till en VHD men har inte initierat den.

## <a name="initialize-data-disks"></a>Initiera datadiskar

Alla ytterligare enheter du skapar från början måste initieras och formateras. Processen för att gör detta är identisk med en fysisk enhet.

1. Starta verktyget **Diskhantering** från Start-menyn.

1. En varning visas som anger att en oinitierad disk har identifierats.

    ![Initiera datadisken på den virtuella datorn](../media-drafts/6-disk-management.png)

1. Klicka på **OK** för att initiera disken. Då visas disken i listan med volymer där du kan formatera den och tilldela en enhetsbeteckning.

1. Öppna Utforskaren så ser du din dataenhet.

1. Gå vidare och stäng RDP-klienten för att logga ut från den virtuella datorn. Servern fortsätter att köras.

Med RDP kan du arbeta med den virtuella Azure-datorn på samma sätt som en lokal dator. Med åtkomst till skrivbordsgränssnittet kan du administrera den här virtuella datorn på samma sätt som en vanlig Windows-dator och till exempel installera programvara, konfigurera roller, justera funktioner och utföra andra vanliga aktiviteter. Men det är en manuell process. Om vissa program alltid behöver installeras kan du automatisera processen med hjälp av skript.
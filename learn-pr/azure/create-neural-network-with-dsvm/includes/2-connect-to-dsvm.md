### <a name="connect-to-the-data-science-vm"></a>Ansluta till din Data Science VM

I den här enheten får du fjärransluta till Ubuntu-skrivbordet på den virtuella dator som du skapade i den föregående övningen. För att kunna göra detta behöver du en klient med stöd för [Xfce](https://xfce.org/), en enkel skrivbordsmiljö för Linux. Om du behöver mer bakgrundsinformation och en översikt över hur du ansluter till en Data Science Virtual Machine-dator (DVM) kan du läsa [How to access the Data Science Virtual Machine for Linux](https://docs.microsoft.com/azure/machine-learning/data-science-virtual-machine/dsvm-ubuntu-intro#how-to-access-the-data-science-virtual-machine-for-linux) (Så här kommer du åt Data Science Virtual Machine för Linux).

1. Om du inte redan har en Xfce-klienten installerad bör du ladda ned [X2Go-klienten](https://wiki.x2go.org/doku.php/download:start) och installera den innan du fortsätter med den här övningen. X2Go är en kostnadsfri och Xfce-lösning med öppen källkod som fungerar på en rad olika operativsystem, inklusive Windows och OS X. Anvisningarna i den här enheten förutsätter att du använder X2Go, men du kan också använda någon annan klient som stöder Xfce.

1. Gå tillbaka till resursgruppen **data-science-rg** i Azure Portal. Klicka på resursen **data-science-vm** för att öppna den i portalen.

    ![Öppna en Data Science VM](../media/2-open-data-science-vm.png)

1. Hovra över IP-adressen som visas för den virtuella datorn och klicka på knappen **Kopiera** för att kopiera IP-adressen till Urklipp.

    ![Kopiera den virtuella datorns IP-adress](../media/2-copy-ip-address.png)

1. Starta X2Go-klienten och anslut till Data Science VM med hjälp av den IP-adress som finns i Urklipp och det användarnamn som du angav i föregående övning. Anslut via port **22** (standardport för SSH-anslutningar) och ange **XFCE** som sessionstyp. Klicka på knappen **OK** för att bekräfta inställningarna.

    ![Ansluta med X2Go](../media/2-new-session-1.png)

1. I panelen New session (Ny session) till höger väljer du den upplösning som du vill använda vid anslutning till fjärrskrivbordet. Klicka sedan på **New session** (Ny session) överst i panelen.

    ![Starta en ny session](../media/2-new-session-2.png)

1. Ange lösenordet som du skapade i den senaste enheten och klicka sedan på knappen **OK**. Om du får frågan om du litar på serverns värdnyckel svarar du **Ja**. Ignorera eventuella felmeddelanden som säger att SSH-daemon inte kunde startas.

    ![Logga in på den virtuella datorn](../media/2-new-session-3.png)

1. Vänta tills fjärrskrivbord visas och kontrollera att det liknar det nedan.

    > Om texten och ikonerna på skrivbordet är för stora bör du avsluta sessionen. Klicka på ikonen i det nedre högra hörnet av panelen New session (Ny session) och välj **Session preferences...** (Sessionsinställningar ...) i menyn. Gå till fliken Input/Output (Indata/utdata) i dialogrutan New session (Ny session), justera bildskärmens upplösning (dpi) och starta sedan en ny session. Börja med 96 dpi, samt och justera efter behov.

    ![Anslutning upprättad!](../media/2-ubuntu-desktop.png)

Nu när du är ansluten kan du utforska genvägarna på skrivbordet. Det här är genvägar till ett antal datavetenskapsverktyg som finns förinstallerade i den virtuella datorn, bland annat [Jupyter](http://jupyter.org/), [R Studio](https://www.rstudio.com/) och [Microsoft Azure Storage Explorer](https://azure.microsoft.com/features/storage-explorer/).
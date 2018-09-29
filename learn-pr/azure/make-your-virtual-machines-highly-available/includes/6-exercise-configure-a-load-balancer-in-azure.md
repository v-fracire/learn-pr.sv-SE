Du har redan skapat de resurser som är viktigast för din onlinebankarkitektur. I den här övningen ska du slutföra installationen genom att konfigurera regler för lastbalanseraren.

När installationen är klar ska du testa belastningsutjämnaren genom att ta bort en virtuell dator från poolen och verifiera att klientbegäranden inte längre skickas till den virtuella datorn.

Vi börjar genom att definiera vår serverdelspool i lastbalanseraren. Detta avgör var inkommande begäranden dirigeras.

## <a name="create-a-backend-address-pool"></a>Skapa en serverdelsadresspool

1. I menyn till vänster i[Azure-portalen](https://portal.azure.com/learn.docs.microsoft.com?azure-portal=true), välj **Alla resurser**. Välj sedan lastbalanseraren i resurslistan (**woodgrove-LB**).

1. Välj **Inställningar** > **serverdelspooler**. Observera att vi inte har några definierade ännu.

1. Lägg till en ny serverdelspool genom att klicka på **Lägg till**.

    ![Skärmbilden visar hur man lägger till en serverdelspool.](../media/6-backend-pools.png)

1. Lägg till följande information på **Lägg till serverdelspool**-bladet:
    - Ge den namnet _woodgrove BEP_.
    - Associerade poolen till en **Tillgänglighetsuppsättning**.
    - För Tillgänglighetsuppsättningen, välj _woodgrove-AS_.

1. Klicka på **Lägg till en IP-konfiguration för målnätverk**.

1. I listan **Virtuell måldator**, välj _woodgrove-VM1_.

1. I listan **Nätverkets IP-konfiguration** väljer du den virtuella datorns IP-adresspool.

1. Upprepa dessa steg för att lägga till _woodgrove-VM2_ och _woodgrove-VM3_ till serverdelspoolen.

    ![Skärmbilden visar bladet Lägg till serverdelspool med alla tre virtuella datorer som valts.](../media/6-add-backend-pool.png)

1. Klicka på **OK** för att stänga bladet.

Vänta tills belastningsutjämningskonfigurationen har uppdaterats innan du fortsätter med den här övningen.

## <a name="create-a-health-probe-for-the-load-balancer"></a>Skapa en hälsoavsökning för lastbalanseraren

Lägg sedan till en hälsoavsökning för HTTP via port 80.

1. På bladet **woodgrove-LB** under **Inställningar** klickar du på **Hälsoavsökningar**. Observera att den är tom i nuläget.

1. Lägg till en ny hälsoavsökning genom att klicka på **Lägg till**.

1. Lägg till följande konfiguration på bladet **Lägg till hälsoavsökning**:
    - **Namn:** _woodgrove-HP_
    - **Protokoll:** _HTTP_
    - **Port:** _80_
    - **Sök:** _/_
    - **Intervall:** _15_
    - **Defekt tröskelvärde:** _2_

1. Spara ändringarna genom att klicka på **OK**.

Vänta tills belastningsutjämningskonfigurationen har uppdaterats innan du fortsätter med den här övningen.

## <a name="create-a-load-balancer-rule"></a>Skapa en lastbalanseringsregel

Skapa slutligen en regel för lastbalanseraren för HTTP via port 80 som associerar serverdelspoolen med hälsoavsökningen.

1. På bladet **woodgrove-LB** under **Inställningar** klickar du på **Regler för lastbalanserare**.

1. Klicka på **Lägg till** för att lägga till en ny regel.

1. På bladet **Lägg till regel för lastbalanserare** anger du **Namnet** _woodgrove-HTTP-LBRule_.

1. Kontrollera att följande information har angetts automatiskt:
    - IP-version: **IPv4**
    - Klientdelens IP-adress: **LoadBalancerFrontEnd**
    - Protokoll: **TCP**
    - Port: **80**
    - Serverdelsport: **80**
    - Serverdelspool: **woodgrove-BEP**
    - Hälsoavsökning: **woodgrove-HP**
    - Sessionspermanens: **Ingen**
    - Tidsgräns för inaktivitet: **4 minuter**
    - Flytande IP: **Inaktiverat**

1. Spara regeln genom att klicka på **OK**.

Vänta tills belastningsutjämningskonfigurationen har uppdaterats innan du fortsätter med den här övningen.

## <a name="test-the-load-balancer"></a>Testa lastbalanseraren

1. Gå tillbaka till sidan **Översikt** i lastbalanseraren och klickar på länken **Offentliga IP-adress** för att komma till den tilldelade IP-adressen. Alternativt kan du använda **Alla resurser** och sedan välja **woodgrove-LB-ip** i resurslistan.

1. Välj **IP-adress** på bladet **Översikt** och klicka sedan på knappen **Kopiera**. Det här är den _offentliga_ IP-adressen för lastbalanseraren.

    ![Skärmbild visar översiktspanelen för den offentliga IP-adressen](../media/6-public-ip.png)

1. Öppna en ny webbläsarflik och klistra in IP-adressen i adressfältet. Anteckna namnet på servern och håll fliken öppen.

1. Klicka på **Alla resurser** i menyn till vänster i Azure Portal. Klicka på den server som du antecknade ovan i resurslistan.

1. Klicka på **Stoppa** på **Översikt**-bladet och klicka sedan på **Ja**.

1. Vänta tills den virtuella datorn har stoppats och växla sedan till fliken du öppnade i steg 3. Uppdatera sidan.

1. Belastningsutjämnaren skickar nu din HTTP-begäran till en av dina andra virtuella datorer.

I den här övningen har du slutfört distributionen av dina virtuella datorer för serverdelar och för lastbalanseraren.

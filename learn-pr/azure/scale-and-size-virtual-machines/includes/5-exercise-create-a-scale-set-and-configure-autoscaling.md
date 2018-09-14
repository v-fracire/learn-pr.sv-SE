I den här övningen använder du Azure-portalen för att skapa en VM-skalningsuppsättning med regler för automatisk skalning.

## <a name="create-a-virtual-machine-scale-set"></a>Skapa en VM-skalningsuppsättning

1. Klicka på **Skapa en resurs** i Azure-portalen.

1. I sökrutan skriver **skalningsuppsättning** och tryck på RETUR. På den **resultat** bladet klickar du på **Virtual machine scale Sets**, och på den **Virtual machine scale Sets** bladet klickar du på **skapa**.

1. På den **skapa VM scale Sets** bladet anger du följande värden (Ersätt `<your initials>`, `<date>`, och `<time>` med relevanta data), och klicka sedan på **skapa**.

    |Inställning|Värde|
    |---|---|
    |Namn för VM-skalningsuppsättningen|WebServerSS|
    |Resursgrupp|Använd befintlig - <rgn>[Sandbox resursgruppens namn]</rgn>|
    |Användarnamn|LocalAdmin|
    |Lösenord och Bekräfta lösenord|Adm1nPa$ word|
    |Instansantal|2|
    |Instansstorlek|D2s_v3|
    |Välj belastningsutjämningsalternativ|Lastbalanserare|
    |Namn på offentlig IP-adress|WebServerPubIP|
    |Domännamnsetikett|`<your initials><date><time>` (till exempel ja0904181202)|

1. Vänta tills skalningsuppsättningen att skapas.

1. Navigera till den **<rgn>[Sandbox resursgruppens namn]</rgn>** resursgrupp. På den **<rgn>[Sandbox resursgruppens namn]</rgn>** bladet klickar du på den **WebServerSS** objekt, och på den **WebServerSS** bladet klickar du på **Instanser**. Observera de två instanser av virtuella datorer som körs i skalningsuppsättningen.

## <a name="create-and-apply-autoscale-rules"></a>Skapa och tillämpa regler för automatisk skalning

1. På den **WebServerSS** bladet klickar du på **skalning**. På den **WebServerSS - skalning** bladet klickar du på **aktivera autoskalning**.

1. På den **WebServerSS - skalning** bladet klickar du på **lägga till en regel**.

1. På den **skalningsregeln** bladet anger du följande information för att skapa en regel för att skala ut en extra två virtuella datorer när Genomsnittlig CPU-användning är mer än 75% över en period på fem minuter och klicka sedan på **Lägg till**.

    |Inställning|Värde|
    |---|---|
    |Tidsmängd|Medel|
    |Måttnamn|Procent CPU|
    |Tidsintervallstatistik|Medel|
    |Operator|Större än|
    |Tröskelvärde|75|
    |Varaktighet (i minuter)|5|
    |Åtgärd|Öka antal med|
    |Instansantal|2|
    |Väntetid (minuter)|5|

1. På den **WebServerSS - skalning** bladet klickar du på **lägga till en regel**.

1. På den **skalningsregeln** bladet anger du följande information för att skapa en regel för att skala på en server i taget när Genomsnittlig CPU-användning är under 30% under en period på fem minuter och klicka sedan på **Lägg till**.

    |Inställning|Värde|
    |---|---|
    |Tidsmängd|Medel|
    |Måttnamn|Procent CPU|
    |Tidsintervallstatistik|Medel|
    |Operator|Mindre än|
    |Tröskelvärde|30|
    |Varaktighet (i minuter)|5|
    |Åtgärd|Minska antal med|
    |Instansantal|1|
    |Väntetid (minuter)|5|

1. På den **WebServerSS - skalning** bladet i den **namn på Autoskalningsinställning** skriver **WebAutoscaleSetting**. Bredvid **Instansgränser**, anger du följande värden och klickar sedan på **spara**.

    |Inställning|Värde|
    |---|---|
    |Minimum|2|
    |Maximal|8|
    |Standard|2|

## <a name="generate-load-to-demonstrate-autoscaling"></a>Generera belastning för att visa automatisk skalning

Nu ska du använda verktyget CPUStress.exe att generera belastningen på de virtuella datorerna i skalningsuppsättningen och visa automatisk skalning ut.

1. För att avgöra den offentliga IP-adressen för att ansluta till, bläddrar du till den **ExerciseRG** resursgrupp. På den **ExerciseRG** bladet klickar du på den **WebServerSSlb** objekt.

1. På den **WebServerSSlb** bladet klickar du på **Frontend-IP-konfiguration** och anteckna den offentliga IP-adressen som visas. Stäng den **LoadBalancerFrontEnd** bladet.

1. Att fastställa det aktuella portnumret som ansluter till, på den **WebServerSSlb** bladet klickar du på **inkommande NAT-regler**. Anteckna det anpassa portnumret i den här kolumnen för varje virtuell dator.

1. På din lokala dator, starta den **anslutning till fjärrskrivbord**. I den **datorn** skriver du den offentliga IP-adressen för belastningsutjämnaren, följt av ett kolon och sedan portnumret exempelvis värdet 0 (till exempel 40.67.191.221:50000) och klicka sedan på **Connect**.

1. I den **Windows Security** dialogrutan klickar du på **fler alternativ**, och klicka sedan på **Använd ett annat konto**. I den **användarnamn** skriver **LocalAdmin**. I den **lösenord** skriver **Adm1nPa$ word**, och klicka sedan på **OK**.

1. I den **anslutning till fjärrskrivbord** dialogrutan klickar du på **Ja**.

1. I Fjärrskrivbord virtuell dator, öppna Internet Explorer, och den **konfigurera Internet Explorer** dialogrutan klickar du på **OK**.

1. I **Internet Explorer**, i adressfältet skriver **http://download.sysinternals.com/files/CPUSTRES.zip** och tryck på RETUR. I den **Internet Explorer** dialogrutan klickar du på **Lägg till**. I den **tillförlitliga platser** dialogrutan klickar du på **Lägg till** och klicka sedan på **Stäng**.

1. Om download uppmaningen inte visas automatiskt, kan du behöva ange URL: en igen och tryck på RETUR. I den **Internet Explorer** dialogrutan klickar du på **spara**. När hämtningen är klar klickar du på **Öppna mapp**.

1. I den **hämtar** mappen dubbelklickar du på **CPUSTRES**, och dubbelklicka sedan på den **CPUSTRES** program. I den **komprimerade mappar** dialogrutan klickar du på **kör**.

1. I den **CPU Stress** fönstret under **tråd 1**i den **aktivitet** listrutan, väljer **maximala**. Under **tråd 2**, klickar du på den **Active** markerar du kryssrutan och i den **aktivitet** listrutan, väljer **maximala**.

1. Klicka på knappen Start och klicka sedan på **Aktivitetshanteraren**. I Aktivitetshanteraren klickar du på **mer**, och bekräfta att Processorn över är 75%.

1. Upprepa installation och start av **CPUSTRES** ange på den andra virtuella datorn i skalan och vänta fem minuter sedan.

1. I Azure-portalen bläddrar du till den **ExerciseRG** resursgrupp. På den **ExerciseRG** bladet klickar du på den **WebServerSS** objekt. På den **WebServerSS** bladet klickar du på **instanser**. Observera hur många instanser av virtuella datorer visas i skalningsuppsättningen och status.

Här kan skapa du en VM-skalningsuppsättning med två virtuella datorer. Du sedan skapar regler för att automatiskt skala ut och skala i skalningsuppsättningen och du har lagt till regler i en autoskalningsprofil.
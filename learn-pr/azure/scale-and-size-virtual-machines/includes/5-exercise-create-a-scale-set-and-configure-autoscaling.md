I den här övningen använder du Azure Portal till att skapa en VM-skalningsuppsättning med regler för automatisk skalning.

## <a name="create-a-virtual-machine-scale-set"></a>Skapa en VM-skalningsuppsättning

1. Klicka på **Skapa en resurs** i Azure Portal.

1. I sökrutan skriver du **skalningsuppsättning** och trycker på Retur. På bladet **Resultat** klickar du på **VM-skalningsuppsättning**, och på bladet **VM-skalningsuppsättningar** klickar du på **Skapa**.

1. På bladet **Skapa VM-skalningsuppsättningar** anger du följande värden (byt ut `<your initials>`, `<date>` och `<time>` mot relevanta data) och klickar sedan på **Skapa**.

    |Inställning|Värde|
    |---|---|
    |Namn på VM-skalningsuppsättning|WebServerSS|
    |Resursgrupp|Använd befintlig – ExerciseRG|
    |Användarnamn|LocalAdmin|
    |Lösenord och Bekräfta lösenord|Adm1nPa$$word|
    |Antal instanser|2|
    |Instansstorlek|D2s_v3|
    |Välj alternativ för belastningsutjämning|Lastbalanserare|
    |Offentlig IP-adress|WebServerPubIP|
    |Domännamnsetikett|`<your initials><date><time>` (till exempel ja0904181202)|

1. Vänta tills skalningsuppsättningen har skapats.

1. Navigera till resursgruppen **ExerciseRG**. På bladet **ExerciseRG** klickar du på objektet **WebServerSS**, och på bladet **WebServerSS** klickar du på **Instanser**. Lägg märke till de två VM-instanser som körs i skalningsuppsättningen.

## <a name="create-and-apply-autoscale-rules"></a>Skapa och använda regler för automatisk skalning

1. På bladet **WebServerSS** klickar du på **Skalning**. På bladet **WebServerSS – Skalning** klickar du på **Aktivera autoskalning**.

1. På bladet **WebServerSS – Skalning** klickar du på **Lägg till en regel**.

1. På bladet Skalningsregel anger du följande information, så att du skapar en regel som skalar ut med två extra virtuella datorer när den genomsnittliga processoranvändningen är mer än 75 % i fem minuter, och klickar sedan på **Lägg till**.

    |Inställning|Värde|
    |---|---|
    |Tidsmängd|Medelvärde|
    |Måttnamn|Processorprocentandel|
    |Tidsintervallstatistik|Medelvärde|
    |Operator|Större än|
    |Tröskelvärde|75|
    |Varaktighet (i minuter)|5|
    |Åtgärd|Öka antalet med|
    |Antal instanser|2|
    |Väntetid (minuter)|5|

1. På bladet **WebServerSS – Skalning** klickar du på **Lägg till en regel**.

1. På bladet Skalningsregel anger du följande information, så att du skapar en regel som skalar ned med en server åt gången när den genomsnittliga processoranvändningen är mindre än 30 % i fem minuter, och klickar sedan på **Lägg till**.

    |Inställning|Värde|
    |---|---|
    |Tidsmängd|Medelvärde|
    |Måttnamn|Processorprocentandel|
    |Tidsintervallstatistik|Medelvärde|
    |Operator|Mindre än|
    |Tröskelvärde|30|
    |Varaktighet (i minuter)|5|
    |Åtgärd|Minska antalet med|
    |Antal instanser|1|
    |Väntetid (minuter)|5|

1. På bladet **WebServerSS – Skalning**, i rutan **Namn på autoskalningsinställning**, skriver du **WebAutoscaleSetting**. Bredvid **Instansgränser** anger du följande värden och klickar sedan på **Spara**.

    |Inställning|Värde|
    |---|---|
    |Minimum|2|
    |Maximal|8|
    |Standard|2|

## <a name="generate-load-to-demonstrate-autoscaling"></a>Generera belastning för att se den automatiska skalningen

Nu ska du använda verktyget CPUStress.exe till att generera belastning på de virtuella datorerna i skalningsuppsättningen så att du ser den automatiska utskalningen.

1. Du ser vilken offentliga IP-adress du ska ansluta till om du bläddrar till resursgruppen **ExerciseRG**. På bladet **ExerciseRG** klickar du på objektet **WebServerSSlb**.

1. På bladet **WebServerSSlb** klickar du på IP-konfiguration för klientdel och skriver ned den offentliga IP-adress som visas. Stäng bladet **LoadBalancerFrontEnd**.

1. Du ser vilket portnummer du ska ansluta till om du går till bladet **WebServerSSlb** och klickar på **Ingående NAT-regler**. Skriv ned portnumret i kolumnen Tjänst för varje virtuell dator.

1. Starta **Anslutning till fjärrskrivbord** lokalt. I rutan **Dator** skriver du den offentliga IP-adressen till belastningsutjämnaren, följt av ett kolon och sedan portnumret för instans 0 (till exempel 40.67.191.221:50000). Klicka på Anslut.

1. I dialogrutan **Windows-säkerhet** klickar du på **Fler alternativ** och **Använd ett annat konto**. I rutan **Användarnamn** skriver du **LocalAdmin** och i rutan **Lösenord** skriver du **Adm1nPa$ word**. Klicka sedan på **OK**.

1. I dialogrutan **Anslutning till fjärrskrivbord** klickar du på **Ja**.

1. Öppna Internet Explorer på den virtuella datorns fjärrskrivbord. I dialogrutan **Konfigurera Internet Explorer** klickar du på **OK**.

1. I adressfältet i **Internet Explorer** skriver du **http://download.sysinternals.com/files/CPUSTRES.zip** och trycker på Retur. I dialogrutan **Internet Explorer** klickar du på **Lägg till**. I dialogrutan **Betrodda platser** klickar du på **Lägg till** och sedan på **Stäng**.

1. Om nedladdningsprompten inte visas automatiskt kan du behöva ange webbadressen igen och trycka på Retur. I dialogrutan **Internet Explorer** klickar du på **Spara**. När nedladdningen är färdig klickar du på **Öppna mapp**.

1. I mappen **Hämtade filer** dubbelklickar du på **CPUSTRES** och sedan på programmet **CPUSTRES**. I dialogrutan **Komprimerade mappar** klickar du på **Kör**.

1. I fönstret **CPU Stress** (CPU-belastning) under **Thread 1** (Tråd 1) går du till listrutan **Activity** (Aktivitet) och väljer **Maximum** (Maximal). Under **Thread 2** (Tråd 2) markerar du kryssrutan **Active** (Aktiv) och i listrutan **Activity** (Aktivitet) väljer du **Maximum** (Maximal).

1. Klicka på knappen Start och sedan på **Aktivitetshanteraren**. I Aktivitetshanteraren klickar du på **Mer information** och kontrollerar att CPU-värdet är över 75 %.

1. Upprepa installationen och starten av **CPUSTRES** på den andra virtuella datorn i skalningsuppsättningen och vänta i fem minuter.

1. Bläddra till resursgruppen **ExerciseRG** i Azure Portal. Gå till bladet **ExerciseRG** och klicka på objektet **WebServerSS**. På bladet **WebServerSS** klickar du på **Instanser**. Notera hur många virtuella datorinstanser som visas i skalningsuppsättningen och deras status.

Här har du skapat en VM-skalningsuppsättning med två virtuella datorer. Sedan skapade du regler för att automatiskt skala ut och skala in skalningsuppsättningen, och lade till reglerna i en profil för automatisk skalning.
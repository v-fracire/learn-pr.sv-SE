I den här övningen använder du Azure Portal till att skapa en VM-skalningsuppsättning med regler för automatisk skalning.

## <a name="create-a-virtual-machine-scale-set"></a>Skapa en VM-skalningsuppsättning

1. I [Azure-portalen](https://portal.azure.com/learn.docs.microsoft.com?azure-portal=true) klickar du på **Skapa en resurs**.

1. I sökrutan skriver du **skalningsuppsättning** och trycker på <kbd>Retur</kbd>. På bladet **Resultat** klickar du på **VM-skalningsuppsättningar**, och på bladet **VM-skalningsuppsättningar** klickar du på **Skapa**.

1. Ange följande värden på bladet **Skapa VM-skalningsuppsättningar**.
    - Använd _WebServerSS_ som **Namn**.
    - Lämna _Windows Server 2016 Datacenter_ för **Avbildning av operativsystemdisk**.
    - Välj _Concierge-prenumeration_ under **Prenumeration**.
    - Välj **<rgn>[Resursgruppsnamn för sandbox-miljö]</rgn>** som **Resursgrupp**.
    - Välj en plats från nedanstående lista:   [!include[](../../../includes/azure-sandbox-regions-note-friendly.md)]

    - Ange ett giltigt **Användarnamn** och **Lösenord**. (Spara dessa för senare användning)
    - Ange **Antal instanser** till _2_.
    - Ange **Instansstorlek** till _D2s_v3_.
    - Lämna **Autoskalning** som _Inaktiverad_.
    - Välj **Lastbalanserare**.
    - Ange _WebServerPubIP_ för **Namn på offentlig IP-adress**.
    - Använd dina initialer + datum + tid som **Domännamnsetikett** så att den blir unik.
    - För **Virtuellt nätverk** väljer du **Skapa nytt**. Ge det namnet **SSVNet** och klicka på **Skapa**.

1. Klicka på **Skapa** för att använda skalningsuppsättningen.

1. Vänta tills skalningsuppsättningen har skapats.

## <a name="create-and-apply-autoscale-rules"></a>Skapa och använda regler för automatisk skalning

1. Klicka på **Resursgrupper** i det vänstra sidofältet.

1. Välj resursgruppen **<rgn>[Resursgruppsnamn för sandbox-miljö]</rgn>**.

1. På bladet **<rgn>[Resursgruppsnamn för sandbox-miljö]</rgn>** klickar du på **WebServerSS**-objektet för att öppna skalningsuppsättningen.

1. På bladet **WebServerSS** under **Inställningar** klickar du på **Instanser**. Lägg märke till de två VM-instanser som körs i skalningsuppsättningen.

1. På bladet **WebServerSS** klickar du på **Skalning**.

1. På bladet **WebServerSS – Skalning** klickar du på **Aktivera autoskalning**.

1. Ange namnet **WebAutoscaleSetting** på konfigurationsskärmen.

1. Leta upp **Instansgränser** i standardvillkoret och ange följande värden.

    |Inställning|Värde|
    |---|---|
    |Minimum|2|
    |Maximal|8|
    |Standard|2|

1. Klicka på **Lägg till en regel**.

1. På bladet **Skalningsregel** anger du följande information för att skapa en regel för att skala ut två extra virtuella datorer när genomsnittlig CPU-användning är mer är 75 % över en femminutersperiod och klickar därefter på **Lägg till**.

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

1. Lägg till ännu en regel. Den här gången anger du följande information så att du skapar en regel som skalar in med en server åt gången när den genomsnittliga processoranvändningen är mindre än 30 % i fem minuter och klickar sedan på **Lägg till**.

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

1. Reglerna bör se ut ungefär så här:

    ![Skärmbild av regeluppsättningen för autoskalning tillämpad på VM-skalningsuppsättningen](../media/5-scale-rules.png)

1. Klicka på **Spara**.

## <a name="generate-load-to-demonstrate-autoscaling"></a>Generera belastning för att se den automatiska skalningen

Nu ska du använda SysInternals-verktyget **CPUStress.exe** till att generera belastning på de virtuella datorerna i skalningsuppsättningen så att du ser den automatiska utskalningen.

Vi kör det här på den virtuella datorn – du behöver inte ha Windows för detta, men du _behöver_ en fjärrskrivbordsklient. Microsoft har en för macOS och Windows, och olika distributioner av Linux inkluderar även en klient.

1. Du ser vilken offentlig IP-adress du ska ansluta till om du bläddrar till resursgruppen **<rgn>[Resursgruppsnamn för sandbox-miljö]</rgn>**. På bladet **<rgn>[Resursgruppsnamn för sandbox-miljö]</rgn>** klickar du på **WebServerSSlb**-objektet.

1. På bladet **WebServerSSlb** klickar du på **IP-konfiguration för klientdel** och skriver ned den offentliga IP-adress som visas. Stäng bladet **LoadBalancerFrontEnd**.

1. Du ser vilket portnummer du ska ansluta till om du går till bladet **WebServerSSlb** och klickar på **Ingående NAT-regler**. Skriv ned portnumret i kolumnen Tjänst för varje virtuell dator.

1. Starta **Anslutning till fjärrskrivbord** lokalt. I rutan **Dator** skriver du den offentliga IP-adressen till belastningsutjämnaren, följt av ett kolon och sedan portnumret för instans 0 (till exempel 40.67.191.221:50000). Klicka på **Anslut**.

1. I dialogrutan **Windows-säkerhet** klickar du på **Fler alternativ** och sedan på **Använd ett annat konto**. I rutorna **Användarnamn** och **Lösenord** anger du de autentiseringsuppgifter du angav ovan när du skapade skalningsuppsättningen.

1. I dialogrutan **Anslutning till fjärrskrivbord** klickar du på **Ja**.

1. Öppna Internet Explorer på den virtuella datorns fjärrskrivbord. I dialogrutan **Konfigurera Internet Explorer** klickar du på **OK**.

1. I adressfältet i **Internet Explorer** skriver du **http://download.sysinternals.com/files/CPUSTRES.zip** och trycker på <kbd>Retur</kbd>. I dialogrutan **Internet Explorer** klickar du på **Lägg till**. I dialogrutan **Betrodda platser** klickar du på **Lägg till** och sedan på **Stäng**.

1. Om nedladdningsprompten inte visas automatiskt kan du behöva ange webbadressen igen och trycka på <kbd>Retur</kbd>. I dialogrutan **Internet Explorer** klickar du på **Spara**. När nedladdningen är färdig klickar du på **Öppna mapp**.

1. I mappen **Hämtade filer** dubbelklickar du på **CPUSTRES** och sedan på programmet **CPUSTRES**. I dialogrutan **Komprimerade mappar** klickar du på **Kör**.

1. I fönstret **CPU Stress** (CPU-belastning) under **Thread 1** (Tråd 1) går du till listrutan **Activity** (Aktivitet) och väljer **Maximum** (Maximal). Under **Thread 2** (Tråd 2) markerar du kryssrutan **Active** (Aktiv) och i listrutan **Activity** (Aktivitet) klickar du på nedpilen i listrutan och väljer **Maximum** (Maximal). De här inställningarna börjar gälla omedelbart.

1. Klicka på knappen Start i Windows och sedan på **Aktivitetshanteraren**. I Aktivitetshanteraren klickar du på **Mer information** och kontrollerar att CPU-värdet är över 75 %.

1. **Upprepa** installationen och starten av **CPUSTRES** på den andra virtuella datorn i skalningsuppsättningen och vänta i fem minuter.

1. Gå till resursgruppen **<rgn>[Resursgruppsnamn för sandbox-miljö]</rgn>** i Azure-portalen. På bladet **<rgn>[Resursgruppsnamn för sandbox-miljö]</rgn>** klickar du på **WebServerSS**-objektet. På bladet **WebServerSS** klickar du på **Instanser**. Notera hur många virtuella datorinstanser som visas i skalningsuppsättningen och deras status.

Här har du skapat en VM-skalningsuppsättning med två virtuella datorer. Sedan skapade du regler för att automatiskt skala ut och skala in skalningsuppsättningen och lade till reglerna i en profil för automatisk skalning.

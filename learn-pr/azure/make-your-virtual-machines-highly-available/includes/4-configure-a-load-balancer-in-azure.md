Innan belastningsutjämnaren ska fungera korrekt måste konfigurera du inställningar som styr belastningsutjämnarens beteende. Här ska vi titta på konfigurera nätverket, hälsoavsökning, säkerhetsregler, belastningsutjämningsregler och serverpoolen.

## <a name="steps-for-configuring-a-basic-public-load-balancer"></a>Stegen för att konfigurera en offentlig grundläggande belastningsutjämnare

Följande är en översikt över de viktigaste konfigurationsstegen för en offentlig grundläggande belastningsutjämnare. Stegen för en standard-belastningsutjämnare och för en intern Azure load balancer ska vara detsamma.

### <a name="back-end-servers"></a>Backend-servrar

Du måste först konfigurera serverdelens VM-pool. De virtuella datorerna ska vara i samma tillgänglighetsuppsättning och har sina egna offentliga IP-adressen (även om detta inte faktiskt används av din offentliga slutpunkter).

Du måste skapa ett nytt virtuellt nätverk och definiera ett undernät för VM-poolen som ska användas.

 När du har flera virtuella datorer som tillhandahåller samma tjänster, bör du använda en **nätverkssäkerhetsgrupp (NSG)** så att samma brandväggsregler är på plats för VM-pool (även om detta inte är en del av den faktiska Utjämning av nätverksbelastning) . För virtuella datorer som är värd för webbprogram, kommer du behöva skapa inkommande säkerhetsregler på port 80 för HTTP och port 8080 för HTTPS.

### <a name="public-ip-address"></a>Offentlig IP-adress

När du skapar en offentlig grundläggande belastningsutjämnare med hjälp av portalen den **offentliga IP-adressen** konfigureras automatiskt som belastningsutjämnarens klientdel.

En del av konfigurationen av belastningsutjämnaren är den **backend adresspoolen**, som innehåller IP-adresserna för varje virtuell dators virtuella nätverkskort som är anslutna till belastningsutjämnaren och används för att distribuera trafik till de virtuella datorerna. 

### <a name="health-probe"></a>Hälsoavsökning

Hälsoavsökningen lägger till eller tar bort virtuella datorer dynamiskt från lastbalanserarens rotation baserat på deras svar på hälsokontroller.
Som standard finns 15 sekunder mellan avsökningsförsöken. När du har två avsökningsfel betraktas en virtuell dator defekt.

### <a name="rules"></a>Regler

Belastningsutjämningsregeln anger den port som klienten lyssnar på och den port som används för att skicka trafik till serverdelen.

För att lastbalanseraren ska fungera korrekt måste du konfigurera inställningar som styr lastbalanserarens beteende. Här går du igenom konfiguration av nätverket, hälsoavsökning, säkerhetsregler, belastningsutjämningsregler och serverpoolen.

## <a name="steps-to-configure-a-basic-public-load-balancer"></a>Steg för att konfigurera en Basic Public Load Balancer

Följande är en översikt över de viktigaste konfigurationsstegen för en Basic Public Load Balancer; stegen för en Standard Load Balancer och för en intern lastbalanserare kommer att vara liknande.

### <a name="backend-servers"></a>Serverdelsservrar

Först måste du konfigurera din pool med virtuella serverdelsdatorer. De virtuella datorerna ska vara i samma tillgänglighetsuppsättning och ha sina egna offentliga IP-adresser (även om detta inte faktiskt används av dina offentliga slutpunkter).

Du måste skapa ett nytt virtuellt nätverk (vNet) och definiera ett undernät som VM-poolen ska använda.

Det hör inte direkt till belastningsutjämning, men när du har flera virtuella datorer som tillhandahåller samma tjänster bör du använda en **nätverkssäkerhetsgrupp (NSG)** så att samma brandväggsregler används i hela VM-poolen. För virtuella datorer som är värd för webbprogram behöver du till exempel skapa inkommande säkerhetsregler på port 80 för HTTP och port 8080 för HTTPS.

### <a name="public-ip-address"></a>Offentlig IP-adress

När du skapar en offentlig Basic-lastbalanserare med hjälp av portalen konfigureras den **offentliga IP-adressen** automatiskt som lastbalanserarens klientdel.

En del av konfigurationen av lastbalanseraren är **serverdelsadresspoolen**, som innehåller IP-adresserna för varje virtuella dators nätverkskort som är anslutet till lastbalanseraren och används för att distribuera trafik till de virtuella datorerna. 

### <a name="health-probe"></a>Hälsoavsökning

Hälsoavsökningen lägger till eller tar bort virtuella datorer dynamiskt från lastbalanserarens rotation baserat på deras svar på hälsokontroller.
Som standard går det 15 sekunder mellan avsökningsförsöken, och efter två avsökningsfel i rad betraktas en virtuell dator som defekt.

### <a name="rules"></a>Regler

Lastbalanserarens regel anger den port som klientdelen lyssnar på och den port som används för att skicka trafik till serverdelen.
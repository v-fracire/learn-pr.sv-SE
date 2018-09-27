För att lastbalanseraren ska fungera korrekt måste du konfigurera inställningar som styr lastbalanserarens beteende. Här går du igenom konfiguration av nätverket, hälsoavsökning, säkerhetsregler, lastbalanseringsregler och serverpoolen.

## <a name="steps-for-configuring-a-basic-public-load-balancer"></a>Steg för att konfigurera en grundläggande offentlig lastbalanserare

Följande är en översikt över de huvudsakliga konfigurationsstegen för en grundläggande offentlig lastbalanserare. Stegen för en standard lastbalanserare och för en intern lastbalanserare kommer att vara liknande. Det finns i huvudsak fyra saker som vi måste installera:

- Backend-servrar för att bearbeta begäranden
- Offentlig IP-adress
- Hälsoavsökning
- Regler för lastbalanserare

### <a name="backend-servers"></a>Serverdelsservrar

Först måste du konfigurera din pool med virtuella serverdelsdatorer. De virtuella datorerna ska vara i samma tillgänglighetsuppsättning och ha sina egna offentliga IP-adresser (även om detta inte faktiskt används av dina offentliga slutpunkter).

Du måste skapa ett nytt virtuellt nätverk och definiera ett undernät som VM-poolen ska använda.

 När du har flera virtuella datorer som tillhandahåller samma tjänster bör du använda en **nätverkssäkerhetsgrupp (NSG)** för att se till att samma brandväggsregler används i hela VM-poolen (även om det här inte är en del av den faktiska lastbalanseringsprocessen). För virtuella datorer som är värd för webbprogram behöver du till exempel skapa inkommande säkerhetsregler på port 80 för HTTP och port 8080 för HTTPS.

### <a name="public-ip-address"></a>Offentlig IP-adress

När du skapar en offentlig grundläggande lastbalanserare med hjälp av portalen konfigureras den **offentliga IP-adressen** automatiskt som lastbalanserarens främre del.

En del av konfigurationen av lastbalanseraren är **serverdelsadresspoolen**, som innehåller IP-adresserna för varje virtuella dators nätverkskort som är anslutet till lastbalanseraren och används för att distribuera trafik till de virtuella datorerna.

### <a name="health-probe"></a>Hälsoavsökning

Hälsoavsökningen lägger till eller tar bort virtuella datorer dynamiskt från lastbalanserarens rotation baserat på deras svar på hälsokontroller. Som standard är det 15 sekunder mellan avsökningsförsöken. Efter två avsökningsfel betraktas en virtuell dator som defekt och trafik  dirigeras inte dit tills servern är online igen.

### <a name="rules"></a>Regler

Lastbalanserarens regel identifierar den port som Lastbalanseraren lyssnar på och den port som används för att skicka trafik till virtuella datorers bakre del. Du behöver minst en regel för att dirigera trafik.

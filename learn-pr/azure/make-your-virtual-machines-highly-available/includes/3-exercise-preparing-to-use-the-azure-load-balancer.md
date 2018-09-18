I den här övningen skapar du en lastbalanserare, ett virtuellt nätverk och flera virtuella datorer med hjälp av Azure-portalen.

Anta att du arbetar för Woodgrove Bank, ett nystartat företag som håller på att starta onlinetjänster för bankärenden. Konkurrensen är mycket kraftig i den här sektorn, så du måste garantera minst 99,99 % tjänsttillgänglighet. Du har fastställt att en Azure Load Balancer med en pool med tre virtuella datorer uppfyller det här målet.

## <a name="create-a-public-load-balancer"></a>Skapa en offentlig lastbalanserare

1. I en webbläsare navigerar du till [Azure-portalen](https://portal.azure.com/?azure-portal=true) och loggar in på ditt konto.

1. I sidofältet klickar du på **Skapa en resurs**, och i det **nya** bladet klickar du på **Nätverk** och sedan på **Lastbalanserare**.

1. På bladet Skapa lastbalanserare anger eller väljer du följande information:
    - Namn: **woodgrove-LB**
    - Typ: **Offentlig**
    - SKU: **Basic**
    - Offentlig IP-adress: välj **Skapa ny**, skriv **woodgrove-LB-ip** i textrutan och låt Tilldelning vara kvar som **Dynamisk**
    - Resursgrupp: välj **Skapa ny** och skriv **woodgrove-RG** i rutan
    - Plats: Välj en region nära dig

1. Klicka på **Skapa**.

1. Vänta tills lastbalanseraren har distribuerats innan du fortsätter med den här övningen.

## <a name="create-a-virtual-network"></a>Skapa ett virtuellt nätverk

1. I den vänstra menyn klickar du på **Skapa en resurs**, och i det **nya** bladet klickar du på **Nätverk** och sedan på **Virtuellt nätverk**.

1. På bladet **Skapa virtuellt nätverk** anger eller väljer du följande information:
    - Namn: **woodgrove-VNET**
    - Adressutrymme: **172.20.0.0/16**
    - Resursgrupp: välj **Använd befintlig** och sedan **woodgrove-RG**.
    - Undernät: **backendSubnet**
    - Adressutrymme: **172.20.0.0/24**
    - DDoS-skydd: **Basic**
    - Tjänstens slutpunkter: **Inaktiverat**

1. Klicka på **Skapa**.

1. Vänta tills det virtuella nätverket har distribuerats innan du fortsätter med den här övningen.

## <a name="create-a-vm-template"></a>Skapa en mall för virtuell dator

Börja med att definiera grundläggande information för den virtuella datorn:

1. I Azure-portalen går du till den vänstra menyn och klickar på **Virtuella datorer** och sedan på **Skapa virtuell dator**.

1. På bladet Beräkning går du till området **Rekommenderas** och klickar på **Windows Server**.

1. På bladet **Windows Server** klickar du på **Windows Server 2016 Datacenter**.

1. På bladet **Windows Server 2016 Datacenter** klickar du på **Skapa**.

1. På bladet **Basic** går du till rutan **Namn** och skriver **woodgrove-SVR01**.

1. I rutorna **Användarnamn** och **Lösenord** skriver du ett säkert namn och lösenord för ett administratörskonto på den här servern.

1. I rutan **Prenumeration** väljer du din Azure-prenumeration.

1. Under **Resursgrupp** väljer du **Använd befintlig**, och i listan väljer du **woodgrove-RG**.

1. I listrutan **Plats** väljer du en region nära dig.

1. Klicka på **OK**.

Välj en storlek för den virtuella datorn och konfigurera inställningar:

1. På bladet **Välj storlek** väljer du en **Standard**-SKU, till exempel **D2s_v3** och klickar sedan på **Välj**.

1. På bladet **Inställningar** klickar du på **Tillgänglighetsuppsättning**.

1. På bladet **Ändra tillgänglighetsuppsättning** klickar du på **Skapa ny**.

1. På bladet **Skapa ny** går du till rutan **Namn**, skriver **woodgrove-AS** och klickar sedan på **OK**.

1. På bladet **Inställningar** går du till **Nätverkssäkerhetsgrupp**, klickar på **Avancerat** och klickar sedan på **(new) woodgrove-SVR01-nsg**.

1. På bladet **Skapa nätverkssäkerhetsgrupp** går du till rutan **Namn**, ändrar namnet till **woodgrove-NSG** och klickar sedan på **OK**.

1. I bladet **Inställningar** klickar du på **OK**.

Spara inställningarna till en mall så att du enkelt kan distribuera flera virtuella datorer.

1. På bladet **Skapa** klickar du på **Ladda ned mall och parametrar**.

1. På bladet **Mall** klickar du på **Lägg till i bibliotek**.

1. På bladet **Spara mallen** går du till rutorna **Namn** och **Beskrivning**, skriver in **woodgrove-server-template** och klickar sedan på **Spara**.

> [!NOTE]
> Om du behöver hitta den här mallen klickar du på **Alla tjänster** i den vänstra menyn, skriver **template** (mall) i filterrutan och klickar sedan på **Templates (PREVIEW)** (Mallar (FÖRHANDSVERSION)).

## <a name="use-the-template-to-provision-the-first-vm"></a>Använda mallen för att etablera den första virtuella datorn

1. På bladet **Mall** klickar du på **Distribuera**.

1. På bladet **Custom deployment** (Anpassad distribution) går du till **Resursgrupp**, väljer **Använd befintlig** och väljer **woodgrove-RG** i listan.

1. På bladet **Custom deployment** (Anpassad distribution) går du till rutan **Admin password** (Administratörslösenord) och skriver in samma lösenord som du använde tidigare.

1. På bladet **Custom deployment** (Anpassad distribution) markerar du kryssrutan **I agree to the terms and conditions** (Jag godkänner villkoren) och klickar sedan på **Köp** (kostnaden är den vanliga Azure-beräkningskostnaden, som beror på prisnivån för virtuella datorer).

1. Vänta tills den virtuella datorn har distribuerats innan du fortsätter med den här övningen; detta är så att du kan vara säker på att mallen är korrekt konfigurerad innan du använder den för att etablera ytterligare virtuella datorer och att alla de associerade resurserna har skapats.

## <a name="use-the-template-to-provision-two-additional-vms"></a>Använda mallen för att etablera ytterligare två virtuella datorer

1. I Azure-bladet går du till bladet **Mall** och klickar på **Distribuera**.

1. På bladet **Custom deployment** (Anpassad distribution) går du till **Resursgrupp**, väljer **Använd befintlig** och väljer **woodgrove-RG** i listan.

1. På bladet **Custom deployment** (Anpassad distribution) går du till rutan **Namn på virtuell dator** och ändrar namnet till **woodgrove-SVR02**.

1. På bladet **Custom deployment** (Anpassad distribution) går du till rutan **Network Interface Name** (Namn på nätverksgränssnitt) och ändrar namnet till **woodgrovesvr02222**.

1. På bladet **Custom deployment** (Anpassad distribution) går du till rutan **Admin password** (Administratörslösenord) och skriver in samma lösenord som du använde tidigare.

1. På bladet **Custom deployment** (Anpassad distribution) går du till rutan **Public Ip Address Name** (Namn på offentlig IP-adress) och ändrar namnet till **woodgrove-SVR02-ip**.

1. På bladet **Custom deployment** (Anpassad distribution) markerar du kryssrutan **I agree to the terms and conditions** (Jag godkänner villkoren) och klickar sedan på **Köp** (kostnaden är den vanliga Azure-beräkningskostnaden, som beror på prisnivån för virtuella datorer).

1. Upprepa stegen 1–7 med följande information:
    - Namn på virtuell dator: **woodgrove-SVR03**
    - Namn på nätverksgränssnitt: **woodgrovesvr03333**
    - Namn på offentlig IP-adress: **woodgrove-SVRr03-ip**

1. Vänta tills de virtuella datorerna har distribuerats innan du fortsätter med den här övningen.

Nu har du en offentlig lastbalanserare som är redo att konfigureras och tre virtuella datorer som är redo att användas med den här lastbalanseraren.
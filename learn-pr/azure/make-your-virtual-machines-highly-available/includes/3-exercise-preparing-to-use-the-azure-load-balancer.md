I den här övningen ska du skapa en belastningsutjämnare, ett virtuellt nätverk och flera virtuella datorer med Azure portal.

Anta att du arbetar för Woodgrove Bank, en start som håller på att starta online-tjänster. Sektorn är konkurrenskraftiga, så du måste garantera av minst 99,99% tillgänglighet. Du har fastställt att Azure Load Balancer med en pool med tre virtuella datorer uppfyller det här målet.

## <a name="create-a-public-load-balancer"></a>Skapa en offentlig lastbalanserare

[!include[](../../../includes/azure-sandbox-activate.md)]

[!include[](../../../includes/azure-sandbox-regions-first-mention-note.md)]

1. Logga in på [Azure Portal](https://portal.azure.com/?azure-portal=true).

1. I sidopanelen, klickar du på **skapa en resurs**. I den **New** bladet klickar du på **nätverk**, och klicka sedan på **belastningsutjämnaren**.

1. I den **skapa belastningsutjämnare** bladet ange eller Välj följande information:
    - Namn: **woodgrove-LB**
    - Typ: **offentliga**
    - SKU: **Basic**
    - Offentlig IP-adress: Välj **Skapa ny**. I textrutan skriver **woodgrove-LB-IP-**. Lämna tilldelningen som **dynamisk**.
    - Resursgrupp: Välj **Använd befintlig** och välj <rgn>[Sandbox resursgruppens namn]</rgn>.
    - Plats: Välj en region nära dig.

1. Klicka på **Skapa**.

1. Vänta tills belastningsutjämnaren har distribuerats innan du fortsätter med den här övningen.

## <a name="create-a-virtual-network"></a>Skapa ett virtuellt nätverk

1. I den vänstra menyn klickar du på **skapa en resurs**. I den **New** bladet klickar du på **nätverk**, och klicka sedan på **virtuellt nätverk**.

1. I den **skapa virtuellt nätverk** bladet ange eller Välj följande information:
    - Namn: **woodgrove-VNET**
    - Adressutrymme: **172.20.0.0/16**
    - Resursgrupp: Välj **Använd befintlig**, och välj sedan <rgn>[Sandbox resursgruppens namn]</rgn>.
    - Undernät: **backendSubnet**
    - Adressutrymme: **172.20.0.0/24**
    - DDoS-skydd: **grundläggande**
    - Tjänstens slutpunkter: **inaktiverad**

1. Klicka på **Skapa**.

1. Vänta tills det virtuella nätverket har distribuerats innan du fortsätter med den här övningen.

## <a name="create-a-vm-template"></a>Skapa en mall

Starta genom att definiera grundläggande information för den virtuella datorn:

1. I Azure-portalen, i den vänstra menyn klickar du på **virtuella datorer**, och klicka sedan på **Skapa virtuell dator**.

1. På den **Compute** bladet i den **rekommenderas** klickar du på **Windows Server**.

1. Klicka på **Windows Server 2016 Datacenter** i bladet **Windows Server**.

1. Klicka på **Skapa** i bladet **Windows Server 2016 Datacenter**.

1. I den **grunderna** bladet i den **namn** skriver **woodgrove SVR01**.

1. I den **användarnamn** och **lösenord**, anger du säkra namn och lösenord för ett administratörskonto på den här servern.

1. Välj din Azure-prenumeration i fältet **Prenumeration**.

1. Under **resursgrupp**väljer **Använd befintlig**. I listan, Välj **woodgrove-RG**.

1. Välj en region nära dig i listrutan **Plats**.

1. Klicka på **OK**.

Välj en storlek för den virtuella datorn och konfigurerar sedan inställningarna:

1. På den **väljer du en storlek** bladet och välja en **Standard** SKU, till exempel **D2s_v3**. Klicka sedan på **Välj**.

1. På den **inställningar** bladet klickar du på **tillgänglighetsuppsättning**.

1. På den **ändra tillgänglighetsuppsättning** bladet klickar du på **Skapa ny**.

1. På den **Skapa nytt** bladet i den **namn** skriver **woodgrove-AS**, och klicka sedan på **OK**.

1. På den **inställningar** bladet under **Nätverkssäkerhetsgrupp**, klickar du på **Avancerat**, och klicka sedan på **(ny) woodgrove-SVR01-nsg**.

1. På den **skapa nätverkssäkerhetsgrupp** bladet i den **namn** ändrar du namnet på **woodgrove NSG**, och klicka sedan på **OK**.

1. På den **inställningar** bladet klickar du på **OK**.

Spara inställningarna till en mall så att du kan enkelt distribuera flera virtuella datorer.

1. På den **skapa** bladet klickar du på **ladda ned mall och parametrar**.

1. På den **mall** bladet klickar du på **lägger du till biblioteket**.

1. På den **spara mallen** bladet i den **namn** och **beskrivning** rutor, Skriv **woodgrove servermall**. Klicka sedan på **Spara**.

> [!NOTE]
> Om du vill hitta den här mallen kan du klicka på **alla tjänster** i den vänstra menyn skriver **mall** i filterrutan och sedan på **mallar (FÖRHANDSVERSION)**.

## <a name="use-the-template-to-provision-the-first-vm"></a>Använda mallen för att etablera den första virtuella datorn

1. På den **mall** bladet klickar du på **distribuera**.

1. På den **anpassad distribution** bladet under **resursgrupp**väljer **Använd befintlig**. I listan, Välj **woodgrove-RG**.

1. På den **anpassad distribution** bladet i den **adminlösenord** skriver du samma lösenord som du använde tidigare.

1. På den **anpassad distribution** bladet och välja den **jag samtycker till villkoren** och klicka sedan på **köp** (kostnaden är vanliga Azure-beräkning debiterar som beror på den virtuella datorn prisnivå).

1. Vänta tills den virtuella datorn har distribuerats innan du fortsätter med den här övningen. Detta är så kan du vara säker på att mallen har konfigurerats korrekt innan du kan använda den för att etablera ytterligare virtuella datorer och att de associerade resurserna har skapats.

## <a name="use-the-template-to-provision-two-additional-vms"></a>Med mallen kan du etablera ytterligare två virtuella datorer

1. I Azure-portalen på den **mall** bladet klickar du på **distribuera**.

1. På den **anpassad distribution** bladet under **resursgrupp**väljer **Använd befintlig**. I listan, Välj **woodgrove-RG**.

1. På den **anpassad distribution** bladet i den **virtuellt datornamn** ändrar du namnet på **woodgrove SVR02**.

1. På den **anpassad distribution** bladet i den **Nätverksgräsnittsnamn** ändrar du namnet på **woodgrovesvr02222**.

1. På den **anpassad distribution** bladet i den **adminlösenord** skriver du samma lösenord som du använde tidigare.

1. På den **anpassad distribution** bladet i den **offentligt Ip-adressnamn** ändrar du namnet på **woodgrove-SVR02-IP-**.

1. På den **anpassad distribution** bladet och välja den **jag samtycker till villkoren** och klicka sedan på **köp** (kostnaden är vanliga Azure-beräkning debiterar som beror på den virtuella datorn prisnivå).

1. Upprepa steg 1-7, med följande information:
    - Namn på virtuell dator: **woodgrove SVR03**
    - Nätverksgränssnittets namn: **woodgrovesvr03333**
    - Namn på offentlig IP-adress: **woodgrove-SVRr03-ip**

1. Vänta tills de virtuella datorerna har distribuerat innan du fortsätter med den här övningen.

Nu har du en offentlig belastningsutjämnare som är redo att konfigurera och tre virtuella datorer är klar för att användas med den här belastningsutjämnaren.
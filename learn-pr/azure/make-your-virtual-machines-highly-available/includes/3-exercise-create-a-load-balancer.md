Anta att du arbetar för Woodgrove Bank och att du håller på att starta onlinetjänster för bankärenden. Konkurrensen är mycket kraftig i den här sektorn, så du måste garantera minst 99,99 % tjänsttillgänglighet. Du har fastställt att en Azure Load Balancer med en pool med tre virtuella datorer uppfyller det här målet.

I den här övningen skapar du en lastbalanserare och ett virtuellt nätverk hjälp av Azure-portalen. Eftersom vi bara behöver en av dessa är det enkelt att skapa den i portalen.

[!include[](../../../includes/azure-sandbox-activate.md)]

## <a name="create-a-public-load-balancer"></a>Skapa en offentlig lastbalanserare

1. Logga in på [Azure-portalen](https://portal.azure.com/learn.docs.microsoft.com?azure-portal=true) med samma konto som du använde för att aktivera sandbox-miljön.

1. I sidofältet klickar du på **Skapa en resurs**.

1. Välj avsnittet **Nätverk** och klicka sedan på **Lastbalanserare**. Du kan använda sökrutan om du inte ser det alternativet.

    ![Skärmbild som visar Microsoft Azure Marketplace med avsnittet Nätverk valt och Lastbalanserare markerat](../media/3-azure-marketplace.png)

1. I bladet **Skapa lastbalanserare** anger du eller väljer följande information:
    - **Namn:** _woodgrove-LB_
    - **Typ:** _Offentlig_
    - **SKU:** _Grundläggande_
    - **Offentlig IP-adress:** Välj **Skapa ny**. I textrutan skriver du _woodgrove-LB-ip_. Lämna tilldelningen som _Dynamisk_.
    - **Prenumeration:** bör redan ha _Concierge-prenumeration_ valt.
    - **Resursgrupp:** välj **Använd befintlig** och välj _<rgn>[Resursgruppnamn för sandbox-miljön]</rgn>_.
    - **Plats:** välj en region nära dig från följande lista.

    [!include[](../../../includes/azure-sandbox-regions-first-mention-note-friendly.md)]

    ![Skärmbild som visar skärmen för att skapa ny lastbalanserare](../media/3-create-load-balancer.png)

1. Klicka på **Skapa**.

Medan lastbalanserarresursen skapas och distribueras ska vi skapa det Azure Virtual Network vi ska använda för serversidans undernät.

## <a name="create-a-virtual-network"></a>Skapa ett virtuellt nätverk

1. I den vänstra menyn klickar du på **Skapa en resurs**. I bladet **Ny** klickar du på **Nätverk** och därefter på **Virtuellt nätverk**.

    ![Skärmbild som visar Microsoft Azure Marketplace med avsnittet Nätverk valt och Lastbalanserare markerat](../media/3-azure-marketplace-2.png)

1. På bladet **Skapa virtuellt nätverk** anger eller väljer du följande information:
    - **Namn:** _woodgrove-VNET_
    - **Adressutrymme:** _172.20.0.0/16_
    - **Prenumeration:** bör redan ha _Concierge-prenumeration_ valt.
    - **Resursgrupp:** välj den befintliga resursgruppen _<rgn>[Resursgruppsnamn]</rgn>_ i listan.
    - **Undernät:** _backendSubnet_
    - **Adressintervall för undernät:** _172.20.0.0/24_
    - **DDoS-skydd:** _Grundläggande_
    - **Tjänstens slutpunkter:** _Inaktiverat_

    ![Skärmbild som visar skärmen för att skapa ny lastbalanserare](../media/3-create-vnet.png)

1. Klicka på **Skapa**.

Medan det virtuella nätverket distribueras kan vi skapa ytterligare en sak: en nätverkssäkerhetsgrupp.

## <a name="create-and-configure-a-network-security-group"></a>Skapa och konfigurera en nätverkssäkerhetsgrupp

1. Klicka på **Skapa en resurs**

1. Välj gruppen **Nätverk** och klicka på objektet **Nätverkssäkerhetsgrupp**.

    ![Skärmbild som visar Microsoft Azure Marketplace med avsnittet Nätverk valt och Nätverkssäkerhetsgrupp markerat](../media/3-azure-marketplace-3.png)


1. Ge den namnet **woodgrove-NSG** och placera den i Azure-sandbox-resursgruppen.

1. Kontrollera att den finns på samma plats som Azure Load Balancer.

    ![Skärmbild som visar skärmen för att skapa nätverkssäkerhetsgrupp](../media/3-create-nsg.png)

1. Klicka på **Skapa**.

Vänta tills lastbalanseraren, det virtuella nätverket och NSG har distribuerats. Sedan kan vi konfigurera nätverkssäkerhetsgruppen.

## <a name="configure-the-network-security-group"></a>Konfigurera nätverkssäkerhetsgruppen

1. Välj den nya nätverkssäkerhetsgruppen – antingen i distributionsmeddelandet eller via sökfältet högst upp i Azure-portalen.

    ![Skärmbild som visar slutförd distribution på meddelandepanelen](../media/3-deployment-success.png)

1. Välj avsnittet **Inställningar** > **Ingående säkerhetsregler**. Observera att vi har tre fördefinierade regler som alltid används. Dessa regler är:
    - **AllowVnetInbound** – tillåter all intern trafik som flödar i det virtuella nätverket. Den här regeln gör att virtuella datorer som delar nätverk kan kommunicera med varandra.
    - **AllowAzureLoadBalancerInBound** – tillåter att lastbalanseraren ”pingar” tjänster i nätverket för att se om de är aktiva.
    - **DenyAllInbound** – nekar all annan trafik.

    Regeln **DenyAllInbound** är särskilt viktig – den säkerställer att all ingående trafik som inte har en regel med högre prioritet blockeras. Det är därför vi måste lägga till en ny regel som tillåter HTTP (80)-trafik på våra webbservrar.

    > [!NOTE]
    > Prioritet i NSG-regler går från 0–65500 och reglerna utvärderas i ordning. Den första matchande regeln är avgörande. Du vill alltid placera dina regler relativt lågt – med start vid cirka 1000 så att de har företräde framför de fördefinierade.

1. Klicka på **Lägg till** för att lägga till en ny regel.

    ![Skärmbild som visar ingående nätverkssäkerhetsregler med knappen Lägg till markerad](../media/3-inbound-security-rules.png)

1. Klicka på knappen **Basic** längst upp för att växla till vyn ”Basic”.

1. Ange uppgifterna för den nya regeln.
    - Välj _HTTP_ för **Tjänst**.
    - Ange **Prioritet** till _1000_.
    - Ge regeln ett namn (eller låt standardnamnet stå kvar).

    ![Skärmbild som visar dialogrutan Lägg till ingående säkerhetsregel ifylld med HTTP-information](../media/3-add-inbound-rule.png)

1. Klicka på **Lägg till** för att lägga till regeln.

    ![Skärmbild som visar den nyligen tillagda HTTP-regeln i listan NSG-regler](../media/3-new-added-rule.png)

## <a name="apply-the-network-security-group-to-the-vnet"></a>Tillämpa nätverkssäkerhetsgruppen på VNet

Nu ska vi tillämpa denna NSG på det virtuella nätverket.

1. Leta reda på det virtuella nätverket – du kan använda sökrutan överst. Namnet är **woodgrove-VNET**.

1. Välj **Inställningar** > **Undernät** för att komma till dina definierade undernät.

1. Klicka på posten **backendSubnet** för att öppna egenskaperna.

    ![Skärmbild som visar backendSubnet-posterna i området Undernät för undernäten](../media/3-subnets.png)

1. Klicka på posten ”Ingen” i **Nätverkssäkerhetsgrupp**

    ![Skärmbild som visar ett tomt Nätverkssäkerhet i backendSubnet](../media/3-add-network-security-group.png)

1. Välj **woodgrove-NSG** för att lägga till det i det här virtuella nätverket.

1. Klicka på **Spara** för att tillämpa ändringen.

Nu när vårt nätverk är klart kan vi skapa några virtuella datorer och placera dem i nätverket!
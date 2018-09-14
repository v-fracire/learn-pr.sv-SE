En av de största problemen med säkerhet att kunna se vilka områden som du behöver för att skydda och hitta sårbarheter innan hackare gör. Azure tillhandahåller en tjänst som gör det mycket enklare kallas Azure Security Center.

## <a name="what-is-azure-security-center"></a>Vad är Azure Security Center?

Azure Security Center (ASC) är en övervakningstjänst som ger skydd mot hot i alla dina tjänster både i Azure och lokalt. Det kan:

- Ange säkerhetsrekommendationer baserat på dina konfigurationer, resurser och nätverk.
- Övervaka säkerhetsinställningar i både lokala och molnbaserade arbetsbelastningar och automatiskt tillämpa den begärda på nya tjänster som de är online.
- Kontinuerligt övervaka alla dina tjänster och utföra automatisk säkerhetsutvärderingar för att identifiera potentiella problem innan de kan utnyttjas.
- Använda maskininlärning att identifiera och blockera skadlig kod installeras i dina tjänster och virtuella datorer. Du kan också lista för tillåten program så att endast de appar som du verifierar tillåts att köra.
- Analysera och identifiera eventuella inkommande attacker och bidra till att undersöka hot och alla aktiviteter efter intrång som har inträffat.
- Just-In-Time-åtkomstkontroll för portar, minska din attackyta genom att kontrollera att nätverket bara tillåter trafik som du behöver.

ASC är en del av den [Center för Internet Security](https://www.cisecurity.org/cis-benchmarks/) (CIS) rekommendationer.

## <a name="activating-azure-security-center"></a>Aktivera Azure Security Center

Azure Security Center erbjuder enhetlig säkerhetshantering och Avancerat skydd för arbetsbelastningar i hybridmoln och erbjuds på två nivåer: kostnadsfri och Standard. Den kostnadsfria nivån innehåller säkerhetsprinciper, utvärderingar och rekommendationer, medan Standard-nivån ger en robust uppsättning funktioner, inklusive hotinformation.

Fördelarna med ASC får har security-teamet på företaget beslutat att det aktiveras för alla prenumerationer på din arbetsplats. Du har fått ett e-postmeddelande i morse att aktivera den för dina program – så vi tittar på hur du gör.

1. Öppna den [Azure-portalen](https://portal.azure.com?azure-portal=true) och välj **Azure Security Center** i den vänstra menyn om du inte ser det, kan du välja **alla tjänster** och hitta  **Security Center** i avsnittet security enligt nedan.

![Öppna Azure Security Center](../media-draft/ASC-Menu.png)

2. Om du aldrig har öppnat ASC bladet börjar den **komma igång** post som kan be dig att uppgradera din prenumeration. Ignorera som för tillfället, Välj **hoppa över** längst ned på sidan och välj sedan **översikt**.
    - ”Big security bild” visas över alla element som finns i din prenumeration.
    - Detta har massor av bra information som du kan utforska.

3. Välj sedan **täckning**, under ”principer och efterlevnad”. Detta visar vad prenumeration element som ska omfattas (eller inte omfattas) av ACS. Här kan du aktivera på ACS för alla prenumerationer som du har åtkomst till. Försök att byta mellan de tre områdena täckning: ”inte motsvarar”, ”grundläggande täckning” och ”standardtäckning”.

4. Prenumerationer som inte omfattas får en uppmaning om att aktivera ACS. Du kan trycka på knappen ”uppgradera nu” för att aktivera ACS för alla resurser i prenumerationen.

![Uppgradera täckning](../media-draft/Upgrade-Now.png)

### <a name="free-vs-standard-pricing-tier"></a>Kostnadsfri version jämfört med Standard-prisnivå

Du kan använda en kostnadsfri Azure-prenumeration-nivå med ASC, är den begränsad till utvärderingar och rekommendationer för Azure-resurser endast. För att verkligen utnyttja ASC, behöver du uppgradera till en Standard-nivån prenumeration enligt ovan. Du kan uppgradera din prenumeration via knappen ”uppgradera nu” i den **täckning** bladet enligt vad som anges ovan. Du kan också växla till den **komma igång** bladet på menyn ASC som beskriver hur du byter din prenumeration. Priser och funktioner kan ändras baserat på regionen, du kan få en fullständig överblick den [prissättningssidan](https://azure.microsoft.com/en-us/pricing/details/security-center/). 

> [!NOTE]
> Om du vill uppgradera en prenumeration till standardnivån måste du vara tilldelad rollen som prenumerationsägare, prenumerationsdeltagare eller säkerhetsadministratör.

> [!IMPORTANT]
> När den 60 dagars utvärderingsperioden är slut, ASC är priset **$15/node per månad** och kommer att debiteras för ditt konto.

## <a name="turning-off-azure-security-center"></a>Om du inaktiverar Azure Security Center

För produktionssystem, kommer du definitivt vill behålla Azure Security Center aktiveras så att den kan övervaka alla dina resurser för hot. Men om du bara spelar med ASC och aktiverad, du kommer förmodligen vill inaktivera den för att se till att du inte debiteras. Låt oss göra det nu.

1. Öppna den [Azure-portalen](https://portal.azure.com?azure-portal=true) och välj **Azure Security Center** i den vänstra menyn om du inte ser det, kan du välja **alla tjänster** och hitta  **Security Center** i avsnittet security enligt nedan.

![Öppna Azure Security Center](../media-draft/ASC-Menu.png)

2. Välj **säkerhetsprincip** från den vänstra menyn.

3. Välj sedan **redigera inställningar >** bredvid den prenumeration som du vill nedgradera ASC.

4. På nästa skärm väljer du ”prisnivå” från den vänstra menyn.

5. En ny sida visas som ser ut som på bilden nedan. Klicka på rutan till vänster som säger ”kostnadsfri (för Azure-resurser endast)”.

![Prisnivå](../media-draft/Pricing-Tier.png)

6. Tryck på knappen ”Spara” längst upp på skärmen.

Du har nu nedgraderas prenumerationen till den kostnadsfria nivån av Azure Security Center.

## <a name="summary"></a>Sammanfattning

<!-- TODO: need link to module --> Grattis, du har vidtagit din första (och viktigaste) steg för att skydda dina program, data och nätverk! <!--If you want to learn more about Azure Security Center, you can go through the **Protect your resources with Azure Security Center** learning module.-->

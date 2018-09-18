Ett av de största problemen med säkerhet är att kunna se vilka områden som du behöver skydda och att hitta sårbarheter innan hackare gör det. Azure tillhandahåller en tjänst som gör det mycket enklare: Azure Security Center.

## <a name="what-is-azure-security-center"></a>Vad är Azure Security Center?

Azure Security Center (ASC) är en övervakningstjänst som ger skydd mot hot i alla dina tjänster både i Azure och lokalt. Det kan:

- Ge säkerhetsrekommendationer baserat på dina konfigurationer, resurser och nätverk.
- Övervaka säkerhetsinställningar i både lokala och molnbaserade arbetsbelastningar och automatiskt tillämpa den säkerhet som krävs på nya tjänster allteftersom de går online.
- Kontinuerligt övervaka alla dina tjänster och utföra automatiska säkerhetsutvärderingar för att identifiera potentiella sårbarheter innan de kan utnyttjas.
- Använda maskininlärning till att identifiera och blockera skadlig kod från att installeras i dina tjänster och virtuella datorer. Du kan även vitlista program så att endast de appar som du verifierar tillåts att köra.
- Analysera och identifiera eventuella inkommande attacker och bidra till att undersöka hot och alla aktiviteter efter intrång som eventuellt har inträffat.
- Just-In-Time-åtkomstkontroll för portar, vilket minskar din attackyta genom att se till att nätverket bara tillåter den trafik du behöver.

ASC är en del av rekommendationerna från [Center for Internet Security](https://www.cisecurity.org/cis-benchmarks/) (CIS).

## <a name="activating-azure-security-center"></a>Aktivera Azure Security Center

Tack vare fördelarna med ASC har säkerhetsteamet på företaget beslutat att det ska aktiveras för alla prenumerationer på din arbetsplats. Du fick ett e-postmeddelande i morse om att aktivera det för dina program – därför går vi igenom hur du gör.

1. Öppna [Azure-portalen](https://portal.azure.com?azure-portal=true) och välj **Azure Security Center** i den vänstra menyn. Om det inte syns där kan du välja **Alla tjänster** och hitta **Security Center** i säkerhetsavsnittet enligt det som visas nedan.

![Öppna Azure Security Center](../media-draft/ASC-Menu.png)

2. Om du aldrig har öppnat ASC börjar bladet på posten **Komma igång**, som kan be dig att uppgradera din prenumeration. Ignorera det för tillfället genom att välja **Hoppa över** längst ned på sidan och sedan välja **Översikt**.
    - ”big security picture” (övergripande om säkerhet) visas över alla element som finns i din prenumeration.
    - Där finns massor med bra information som du kan utforska.

3. Välj sedan **Täckning**, under ”Policy and Compliance” (Principer och efterlevnad). Detta visar vilka prenumerationselement som täcks (eller inte täcks) av ACS. Här kan du aktivera ACS för alla prenumerationer som du har åtkomst till. Försök att växla mellan de tre täckningsområdena: ”Not covered” (Täcks inte), ”Basic coverage” (Grundläggande täckning) och ”Standard coverage” (Standardtäckning).

4. Prenumerationer som inte täcks får en uppmaning om att aktivera ACS. Du kan trycka på knappen ”Upgrade Now” (Uppgradera nu) för att aktivera ACS för alla resurser i prenumerationen.

![Uppgradera täckning](../media-draft/Upgrade-Now.png)

### <a name="free-vs-standard-pricing-tier"></a>Prisnivån Free (Kostnadsfri) jämfört med Prisnivån Standard

Du kan använda en kostnadsfri Azure-prenumerationsnivå med ASC, men den är begränsad till utvärderingar och rekommendationer för Azure-resurser. För att verkligen utnyttja ASC behöver du uppgradera till en prenumeration på Standard-nivå enligt vad som visas ovan. Du kan uppgradera din prenumeration via knappen ”Upgrade Now” (Uppgradera nu) på bladet **Coverage** (Täckning) enligt vad som anges ovan. Du kan även växla till bladet **Komma igång** på ASC-menyn, där det beskrivs hur du byter prenumerationsnivå. Priser och funktioner kan variera beroende på region. Det finns en fullständig överblick på [prissättningssidan](https://azure.microsoft.com/en-us/pricing/details/security-center/). 

> [!NOTE]
> Om du vill uppgradera en prenumeration till standardnivån måste du vara tilldelad rollen som prenumerationsägare, prenumerationsdeltagare eller säkerhetsadministratör.

> [!IMPORTANT]
> När utvärderingsperioden på 60 dagar är slut kostar ASC **15 USD per nod och månad** och debiteras till ditt konto.

## <a name="turning-off-azure-security-center"></a>Stänga av Azure Security Center

För produktionssystem rekommenderas det starkt att du har Azure Security Center aktiverat så att det kan övervaka alla dina resurser efter hot. Men om du bara provkör ASC och aktiverade det bör du inaktivera det så att du inte debiteras. Vi tar och gör det nu.

1. Öppna [Azure-portalen](https://portal.azure.com?azure-portal=true) och välj **Azure Security Center** i den vänstra menyn. Om det inte syns där kan du välja **Alla tjänster** och hitta **Security Center** i säkerhetsavsnittet enligt det som visas nedan.

![Öppna Azure Security Center](../media-draft/ASC-Menu.png)

2. Välj **Säkerhetsprincip** i den vänstra menyn.

3. Sedan väljer du **Redigera inställningar >** intill den prenumeration som du vill nedgradera ASC för.

4. På nästa skärm väljer du ”Prisnivå” från den vänstra menyn.

5. En ny sida visas som ser ut som på bilden nedan. Klicka på rutan till vänster där det står ”Free (for Azure resources only)” (Kostnadsfri (endast för Azure-resurser)).

![Prisnivå](../media-draft/Pricing-Tier.png)

6. Klicka på knappen ”spara” längst upp på skärmen.

Du har nu nedgraderat prenumerationen till den kostnadsfria nivån för Azure Security Center.

## <a name="knowledge-check"></a>Kunskapskontroll
<!-- TODO: move into yaml --> Sätt en bock intill alla ASC-funktioner.

* Rekommendationer (korrekt)
* Åtgärder (falskt)
* Försvar (falskt)
* Just In Time (sant)
* Hotidentifiering (sant)

## <a name="summary"></a>Sammanfattning

<!-- TODO: need link to module --> Grattis! Du har tagit det första (och viktigaste) steget för att skydda programmet, data och nätverket! <!--If you want to learn more about Azure Security Center, you can go through the **Protect your resources with Azure Security Center** learning module.-->

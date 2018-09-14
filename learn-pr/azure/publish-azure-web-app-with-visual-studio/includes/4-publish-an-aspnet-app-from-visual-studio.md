Du har skapat en webbplats och vill distribuera den till Azure. Nu behöver vi överväga vilka Azure-tjänster som bäst passar våra behov. App Service är en mycket skalbar och automatiskt uppdaterad webbvärdtjänst för dina program.

Här tittar vi på hur du använder Visual Studio för att publicera din ASP.NET Core-webbapp i en Azure App Service-plan.

## <a name="azure-subscription"></a>Azure-prenumeration

Du behöver en Azure-prenumeration för att publicera i Azure. Du kan använda en [kostnadsfri Azure-prenumeration](https://azure.microsoft.com/free/) för att testa Azure App Service-funktionerna.

## <a name="what-is-azure-app-service"></a>Vad är Azure App Service?

## <a name="what-is-web-apps"></a>Vad är Web Apps?

Azure App Service är en tjänst som kan användas som värd för webbprogram, REST API:er och mobila serverdelar. App Service stöder kod skriven på många olika språk, som .NET, .NET Core, Java, Ruby, Node.js, PHP och Python. App Service lämpar sig utmärkt för de flesta webbplatser, särskilt om du inte behöver mycket kontroll över värdinfrastrukturen.

## <a name="what-is-the-app-service-plan"></a>Vad är App Service-planen?

App Service-planen definierar de beräkningsresurser som din app kommer att använda, var resurserna finns, hur många ytterligare resurser som planen kan använda samt typen av serviceplan som används. Dessa beräkningsresurser motsvarar servergruppen i konventionella webbvärdtjänster. En eller flera appar kan konfigureras för att köras inom samma App Service-plan.

När du distribuerar appar, kan du skapa en ny App Service-plan eller fortsätta att lägga till appar i en befintlig plan.  Appar i samma App Service-plan delar dock samma beräkningsresurser. För att avgöra om den nya appen har de resurser som krävs måste du känna till kapaciteten för den befintliga App Service-planen och den förväntade belastningen för den nya appen. Om en App Service-plan överbelastas kan det eventuellt medföra driftstopp för nya och befintliga appar.

Du kan definiera en App Service-plan i förväg i Azure Portal med PowerShell eller Azure CLI, eller så kan du konfigurera en när du publicerar ditt program i Visual Studio.

Varje App Service-plan definierar:

- Region (USA, västra, USA, östra och så vidare)
- Antal VM-instanser
- VM-instansernas storlek (liten, medel, stor)
- Prisnivå (Kostnadsfri, Delad, Basic, Standard, Premium, Premium V2, Isolerad)

## <a name="specify-the-region"></a>Ange regionen

När du skapar en App Service-plan måste du definiera en region eller en plats där planen kommer att finnas. Vanligtvis väljer du en region som är geografiskt nära dina förväntade kunder.

## <a name="what-are-the-pricing-and-reliability-levels"></a>Vilka är pris- och tillförlitlighetsnivåerna?

En viktig faktor när du distribuerar appar till Azure App Service är att välja rätt serviceplan. App Service-planen kan skalas upp eller ned när som helst. Du kan välja en lägre prisnivå först och skala upp senare när du behöver fler App Service-funktioner.

Det finns ett antal olika prisnivåer:

**Delad beräkning**: på de två grundnivåerna **Kostnadsfri** och **Delad** körs appar på samma virtuella Azure-dator som andra App Service-appar, inklusive andra kunders appar. Dessa nivåer allokerar CPU-kvoter till varje app som körs på de delade resurserna, och de resurserna kan inte skalas om.

Planerna Kostnadsfri och Delad passar bäst för små personliga projekt med mycket begränsad trafik. De har en fast gräns på 165 MB utgående data var 24:e timme.

**Dedikerad beräkning**: på nivåerna **Basic, Standard, Premium och Premium V2** körs appar på dedikerade virtuella Azure-datorer. Det är bara appar i samma App Service-plan som delar samma beräkningsresurser. Ju högre nivå, desto fler VM-instanser kan du skala ut.

Serviceplanen Standard är bäst lämpad för arbetsbelastningar som är live i produktion, där du publicerar kommersiella program till kunder.
Premium-serviceplanerna stöder webbappar med hög kapacitet där du vill undvika kostnaden för en dedikerad (isolerad) plan.

**Isolerad**: på den här nivån körs dedikerade virtuella Azure-datorer i dedikerade virtuella Azure-nätverk, vilket utöver beräkningsisolering även ger nätverksisolering för dina appar. Den här nivån har flest möjligheter till utskalning. Du väljer bara serviceplanen Isolerad om du har mycket höga krav på säkerhet och prestanda.

Isolera din app i en ny App Service-plan när:

- appen är resurskrävande
- du vill skala om appen oberoende av andra appar i den befintliga planen
- appen behöver resurser i en annan geografisk region.

## <a name="specify-the-resource-group"></a>Ange resursgruppen

Som med de flesta Azure-resurser behöver du ange vilken resursgrupp du vill använda. Du kan använda en befintlig resursgrupp eller skapa en ny direkt i Visual Studio. En resursgrupp är en logisk container där du distribuerar och hanterar Azure-resurser som webbappar, databaser och lagringskonton. Det är en användbar mekanism för att hålla associerade resurser tillsammans.

## <a name="deploy-your-web-app-from-visual-studio"></a>Distribuera webbappen från Visual Studio

Det går snabbt att publicera din app i Azure från Visual Studio.

### <a name="select-the-project"></a>Välj projektet

1. Högerklicka på projektet i Solution Explorer.

1. Välj **Publicera > Publicera till Azure**.

### <a name="configure-the-app-service-plan-in-windows"></a>Konfigurera App Service-planen i Windows

1. Konfigurera App Service-planen

    - Värd: du konfigurerar App Service-planen på den här fliken. Här anges plats, storlek och funktioner för den webbserver som är värd för din app. Du kan välja en befintlig värdplan eller skapa en ny. Windows genererar automatiskt globalt unika namn som du kan ändra under konfigurationen.
    - Tjänster: du kan konfigurera en SQL-databas för webbplatsen här.

        > [!NOTE]
        > Du kan ansluta och hantera en SQL-databas i Azure från Visual Studio, men det ligger utanför omfånget för den här modulen.

1. Klicka på **Skapa** för att distribuera appen. När det är klart startar Visual Studio den webbsida som är värd för webbplatsen.

### <a name="configure-the-app-service-plan-for-mac"></a>Konfigurera App Service-planen för Mac

1. Du kan välja en befintlig App Service-plan om du har en färdigt konfigurerad i Azure, eller så kan du skapa en ny.

1. Konfigurera App Service-planen på den här fliken. Här anges plats, storlek och funktioner för den webbserver som är värd för din app. Du kan välja en befintlig värdplan eller skapa en ny. Kom ihåg att namnet på din webbplats och alla resurser måste vara globalt unika i Azure.

1. Klicka på **Skapa** för att distribuera appen. När det är klart startar Visual Studio den webbsida som är värd för webbplatsen.

## <a name="summary"></a>Sammanfattning

ASP.NET Core-webbprogram och Azure App Service är en flexibel och skalbar värdlösning för dina ASP.NET-webbappar. Precis som med alla Azure-tjänster måste du ange en resursgrupp och välja den prisnivå som bäst passar dina behov.

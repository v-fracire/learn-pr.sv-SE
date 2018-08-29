## <a name="motivation"></a>Motivering
I Azure CLI kan du skriva kommandon och köra dem direkt. Kom ihåg att målet i det här utvecklingsexemplet är att distribuera nya versioner av en webbapp så att de kan testas. Det första steget är att skapa en resursgrupp. Kom ihåg att målet här är att skapa resurserna via en lokal installation av Azure CLI. 

I den här utbildningsenheten får du lära dig att använda Azure CLI till att logga in på din Azure-prenumeration och skapa en ny resurs.

## <a name="what-azure-resources-can-be-managed-using-the-azure-cli"></a>Vilka Azure-resurser kan jag hantera med Azure CLI?
Med Azure CLI kan du styra nästan alla aspekter av varje Azure-resurs. Du kan arbeta med resursgrupper, lagring, virtuella datorer, Azure Active Directory (Azure AD), containers, maskininlärning och så vidare.

Kommandona i CLI är ordnade i grupper och undergrupper. Varje grupp representerar en tjänst som tillhandahålls av Azure och undergrupperna delar upp kommandon för de här tjänsterna i logiska grupper. Till exempel innehåller gruppen **storage** undergrupper som **account**, **blob**, **storage** och **queue**.

Hur hittar jag då de kommandon jag behöver? Ett sätt är att använda **az find**. Om du till exempel vill hitta kommandon som hjälper dig att hantera en **lagringsblob** kan du använda det här find-kommandot:

```bash
az find -q blob
```

Om du redan känner till namnet på det kommando du vill använda så kan argumentet **--help** för kommandot vara användbart. Du ser då detaljerad information om kommandot, och för en kommandogrupp visas en lista med tillgängliga underkommandon. I vårt lagringsexempel kan du visa en lista med undergrupper och kommandon för hantering av bloblagring så här:

```bash
az storage blob --help
```

## <a name="how-to-create-an-azure-resource"></a>Skapa en Azure-resurs
Det ingår normalt tre steg när du ska skapa en ny resurs i Azure: anslut till Azure-prenumerationen, skapa resursen och kontrollera att den skapades.

![Steg för att skapa en resurs med Azure CLI](../media-drafts/4-create-resources-overview.png)

Varje steg motsvarar ett eget Azure CLI-kommando.

### <a name="connect"></a>Anslut
Eftersom du arbetar med en lokal installation av Azure CLI måste du autentisera dig innan du kan köra Azure-kommandon. Det gör du med Azure CLI-kommandot **login**. 

```bash
az login
```

Azure CLI öppnar normalt din standardwebbläsare och visar inloggningssidan för Azure. Om det inte fungerar följer du anvisningarna på kommandoraden och anger en auktoriseringskod på [https://aka.ms/devicelogin](https://aka.ms/devicelogin).

När inloggningen är färdig ansluts du till din Azure-prenumeration. 

### <a name="create"></a>Skapa
Du behöver ofta skapa en ny resursgrupp innan du skapar en ny Azure-tjänst, så vi använder resursgrupper som exempel för att visa hur du kan skapa Azure-resurser via CLI.

Du skapar en resursgrupp med Azure CLI-kommandot **group create**. Du måste ange ett namn och en plats. Namnet måste vara unikt inom prenumerationen. Platsen avgör var metadata för resursgruppen lagras. Du använder strängar som ”USA, västra”, ”Europa, norra” eller ”västra Indien” för att ange platsen. Du kan också använda konkatenerade ord som usavästra, europanorra eller indienvästra. Den grundläggande syntaxen ser ut så här:

```bash
az group create --name <name> --location <location>
```

### <a name="verify"></a>Verifiera
För många Azure-resurser har Azure CLI ett **list**-underkommando som visar information om resursen. Azure CLI-kommandot **group list** visar till exempel en lista med dina Azure-resursgrupper. Det här är användbart när du ska kontrollera att resursgruppen har skapats:

```bash
az group list
```

Du kan göra vyn tydligare genom att formatera utdata som en tabell:

```bash
az group list --output table
```

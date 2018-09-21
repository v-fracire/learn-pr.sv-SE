Med Azure CLI kan du skriva kommandon och köra dem direkt från kommandoraden. Kom ihåg att målet i det här utvecklingsexemplet är att distribuera nya versioner av en webbapp så att de kan testas. Nu ska vi titta på vilka slags uppgifter du kan utföra med Azure CLI.

## <a name="what-azure-resources-can-be-managed-using-the-azure-cli"></a>Vilka Azure-resurser kan jag hantera med Azure CLI?

Med Azure CLI kan du styra nästan alla aspekter av varje Azure-resurs. Du kan arbeta med resursgrupper, lagring, virtuella datorer, Azure Active Directory (Azure AD), containrar, maskininlärning och så vidare.

Kommandona i CLI är strukturerade i _grupper_ och _undergrupper_. Varje grupp representerar en tjänst som tillhandahålls av Azure och undergrupperna delar in kommandona för de här tjänsterna i logiska grupper. Till exempel innehåller gruppen `storage` undergrupper som **account**, **blob**, **storage** och **queue**.

Hur hittar jag de kommandon jag behöver? Ett sätt är att använda `az find`. Om du till exempel vill hitta kommandon som hjälper dig att hantera en lagringsblob kan du använda det här find-kommandot:

```azurecli
az find -q blob
```

Om du redan vet namnet på det kommando som du vill köra kan du använda argumentet `--help` för att visa mer detaljerad information om kommandot, eller visa en lista över de tillgängliga underkommandona för en kommandogrupp. I vårt lagringsexempel kan du visa en lista med undergrupper och kommandon för hantering av bloblagring så här:

```azurecli
az storage blob --help
```

## <a name="how-to-create-an-azure-resource"></a>Skapa en Azure-resurs

När du ska skapa en ny resurs i Azure gör du det vanligen i tre steg: ansluter till Azure-prenumerationen, skapar resursen och kontrollerar att den skapades. Följande illustration visar en översikt över den här processen.

![En bild som visar hur du skapar en Azure-resurs med hjälp av kommandoradsgränssnittet.](../media/4-create-resources-overview.png)

Varje steg motsvarar ett eget Azure CLI-kommando.

### <a name="connect"></a>Ansluta

Eftersom du arbetar med en lokal installation av Azure CLI måste du autentisera dig innan du kan köra Azure-kommandon. Det gör du med Azure CLI-kommandot **login**.

```azurecli
az login
```

Azure CLI öppnar normalt din standardwebbläsare och visar inloggningssidan för Azure. Om det inte fungerar följer du anvisningarna på kommandoraden och anger en auktoriseringskod på [https://aka.ms/devicelogin](https://aka.ms/devicelogin).

När inloggningen är färdig ansluts du till din Azure-prenumeration.

### <a name="create"></a>Skapa

Du behöver ofta skapa en ny resursgrupp innan du skapar en ny Azure-tjänst, så vi använder resursgrupper som exempel för att visa hur du kan skapa Azure-resurser via CLI.

Du skapar en resursgrupp med Azure CLI-kommandot **group create**. Du måste ange ett namn och en plats. Namnet måste vara unikt inom prenumerationen. Platsen avgör var metadata för resursgruppen lagras. Du använder strängar som ”USA, västra”, ”Europa, norra” eller ”västra Indien” för att ange platsen. Du kan också använda konkatenerade ord som usavästra, europanorra eller indienvästra. Den grundläggande syntaxen ser ut så här:

```azurecli
az group create --name <name> --location <location>
```

> [!IMPORTANT]
> Du behöver inte skapa en resursgrupp när du använder den kostnadsfria Azure sandbox-miljön. I stället använder du en resursgrupp som skapats i förväg.

### <a name="verify"></a>Verifiera

För många Azure-resurser har Azure CLI ett **list**-underkommando som visar information om resursen. Azure CLI-kommandot **group list** visar till exempel en lista med dina Azure-resursgrupper. Det här är användbart när du ska kontrollera att resursgruppen har skapats:

```azurecli
az group list
```

Du kan göra vyn tydligare genom att formatera utdata som en tabell:

```azurecli
az group list --output table
```

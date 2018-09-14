Nu när en virtuell dator har skapats kan vi få information om den med hjälp av andra kommandon.

Låt oss börja med `vm list`.

```azurecli
az vm list
```

Det här kommandot returnerar _alla_ virtuella datorer som definierats i den här prenumerationen. Utdata kan filtreras till en specifik resursgrupp med hjälp av parametern `--resource-group`. 

## <a name="output-types"></a>Utdatatyper
Observera att standardsvarstypen för alla kommandon som vi har gått igenom hittills är JSON. Det passar bra för skript, men många tycker att det är svårare att läsa. Du kan ändra utdataformatet för svar med hjälp av flaggan `--output`. Prova till exempel följande kommando i Azure Cloud Shell för att se ett annat utdataformat.

```azurecli
az vm list --output table
```

Tillsammans med `table` kan du ange `json` (standard), `jsonc` (färglagd JSON) eller `tsv` (tabellavgränsade värden). Prova några varianter med kommandot ovan för att se skillnaden.

## <a name="getting-the-ip-address"></a>Hämta IP-adressen

Ett annat användbart kommando är `vm list-ip-addresses`, som visar en lista över de offentliga och privata IP-adresserna för en virtuell dator. Om de ändras, eller om du inte hämtade dem när de skapades, kan du hämta dem när som helst.

```azurecli
az vm list-ip-addresses -n SampleVM -o table
```

Detta returnerar:

```
VirtualMachine    PublicIPAddresses    PrivateIPAddresses
----------------  -------------------  --------------------
SampleVM          168.61.54.62         10.0.0.4
```

> [!TIP]
> Observera att vi använder en förkortad syntax för `--output`-flaggan som `-o`. De flesta Azure CLI-kommandoparametrar kan förkortas till bara ett streck och en bokstav. `--name` kan exempelvis förkortas till `-n` och `--resource-group` till `-g`. Detta är praktiskt när du skriver, men vi rekommenderar att du använder det fullständiga alternativnamnet i skript för tydlighetens skull. Dokumentationen innehåller detaljerad information om varje kommando.

## <a name="getting-vm-details"></a>Hämta information om en virtuell dator

Vi kan få mer detaljerad information om en specifik virtuell dator baserat på namn eller ID med hjälp av kommandot `vm show`.

```azurecli
az vm show --resource-group ExerciseResources --name SampleVM
```

Detta returnerar ett ganska stort JSON-block med många olika typer av information om den virtuella datorn, inklusive anslutna lagringsenheter, nätverksgränssnitt och alla objekt-ID:n för resurser som den virtuella datorn är ansluten till. Även nu kan vi ändra till ett tabellformat, men om vi gör det utelämnas nästan alla intressanta data. I stället kan vi använda ett inbyggt frågespråk för JSON kallat [JMESPath](http://jmespath.org/).

## <a name="adding-filters-to-queries-with-jmespath"></a>Lägga till filter till frågor med JMESPath

JMESPath är ett branschstandardiserat frågespråk som är uppbyggt kring JSON-objekt. Den enklaste frågan är att ange en _identifierare_ som väljer en nyckel i JSON-objektet.

Anta att vi till exempel har objektet:

```json
{
  "people": [
    {
      "name": "Fred",
      "age": 28
    },
    {
      "name": "Barney",
      "age": 25
    },
    {
      "name": "Wilma",
      "age": 27
    }
  ]
}
```

Vi kan använda frågan `people` för att returnera matrisen med värden för `people`-matrisen. Om vi bara är intresserade av _en_ av personerna kan vi använda en indexerare. `people[1]` returnerar exempelvis:

```json
{
    "name": "Barney",
    "age": 25
}
```

Vi kan också lägga till specifika kvalificerare som returnerar en delmängd av objekten baserat på vissa kriterier. Till exempel returneras följande om kvalificeraren `people[?age > '25']` läggs till:

```json
[
  {
    "name": "Fred",
    "age": 28
  },
  {
    "name": "Wilma",
    "age": 27
  }
]
```

Slutligen kan vi begränsa resultaten genom att lägga till en ”select”: `people[?age > '25'].[name]` som bara returnerar namnen:

```json
[
  [
    "Fred"
  ],
  [
    "Wilma"
  ]
]
```

JMESQuery har flera andra intressanta frågefunktioner. När du har tid rekommenderar vi att du går [onlinesjälvstudien](http://jmespath.org/tutorial.html) som är tillgänglig på webbplatsen [JMESPath.org](http://jmespath.org/).

## <a name="filtering-our-azure-cli-queries"></a>Filtrera Azure CLI-frågorna

När vi har en grundläggande förståelse för hur JMES-frågor fungerar kan vi lägga till filter till de data som returneras av frågor som till exempel `vm show`-kommandot. Vi kan till exempel hämta administratörens användarnamn:

```azurecli
az vm show --resource-group ExerciseResources --name SampleVM --query "osProfile.adminUsername"
```

Vi kan hämta storleken som tilldelats till vår virtuella dator:

```azurecli
az vm show --resource-group ExerciseResources --name SampleVM --query hardwareProfile.vmSize
```

Och om du vill hämta alla ID:n för dina nätverksgränssnitt kan du använda frågan:

```azurecli
az vm show --resource-group ExerciseResources --name SampleVM --query "networkProfile.networkInterfaces[].id"
```

Den här frågetekniken fungerar med alla Azure CLI-kommandon och kan användas för att hämta specifika delar av data till kommandoraden. Tekniken kan även användas för skript – du kan till exempel hämta ett värde från ditt Azure-konto och lagra det i en miljö- eller skriptvariabel. Om du vill använda den på det här sättet är `--output tsv`-parametern en användbar flagga som du kan lägga till (som kan förkortas till `-o tsv`). När du gör det returneras resultaten som tabbavgränsade värden som endast innehåller själva datavärdena med tabbavgränsare.

Exempelvis returnerar

```azurecli
az vm show --resource-group ExerciseResources --name SampleVM --query "networkProfile.networkInterfaces[].id" -o tsv
```

texten: `/subscriptions/abc13b0c-d2c4-64b2-9ac5-2f4cb819b752/resourceGroups/ExerciseResources/providers/Microsoft.Network/networkInterfaces/SampleVMVMNic`

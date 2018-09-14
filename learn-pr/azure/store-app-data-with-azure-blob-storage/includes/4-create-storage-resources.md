När vi har en uppfattning om hur vi ska lagra data i lagringskonton, behållare och blobar kan vi tänka på de Azure-resurser som vi måste skapa för att appen ska fungera.

### <a name="storage-accounts"></a>Lagringskonton

Skapande av lagringskonton är en administrativ-/hanteringsaktivitet som utförs innan du distribuerar och kör din app. Konton skapas vanligtvis av ett konfigurationsskript för distribution eller miljöer, en Azure Resource Manager-mall, eller manuellt av en administratör. Andra program än administrationsverktyg bör generellt sätt inte ha behörighet att skapa lagringskonton.

### <a name="containers"></a>Containrar

Till skillnad från skapande av lagringskonton är skapandet av behållare en enkel aktivitet som kan utföras inifrån en app. Det är inte ovanligt att appar skapar och tar bort behållare som en del av sättet de arbetar.

För appar som förlitar sig på en känd uppsättning av behållare med hårdkodade eller förkonfigurerade namn är den vanliga metoden att låta appen skapa behållarna som behövs vid start eller vid första användning om de inte redan finns. Att låta appen skapa behållare i stället för att göra det som en del av distributionen av appen eliminerar behovet för både appen och distributionsprocessen att känna till namnen på behållarna som appen använder.

## <a name="exercise"></a>Övning

Vi kommer att färdigställa en oavslutat ASP.NET Core-app genom att lägga till kod för att använda Azure Blob Storage. Den här övningen handlar mer om att utforska API för Blob-lagring än om att utforma en organisations- och namngivningsschema, men här är en snabb överblick över appen och hur den lagrar data.

![Skärmbild av webbappen FileUploader](../media/4-fileuploader-with-files.PNG)

Vår app fungerar som en delad mapp som tar emot filuppladdningar och gör dem tillgängliga för nedladdning. Den använder inte en databas för att organisera blobar &mdash; i stället sanerar den namnen på de uppladdade filerna och använder dem som blobnamn direkt. Alla uppladdade filer lagras i en enskild behållare.

Koden vi börjar med kompileras och körs, men delarna som är ansvariga för att lagra och läsa in data är tomma. När vi har slutfört koden kan vi distribuera appen till Azure App Service och testa den.

Vi kan ställa in lagringsinfrastrukturen för vår app.

### <a name="storage-account"></a>Lagringskonto

[!include[](../../../includes/azure-sandbox-activate.md)]

[!include[](../../../includes/azure-sandbox-regions-first-mention-note.md)]

Använda Azure Cloud Shell-terminalen för att skapa ett lagringskonto. Du måste ange ett unikt namn för lagringskontot &mdash;. Gör en anteckning om det till senare.

```console
az storage account create --name <your-unique-storage-account-name> --resource-group <rgn>[Sandbox resource group name]</rgn> --location <location-name> --kind StorageV2
```

> [!NOTE]
> Varför `--kind StorageV2`? Det finns ett antal olika typer av lagringskonton. I de flesta fall bör du använda General-purpose v2-konton. Den enda anledningen till att du uttryckligen måste ange `--kind StorageV2` är att general-purpose v2-konton fortfarande är relativt nya och att de ännu inte är standard i Azure-portalen eller Azure CLI.

### <a name="container"></a>Container

Appen som vi kommer att arbeta med i den här modulen använder en enskild behållare. Vi ska följa regelverket och låta appen skapa behållaren vid start. Men skapandet av en behållare kan göras från Azure CLI: kör `az storage container create -h` i Cloud Shell-terminalen om du vill se dokumentationen.
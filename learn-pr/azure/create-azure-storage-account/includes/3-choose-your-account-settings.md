De inställningar för lagringskonton som vi redan har gått igenom gäller för datatjänsterna i kontot. Här diskuterar vi de tre inställningarna som tillämpas till själva kontot i stället för de data som lagras i kontot:

- Namn
- Distributionsmodell
- Typ av konto

De här inställningarna påverkar hur du hanterar ditt konto och kostnaden för tjänsterna i.

## <a name="name"></a>Namn

Varje lagringskonto har ett namn. Namnet måste vara globalt unikt, Använd bara gemena bokstäver och siffror och innehålla mellan 3 och 24 tecken.

## <a name="deployment-model"></a>Distributionsmodell

En _distributionsmodell_ är det system som Azure använder för att organisera dina resurser. Den definierar det API som du använder för att skapa, konfigurera och hantera resurserna. Azure tillhandahåller två distributionsmodeller:

- **Resource Manager**: den aktuella modellen som använder Azure Resource Manager API
- **Klassiska**: ett äldre erbjudande som använder Azure Service Management-API

Beslutet om vilket du väljer är vanligtvis enkelt eftersom de flesta Azure-resurser endast fungerar med Resource Manager. Lagringskonton, virtuella datorer och virtuella nätverk stöder dock både, och du måste välja en av när du skapar ditt storage-konto.

Den huvudsakliga skillnaden i funktioner mellan de två modellerna är stödet för gruppering. Resource Manager-modellen lägger till konceptet med en _resursgrupp_, som inte är tillgängliga i den klassiska modellen. Med en resursgrupp kan du distribuera och hantera en samling resurser som en enda enhet.

Microsoft rekommenderar att du använder **Resource Manager** för alla nya resurser.

## <a name="account-kind"></a>Typ av konto

Lagringskontots _typ_ är en uppsättning principer som bestämmer vilka datatjänster du kan inkludera i kontot samt prissättning för dessa tjänster. Det finns tre typer av lagringskonton:

- **StorageV2 (generell användning v2)**: det nuvarande erbjudandet som har stöd för alla lagringstyper och alla de senaste funktionerna
- **Storage (generell användning v1)**: en äldre typ som har stöd för alla lagringstyper som kanske inte stöder alla funktioner
- **BLOB-lagring**: en äldre typ som tillåter endast blockblobbar och tilläggsblobbar

Microsoft rekommenderar att du använder den **gpv2** alternativ för nya storage-konton.

Det finns några särskilda fall som kan utgöra undantag för den här regeln. Priser för transaktioner är till exempel lägre i generell användning v1, som du kan minska kostnaderna något om som matchar din normal belastning.

## <a name="summary"></a>Sammanfattning

Det viktigaste rådet här är att välja distributionsmodellen **Resource Manager** och kontotypen **StorageV2 (generell användning v2)** alla dina lagringskonton. De andra alternativen finns fortfarande tillgängliga, främst för att tillåta att befintliga resurser fortsätter köras. Det finns flera skäl till att tänka på de andra alternativen för nya resurser.
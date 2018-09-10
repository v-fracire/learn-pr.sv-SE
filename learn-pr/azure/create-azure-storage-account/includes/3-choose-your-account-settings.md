De inställningar för lagringskonton som vi redan har gått igenom gäller för datatjänsterna i kontot. Här diskuterar vi de tre inställningar som gäller för konton självt i stället för de data som lagras i kontot:

- Namn
- Distributionsmodell
- Typ av konto

Dessa påverkar hur du hanterar ditt konto och kostnaden för tjänsterna i det.

## <a name="name"></a>Namn

Varje lagringskonto har ett namn. Namnet måste vara globalt unikt. Det måste innehålla mellan 3 och 24 tecken och endast använda gemena bokstäver och siffror.

## <a name="deployment-model"></a>Distributionsmodell

En _distributionsmodell_ är det system som Azure använder för att organisera dina resurser. Den definierar det API som du använder för att skapa, konfigurera och hantera resurserna. Azure tillhandahåller två distributionsmodeller:

- **Resource Manager**: den aktuella modellen som använder Azure Resource Manager (ARM) API
- **Klassisk**: ett äldre erbjudande som använder Azure Service Management (ASM) API.

Beslutet om vilket du väljer är vanligtvis enkelt eftersom de flesta Azure-resurser endast fungerar med Resource Manager. Däremot stöder lagringskonton, virtuella datorer och virtuella nätverk båda två. Det innebär att du måste välja det ena eller det andra när du skapar ditt lagringskonto.

Den huvudsakliga skillnaden i funktioner mellan de två modellerna är stödet för gruppering. Resource Manager-modellen lägger till begreppet med en _resursgrupp_ som är inte tillgängligt i den klassiska modellen. Med en resursgrupp kan du distribuera och hantera en samling resurser som en enda enhet.

Microsoft rekommenderar att du använder Resource Manager för alla nya resurser.

## <a name="account-kind"></a>Typ av konto

Lagringskontots _typ_ är en uppsättning principer som bestämmer vilka datatjänster du kan inkludera i kontot samt prissättning för dessa tjänster. Det finns tre typer av lagringskonton:

- **StorageV2 (generell användning v2)**: det nuvarande erbjudandet som har stöd för alla lagringstyper och alla de senaste funktionerna
- **Storage (generell användning v1)**: en äldre typ som har stöd för alla lagringstyper som kanske inte stöder alla funktioner
- **Blob-lagring**: en äldre typ som endast tillåter blockblobar och tilläggsblobar.

Microsoft rekommenderar att du använder generell användning v2 för nya lagringskonton.

Det finns några särskilda fall som kan utgöra undantag för den här regeln. Till exempel är priserna för transaktioner lägre i generell användning v1, vilket du kan använda för att minska kostnaderna något om det matchar din normala arbetsbelastning.

## <a name="summary"></a>Sammanfattning

Det viktigaste rådet här är att välja distributionsmodellen **Resource Manager** och kontotypen **StorageV2 (generell användning v2)** alla dina lagringskonton. De andra alternativen finns fortfarande tillgängliga, främst för att tillåta att befintliga resurser fortsätter köras. För nya resurser finns det väldigt få anledningar att välja de andra alternativen.
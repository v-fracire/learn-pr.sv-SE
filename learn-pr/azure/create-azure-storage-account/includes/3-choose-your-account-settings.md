De inställningar för lagringskonton som vi redan har gått igenom gäller för datatjänsterna i kontot. Här diskuterar vi de tre inställningar som gäller för själva kontot snarare än för de data som lagras i kontot:

- Namn
- Distributionsmodell
- Typ av konto

Dessa inställningar påverkar hur du hanterar ditt konto och kostnaden för tjänsterna i det.

## <a name="name"></a>Namn

Varje lagringskonto har ett namn. Namnet måste vara globalt unikt. Det måste innehålla mellan 3 och 24 tecken och kan endast innehåll gemena bokstäver och siffror.

## <a name="deployment-model"></a>Distributionsmodell

En _distributionsmodell_ är det system som Azure använder för att organisera dina resurser. Den definierar det API som du använder för att skapa, konfigurera och hantera resurserna. Azure tillhandahåller två distributionsmodeller:

- **Resource Manager**: den aktuella modellen som använder Azure Resource Manager-API:et
- **Klassisk**: ett äldre erbjudande som använder Azure Service Management-API:et

Beslutet om vilket du väljer är vanligtvis enkelt eftersom de flesta Azure-resurser endast fungerar med Resource Manager. Lagringskonton, virtuella datorer och virtuella nätverk stöder dock båda, och du måste därför välja den ena eller den andra när du skapar ditt lagringskonto.

Den huvudsakliga skillnaden i funktioner mellan de två modellerna är stödet för gruppering. Resource Manager-modellen introducerar begreppet _resursgrupp_ som inte är tillgängligt i den klassiska modellen. Med en resursgrupp kan du distribuera och hantera en samling resurser som en enda enhet.

Microsoft rekommenderar att du använder Resource Manager för alla nya resurser.

## <a name="account-kind"></a>Typ av konto

Lagringskontots _typ_ är en uppsättning principer som bestämmer vilka datatjänster du kan inkludera i kontot samt prissättning för dessa tjänster. Det finns tre typer av lagringskonton:

- **StorageV2 (generell användning v2)**: det nuvarande erbjudandet som har stöd för alla lagringstyper och alla de senaste funktionerna
- **Lagring (generell användning v1)**: en äldre typ som har stöd för alla lagringstyper som kanske inte stöder alla funktioner
- **Bloblagring**: en äldre typ som endast tillåter blockblobar och tilläggsblobar

Microsoft rekommenderar att du använder generell användning v2 för nya lagringskonton.

Det finns några särskilda fall som kan utgöra undantag till den här regeln. Till exempel är priserna för transaktioner lägre med generell användning v1, vilket gör att du kan minska kostnaderna något om detta passar din normala arbetsbelastning.

## <a name="summary"></a>Sammanfattning

Det viktigaste rådet här är att välja distributionsmodellen **Resource Manager** och kontotypen **StorageV2 (generell användning v2)** alla dina lagringskonton. De andra alternativen finns fortfarande tillgängliga, främst för att tillåta att befintliga resurser fortsätter köras. För nya resurser finns det få anledningar att välja de andra alternativen.
Du är nu redo att börja implementera temperaturtjänsten. I den föregående delen fastställde du att en lösning utan server skulle passa bäst för dina behov. Det behövs en ”utgångspunkt” för att implementera en Azure-funktion utan server. En Azure-funktionsapp är en sådan utgångspunkt.

## <a name="azure-function-app-overview"></a>Översikt över en Azure-funktionsapp
Azure-funktioner finns i en behållare som kallas funktionsapp. Du kan definiera funktionsappar i Azure för att gruppera och strukturera funktionerna på ett logiskt sätt. Funktionsappar är en beräkningsresurs i Azure. I vårt exempel med en hiss skulle en funktionsapp ha skapats som värd för temperaturtjänsten för maskineriets drivhjul. Det finns några beslut som måste fattas för att skapa en funktionsapp. Du måste välja en tjänstplan och ett kompatibelt lagringskonto.

### <a name="choosing-a-service-plan"></a>Välja en tjänstplan
Funktionsappar kan användas med en av två typer av tjänstplaner. Den första är förbrukningsplanen. Välj den här planen om du använder Azures programplattform utan server. Förbrukningsplanen innehåller automatisk skalning, och du debiteras när funktionerna körs. I förbrukningsplanen ingår en konfigurerbar tidsgräns för funktionens körning. Den är som standard 5 minuter, men kan konfigureras för upp till 10 minuter. 

Den andra planen är Azure App Service-planen. Med den här planen kan din funktion köras kontinuerligt på en virtuell dator. Välj det här alternativet om dina funktioner används kontinuerligt eller kräver mer processorkraft eller körningstid än vad som ingår i förbrukningsplanen. 

### <a name="storage-account-requirements"></a>Krav för lagringskonto
När du skapar en funktionsapp är den vanligtvis länkad till ett lagringskonto med stöd för Azure Blob, Queue och Table Storage. Lagringskontot används av funktionsappen vid interna åtgärder som att logga funktionskörningar och hantera körningsutlösare. Med förbrukningsplanen är det även här som Azures funktionskod och konfigurationsfiler lagras. 

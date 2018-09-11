Containrar är på väg att bli det bästa sättet att paketera, distribuera och hantera molnprogram. Azure Container Instances erbjuder det snabbaste och enklaste sättet att köra en behållare i Azure, utan att behöva hantera några virtuella datorer och utan att behöva använda en tjänst på högre nivå.

Azure Container Instances är en bra lösning för alla scenarier som kan fungera i isolerade behållare, däribland enkla program, automatisering av uppgifter och att skapa jobb. För scenarier där du behöver fullständig containerorkestrering, som tjänstidentifiering för flera containers, automatisk skalning och koordinerade programuppgraderingar rekommenderar vi Azure Kubernetes Service (AKS).

Azure Container Instances har följande fördelar:

- **Snabb start**: Azure Container Instances kan starta containers i Azure på bara några sekunder.
- **Fakturering per sekund**: du debiteras bara när containern körs.
- **Säkerhet på Hypervisornivå**: Azure Container Instances garanterar att ditt program är lika isolerat i en behållare som i en virtuell dator.
- **Anpassade storlekar**: Azure Container Instances erbjuder optimal användning eftersom det tillåter exakta specifikationer av CPU-kärnor och minne.
- **Beständig lagring**: Vi erbjuder direkt montering av Azure Files-resurser för att hämta och bevara tillstånd med Azure Container Instances.
- **Linux och Windows**: Azure Container Instances kan schemalägga både Windows- och Linux-behållare med samma API.

## <a name="learning-objectives"></a>Utbildningsmål  

I den här modulen kommer du att göra följande:

- köra en container i Azure Container Instances.
- kontrollera hur containern startas om.
- använda miljövariabler i din container.
- montera datavolymer till Azure Container Instances.
- lära dig mer om felsökning av Azure Container Instances.

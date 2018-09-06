Containers används allt mer till att paketera, distribuera och hantera molnprogram. Azure Container Instances är det snabbaste och enklaste sättet att köra en container i Azure, utan att du behöver hantera några virtuella datorer och utan att behöva använda någon högnivåtjänst.

Azure Container Instances (ACI) är en utmärkt lösning för alla scenarier som kan fungera i isolerade containers, som enkla appar, automatisering av uppgifter och att skapa jobb. För scenarier där du behöver fullständig containerorkestrering, som tjänstidentifiering för flera containers, automatisk skalning och koordinerade programuppgraderingar rekommenderar vi Azure Kubernetes Service (AKS).

Azure Container Instances har följande fördelar:

- **Snabb start**: ACI kan starta containers i Azure på bara några sekunder.
- **Fakturering per sekund**: du debiteras bara när containern körs.
- **Säkerhet på hypervisornivå**: ACI garanterar att din app är lika isolerad i en container som i en virtuell dator.
- **Anpassad storlek**: med ACI får du en optimal användning eftersom du kan ange antal processorkärnor och mängden minne exakt.
- **Beständig lagring**: vi erbjuder direkt montering av Azure Files-resurser för att hämta och bevara tillstånd med ACI.
- **Linux och Windows**: ACI kan schemalägga både Windows- och Linux-containers via samma API.
 
I den här modulen kommer du att:

- köra en container i Azure Container Instances
- kontrollera hur containern startas om
- använda miljövariabler i din container
- montera datavolymer vid Azure Container Instances
- lära dig mer om felsökning av Azure Container Instances.

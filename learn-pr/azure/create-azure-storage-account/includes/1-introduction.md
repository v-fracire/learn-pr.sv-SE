De flesta organisationer har flera olika krav för deras molnbaserade data. Till exempel lagra data i en viss region eller behöva separat debitering för olika kategorier. Med Azure-lagringskonton kan du formalisera dessa typer av principer och tillämpa dem på dina Azure-data.

Anta att du arbetar på en chokladfabrik som producerar bageriingredienser, till exempel kakaopulver och chokladbitar. Du marknadsför dina produkter till livsmedelsbutiker som sedan säljer dem till konsumenter.

Dina formler och tillverkningsprocesser är affärshemligheter. De kalkylblad, dokument och instruktionsvideor som lagrar den här informationen är kritiskt viktiga för din verksamhet och kräver geografiskt redundant lagring. De här data läses främst från din huvudfabrik, så du vill lagra dem i ett närliggande datacenter. Kostnaden för den här lagringen måste faktureras till tillverkningsavdelningen.

Du har även en försäljningsgrupp som skapar recept på kakor och videor med bakningsinstruktioner för att marknadsföra dina produkter till konsumenter. Din prioritet för dessa data är låg kostnad i stället för redundans eller plats. Den här lagringen måste faktureras till säljteamet.

Här visas hur du hanterar dessa typer av affärskrav genom att skapa flera Azure-lagringskonton. Varje lagringskonto har lämpliga inställningar för de data som det innehåller.

## <a name="learning-objectives"></a>Utbildningsmål

I den här modulen kommer du att göra följande:

 - Bestämma hur många lagringskonton du behöver för projektet
 - Fastställa lämpliga inställningar för varje lagringskonto
 - Skapa ett lagringskonto med Azure-portalen
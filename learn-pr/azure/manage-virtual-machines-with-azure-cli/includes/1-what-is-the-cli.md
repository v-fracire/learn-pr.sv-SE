Jim hanterar en uppsättning virtuella Azure-datorer som körs på företagets webbinfrastruktur, som omfattar flera webbplatser och databasservrar som körs på olika plattformar. 

Även om Azure-portalen är lätt att använda har Jim märkt att det är tidskrävande att navigera bland de olika bladen för några av uppgifterna. 

Medan han tittar på alternativen upptäcker han verktyget Azure CLI.

När Jim arbetar med Azure CLI kan han skriva skript för att kontrollera servrarnas status, distribuera en ny konfiguration, öppna en port eller ansluta till en virtuell dator och ändra en inställning.

Du kanske precis som Jim är ute efter ett verktyg som hjälper till att automatisera uppgifter i din molnmiljö. Vi ska visa hur du använder Azure CLI för att skapa och hantera virtuella datorer i Azure. 

## <a name="azure-cli"></a>Azure CLI

Azure CLI är Microsofts plattformsoberoende kommandoradsverktyg för att hantera Azure-resurser. Det finns för macOS, Linux och Windows eller i webbläsaren med [Azure Cloud Shell](https://docs.microsoft.com/azure/cloud-shell/overview).

> [!IMPORTANT]
> I dag finns det två tillgängliga versioner av verktyget Azure CLI: Azure CLI 1.0 och Azure CLI 2.0. Vi använder Azure CLI 2.0, som är den senaste versionen och rekommenderas om du inte kör äldre skript. Azure CLI 1.0 startas med kommandot `azure`, och Azure CLI 2.0 startas med kommandot `az`. 

Med Azure CLI kan du hantera Azure-resurser som virtuella datorer och diskar från kommandoraden eller i skript. Vi sätter igång och ser vad vi kan göra.

## <a name="learning-objectives"></a>Utbildningsmål

I den här modulen kommer du att göra följande:

- Skapa en virtuell dator med Azure CLI
- Ändra storlek på virtuella datorer med Azure CLI
- Utföra grundläggande hanteringsåtgärder med Azure CLI.
- Ansluta till en aktiv virtuell dator med SSH och Azure CLI.

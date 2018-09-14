Anta att du arbetar för ett företag som gör videon bearbetning och analys för mönstret. Du skapar en ny prototyp plattform för att bearbeta videon från trafik kameror, analysera trender och ger användbara data för trafik och väg förbättringar. 

Du har vidtagit åtgärder med flera nya städer att samla in sina trafikdata kamera för att förbättra dina algoritmer. Men inte alla data som video är i samma format och många av format som endast har Windows-codec för att avkoda data. Därför bör har du valt att använda virtuella datorer (VM) för att göra den inledande bearbetningen och sedan skicka tillbaka till Azure functions som bearbetar ett standardformat. Den här metoden kan du ta på nya dataformat dynamiskt utan att stoppa hela systemet.

Azure tillhandahåller en robust virtuell dator med en värdlösning som uppfyller dina behov. Låt oss se hur du skapar och arbetar med virtuella Windows-datorer i Azure.

## <a name="learning-objectives"></a>Utbildningsmål

I den här modulen kommer du att göra följande:

- Lära dig om de tillgängliga alternativen för virtuella datorer i Azure.
- Skapa en virtuell Windows-dator med Azure Portal.
- Ansluta till en aktiv virtuell Windows-dator med hjälp av fjärrskrivbord.
- Installera programvara och ändra nätverkskonfigurationen på en virtuell dator med Azure Portal.
Anta att du arbetar på ett företag som bearbetar data och lagring från trafikkameror. Videoströmmarna analyseras, kategoriseras och bearbetas för att identifiera ansikten och registreringsskyltar vid specifika tidpunkter. Den här informationen överförs sedan till Azure Data Lake och ett sökbart index genereras för juridiska krav.

Dessa videoströmmar använder en mängd olika codecs och upplösningar. Du måste köra flera Windows-baserade tillverkarspecifika programvarupaket för att utföra den inledande bearbetningen och koda dem till ett vanligt videoformat. Eftersom det ständigt släpps nya format är det bäst att bearbeta videomaterialet på virtuella datorer. Då kan tillverkarspecifika paket läggas till och uppdateras utan att hela systemet behöver stoppas.

Azure tillhandahåller en robust virtuell dator som värdlösning som uppfyller dina behov. Låt oss se hur du skapar och arbetar med virtuella Windows-datorer i Azure.

## <a name="learning-objectives"></a>Utbildningsmål

I den här modulen kommer du att göra följande:

- Förstå de tillgängliga alternativen för virtuella datorer i Azure.
- Skapa en virtuell Windows-dator med Azure Portal.
- Anslut till en aktiv virtuell Windows-dator med hjälp av fjärrskrivbord.
- Installera programvara och ändra nätverkskonfigurationen på en virtuell dator med Azure Portal.
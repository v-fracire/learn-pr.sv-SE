Anta att du arbetar på ett företag som bearbetar data från trafikkameror. Videoströmmarna analyseras, kategoriseras och bearbetas för att identifiera ansikten och registreringsskyltar vid specifika tidpunkter. Den här informationen överförs sedan till Azure Data Lake och ett sökbart index genereras.

Dessa videoströmmar använder en mängd olika codecs och upplösningar. Du måste köra flera Windows-baserade tillverkarspecifika programvarupaket för att utföra den inledande bearbetningen och koda dem till ett vanligt videoformat. Eftersom det ständigt släpps nya format är det bäst att bearbeta videomaterialet på virtuella datorer. Då kan tillverkarspecifika paket läggas till och uppdateras utan att hela systemet behöver stoppas.

Du måste ansluta till användargränssnittet för att administrera kodningsprogramvaran på dina virtuella Windows-datorer.

I den här modulen får du lära dig hur du skapar en virtuell Windows-dator och ansluter till den med Remote Desktop Protocol (RDP).

## <a name="learning-objectives"></a>Utbildningsmål
> [!div class="checklist"]
> * Skapa en virtuell Windows-dator med Azure Portal.
> * Förstå de tillgängliga alternativen för virtuella datorer i Azure.
> * Anslut till en aktiv virtuell Windows-dator med hjälp av Windows fjärrskrivbordsanslutning.

## <a name="prerequisites"></a>Nödvändiga komponenter

- Ha kunskap om Azure Cloud Services-miljön.
- Vara bekant med virtuella datorer.

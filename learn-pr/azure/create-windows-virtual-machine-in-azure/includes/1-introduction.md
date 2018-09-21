Anta att du arbetar för ett företag som sysslar med videodatabearbetning och mönsteranalys. Du skapar en ny plattformsprototyp för att bearbeta video från trafikkameror, analysera trender och ge användbara data för trafik- och vägförbättringar. 

För att förbättra dina algoritmer har du avtalat med flera nya städer att samla in deras data från trafikkameror. Men alla videodata har inte samma format och många av formaten har bara Windows-codec för att avkoda data. Därför har du bestämt dig för att använda virtuella datorer för att utföra den första bearbetningen och sedan skicka data till Azure-funktioner som bearbetar ett standardformat. Med den här metoden kan du ta med nya dataformat dynamiskt utan att stoppa hela systemet.

Azure tillhandahåller en robust virtuell dator med en värdlösning som uppfyller dina behov. Låt oss se hur du skapar och arbetar med virtuella Windows-datorer i Azure.

## <a name="learning-objectives"></a>Utbildningsmål

I den här modulen kommer du att göra följande:

- Lära dig om de tillgängliga alternativen för virtuella datorer i Azure.
- Skapa en virtuell Windows-dator med Azure Portal.
- Ansluta till en aktiv virtuell Windows-dator med hjälp av fjärrskrivbord.
- Installera programvara och ändra nätverkskonfigurationen på en virtuell dator med Azure Portal.

## <a name="prerequisites"></a>Krav

- Grundläggande förståelse av Azure-datorer från **Introduktion till Azure Virtual Machines**
- Fjärrskrivbordsklient
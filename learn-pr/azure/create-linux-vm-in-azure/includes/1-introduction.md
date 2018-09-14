Du har anställts av ett global bilracingföretag för att modernisera hela deras webbplattform. De har bestämt sig för att ersätta sina befintliga Linux-servrar med olika typer av molnbaserad infrastruktur som utnyttjar det senaste i arkitekturtrender. En del av systemet ska köras på Azures serverlösa plattform med Azure Functions för att bearbeta realtidsdata för racing, där statistik, racingdata och andra relevanta delar av analyserad information överförs till kluster av databaser. Företaget vill behålla sin befintliga webbplats, som skrevs om förra året, men ansluta den till den här moderna dataströmmen.

Webbplatsen körs på Apache med Linux och eftersom den redan är igång bestämmer du dig för att flytta den direkt till Azure genom att använda en virtuell Azure-dator. Då får den åtkomst till data med en minimal mängd arbete från din sida.

## <a name="learning-objectives"></a>Utbildningsmål

I den här modulen kommer du att göra följande:

- Lära dig om de tillgängliga alternativen för virtuella datorer i Azure.
- Skapa en virtuell Linux-dator med Azure Portal.
- Ansluta till en virtuell Linux-dator med SSH.
- Installera programvara och ändra nätverkskonfigurationen på en virtuell dator med Azure Portal.
Du är systemarkitekter för en Advokatbyrå. Företaget har bett dig att migrera viktiga system till Azure. Inkluderar databasen för fallet Historik finns för närvarande av en lokal SQLServer och nås från ett skrivbordsprogram. SQLServer körs även vissa anpassade interna tjänster för att underhålla databasen. Du har bestämt dig för att en lösning som baseras på Azure virtual machines (VM) gör att du kan vara värd för din SQLServer och fortsätta att använda dina anpassade tjänster. Du ska skapa en Azure virtuell hårddisk baserat på innehållet i din befintliga lokala-server för att underlätta migreringen.

I den här modulen kommer du lära dig hur du skapar den optimala diskkonfigurationen för de virtuella datorerna som du skapar i Azure. Du skapar virtuella hårddiskar via Azure portal och lägga till dem i virtuella datorer för att skapa helt funktionella beräkningsresurser.

## <a name="learning-objectives"></a>Utbildningsmål

I den här modulen kommer du att göra följande:

- Skapa en virtuell dator i Azure portal
- Skapa operativsystem diskar och datadiskar i virtuella Azure-datorer
- Välj mellan och skapa standard och premium-diskar
- Välj mellan och skapa ohanterade och hanterade diskar
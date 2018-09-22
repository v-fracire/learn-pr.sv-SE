Du är systemarkitekt för en advokatbyrå. Företaget har bett dig att migrera kritiska system till Azure. Åtgärderna innefattar databasen med målhistorik, som för närvarande hanteras av en lokal SQL-server och nås från ett skrivbordsprogram. SQL-servern kör även vissa anpassade interna tjänster för att underhålla databasen. Du anser att en lösning som baseras på virtuella Azure-datorer gör att du kan hantera SQL-servern och fortsätta använda de anpassade tjänsterna. Du skapar en virtuell Azure-hårddisk baserat på innehållet i din befintliga lokala server för att underlätta migreringen.

I den här modulen lär di dig att utforma den optimala diskkonfigurationen för de virtuella datorer som du skapar i Azure.

## <a name="learning-objectives"></a>Utbildningsmål

I den här modulen ska du:

- Skapa en virtuell dator (VM)
- Konfigurera och ansluta virtuella hårddiskar (VHD) till en befintlig virtuell dator
- Fastställa om du behöver premiumdiskar
- Ändra storlek på diskar för en virtuell dator

## <a name="prerequisites"></a>Krav  

Ingen
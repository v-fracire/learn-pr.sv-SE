Att skapa administrationsskript är ett kraftfullt sätt att optimera ditt arbetsflöde på. Du kan automatisera vanliga, repetitiva uppgifter och när ett skript har verifierats kan det kan köras konsekvent, vilket minskar risken för fel.

Anta att du arbetar på ett företag som använder virtuella Azure-datorer till att testa CRM-programvara. De virtuella datorerna skapas från avbildningar som inkluderar en webbklientdel, en webbtjänst som implementerar affärslogik och en SQL-databas.

Du har kört flera tester på en enskild virtuell dator och märkt att ändringar i databasen och konfigurationsfilerna kan orsaka inkonsekventa resultat. I ett fall skapades en post för telefonsamtal i en bugg utan att någon motsvarande kund fanns i databasen. Den överblivna posten orsakade att efterföljande integreringstester misslyckades även när felet hade åtgärdats. Du tänker lösa problemet med hjälp av en ny distribution av virtuella datorer för varje testcykel. Du vill automatisera skapandet av en virtuell dator eftersom det ska utföras flera gånger per vecka. 

Här visar vi hur du hanterar Azure-resurser med Azure PowerShell. Du använder Azure PowerShell interaktivt för engångsuppgifter och skriver skript för att automatisera återkommande uppgifter. 

## <a name="learning-objectives"></a>Utbildningsmål
I den här modulen kommer du att göra följande:

- Avgöra om Azure PowerShell är rätt verktyg för dina Azure-administrationsuppgifter
- Installera Azure PowerShell på Linux, macOS och/eller Windows
- Ansluta till en Azure-prenumeration med hjälp av Azure PowerShell
- Konfigurera Azure-resurser med Azure PowerShell

## <a name="prerequisites"></a>Förutsättningar

- Erfarenhet av ett kommandoradsgränssnitt, till exempel PowerShell eller Bash
- Kunskaper om grundläggande Azure-begrepp, som resursgrupper och virtuella datorer
- Erfarenhet av att administrera Azure-resurser med hjälp av Azure-portalen

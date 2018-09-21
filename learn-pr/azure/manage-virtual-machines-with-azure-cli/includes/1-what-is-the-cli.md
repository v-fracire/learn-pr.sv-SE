Det är vanligt att IT-avdelningar hanterar ett stort antal Azure-resurser – allt från virtuella Azure-datorer till hanterade webbplatser.

Azure-portalen är enkel att använda för engångsuppgifter, men det tar tid att navigera genom de olika bladen när du behöver skapa, ändra eller ta bort flera saker. Det här är som kommandoraden kommer till sin rätt – du kan skicka kommandon snabbt och effektivt eller till och med använda skript för att köra repetitiva uppgifter. Med Azure har du två olika kommandoradsverktyg att arbeta med: Azure PowerShell och Azure CLI.

Med endera av dessa verktyg kan du skriva skript för att kontrollera status för molnservrar, distribuera nya konfigurationer, öppna portar i brandväggen eller ansluta till en virtuell dator för att ändra en inställning. Windows-administratörer brukar föredra Azure PowerShell, medan utvecklare och Linux-administratörer ofta använder Azure CLI.

Den här modulen fokuserar på användning av Azure CLI för att skapa och hantera virtuella datorer i Azure. Om du vill få en översikt över Azure CLI, hur du installerar det samt hur du arbetar med dina Azure-prenumerationer kan du titta på utbildningsmodulen **Kontrollera Azure-tjänster med CLI**.

## <a name="what-is-the-azure-cli"></a>Vad är Azure CLI?

Azure CLI är Microsofts plattformsoberoende kommandoradsverktyg för att hantera Azure-resurser. Det finns för macOS, Linux och Windows eller i webbläsaren med [Azure Cloud Shell](https://docs.microsoft.com/azure/cloud-shell/overview).

Med Azure CLI kan du hantera Azure-resurser som virtuella datorer och diskar från kommandoraden eller i skript. Vi kör igång och tittar på vad du kan göra med virtuella Azure-datorer.

## <a name="learning-objectives"></a>Utbildningsmål

I den här modulen kommer du att göra följande:

- Skapa en virtuell dator med Azure CLI
- Ändra storlek på virtuella datorer med Azure CLI.
- Utföra grundläggande hanteringsåtgärder med Azure CLI.
- Ansluta till en aktiv virtuell dator med SSH och Azure CLI.

## <a name="prerequsites"></a>Förutsättningar

- Grundläggande kunskaper om Azure CLI-verktyget från modulen **Kontrollera Azure-tjänster med CLI**.
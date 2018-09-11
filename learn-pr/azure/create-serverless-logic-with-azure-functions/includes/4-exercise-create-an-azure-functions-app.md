I den här övningen skapar vi en Azure-funktionsapp.

1. Logga in på [Azure Portal](https://portal.azure.com?azure-portal=true) med ditt Azure-konto.
1. Välj knappen **Skapa en resurs** längst upp till vänster på Azure Portal och sedan **Beräkning > Funktionsapp** för att öppna bladet Funktionsapp *Skapa*.
  ![Skärmbild av Azure Portal med fokus på valet av *Skapa en resurs* följt av *Beräkning* och *Funktionsapp*](../images/4-create-function-app-blade.png)
1. Välj ett globalt unikt appnamn. Det kommer att fungera som bas-URL för din tjänst. Du kan till exempel kalla den **escalator-functions-xxxxxxx**, där kryssen kan ersättas med dina initialer och ditt födelseår. Om den ändå inte är globalt unik kan du testa vilken kombination som helst. Giltiga tecken är a-z, 0-9 och -.
1. Välj den Azure-prenumeration som du vill ska vara värd för funktionsappen.
1. Skapa en ny resursgrupp som kallas **escalator-functions-group**. Genom att använda en resursgrupp för alla resurser som används i den här modulen blir det lättare att rensa senare.
1. Välj **Windows** som OS.
1. Välj **Förbrukningsplan** som **Värdplan** så att vi kan dra nytta av de serverlösa funktionerna i Azure.
1. Välj den geografiska plats som är närmast dig (eller dina kunder).
1. Skapa ett nytt lagringskonto och namnge det **escalator functions**.
1. Se till att Azure Application Insights är **På** och välj regionen som är närmast dig (eller dina kunder).
När du är klar bör konfigurationen se ut som på följande skärmbild.

  ![Skärmbild av konfigurationsskärmen Funktionsapp *Skapa* med alla fält konfigurerade enligt instruktionerna ovan.](../images/4-create-function-app-settings.png)

1. Välj **Skapa**. Distributionen kan ta några minuter. Du får ett meddelande när den är klar.

## <a name="verify-your-azure-function-app"></a>Verifiera din Azure-funktionsapp

1. Välj **Resursgrupper** i den vänstra menyn i Azure-portalen. Du bör se **escalator-functions-group** i listan över tillgängliga grupper.
  ![Skärmbild av skärmen Resursgrupper på Azure Portal med resursgruppen **escalator-functions-group** synlig.](../images/4-resource-group.png)
1. Välj **escalator-functions-group**. Du bör se en lista över resurser som liknar följande lista.
  ![Skärmbild av alla resurser i gruppen **escalator-functions-group**, inklusive poster för App Service-plan, lagringskonto, Application Insights och App Service](../images/4-resource-list.png)

Objektet med blixtikonen, som visas som en App Service, är den nya funktionsappen. 

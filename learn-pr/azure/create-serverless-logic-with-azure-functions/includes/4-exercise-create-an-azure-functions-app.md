I den här övningen skapar vi en Azure-funktionsapp.

1. Logga in på [Azure-portalen](https://portal.azure.com) med ditt Azure-konto.
1. Välj knappen **Skapa en resurs** längst upp till vänster i Azure-portalen och sedan **Beräkning>Funktionsapp**.
  ![Skapa en resurs för funktionsappar](../images/4-create-function-app-blade.png)
1. Välj ett globalt unikt appnamn. Det kommer att fungera som bas-URL för din tjänst. Du kan kalla den **escalator-functions-xxxxxxx**, där kryssen kan ersättas med din initialer och ditt födelseår. Om den ändå inte är globalt unikt kan du testa vilken kombination som helst. Giltiga tecken är a-z, 0-9 och -.
1. Välj den Azure-prenumeration som du vill ska vara värd för funktionsappen.
1. Skapa en ny resursgrupp som kallas **escalator-functions-group**. Det kan hjälpa dig rensa vid ett senare tillfälle.
1. Välj **Windows** som OS.
1. Välj **Förbrukningsplan** som **Värdplan** så att vi kan dra nytta av de serverlösa funktionerna i Azure.
1. Välj den geografiska plats som är närmast dig (eller dina kunder).
1. Skapa ett nytt lagringskonto och namnge det **escalatorfunctions**.
1. Se till att Azure Application Insights är **På** och välj regionen som är närmast dig.
1. Välj **Skapa**. Distributionen kan ta några minuter. Du får ett meddelande när den är klar.
  ![Funktionsappinställningar](../images/4-create-function-app-settings.png)

## <a name="verify-your-azure-function-app"></a>Verifiera din Azure-funktionsapp

1. Välj **Resursgrupper** i den vänstra menyn i Azure-portalen. Du bör se **escalator-functions-group** i listan över tillgängliga grupper.
  ![Resursgrupp](../images/4-resource-group.png)
1. Välj **escalator-functions-group**. Du bör se en lista över resurser som ser ut så här: ![Resurslista](../images/4-resource-list.png)

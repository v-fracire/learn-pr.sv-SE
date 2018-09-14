Vid första upp konsulter och har du beviljats åtkomst till en resursgrupp för marknadsföringsgruppen. Du vill bekanta dig med Azure Portal och se vilka roller som är tilldelade för närvarande.

## <a name="launch-lab-and-sign-in-to-the-azure-portal"></a>Starta labb och logga in på Azure portal

1. Klicka på länken ovan för att starta labbet.

1. Logga in på Azure portal enligt **LabAdmin -_XXXXXXX_@_xxxxxxxxxxxx_. onmicrosoft.com**. Du hittar användarnamn och lösenord på fliken **Resurser** högst upp i det här fönstret.

## <a name="list-role-assignments-for-yourself"></a>Visa en lista med egna rolltilldelningar

Följ stegen nedan om du vill se vilka roller som du har tilldelats.

1. Klicka på ditt användarnamn för att öppna menyn i det övre högra hörnet i Azure Portal.

    ![Mina behörigheter-menyn](../media/4-my-permissions-menu.png)

1. Klicka på **Mina behörigheter** att öppna den min behörighetsfönster.

    ![Min behörighetsfönster](../media/4-my-permissions-pane.png)

    På den min behörighetsfönster kan du se en lista över de roller som du har tilldelats och omfång. Din lista ser annorlunda ut.

## <a name="list-role-assignments-for-a-resource-group"></a>Visa rolltilldelningar för en resursgrupp

Följ stegen nedan om du vill se vilka roller som har tilldelats till omfånget för en resursgrupp.

1. Klicka på **Resursgrupper** i navigeringslistan.

   ![Resursgrupper](../media/4-resource-groups.png)

1. Hitta och klicka på resursgruppen med namnet **FirstUpConsultantsRG1 -_XXXXXXX_**.

1. Klicka på **Åtkomstkontroll (IAM)**.

   ![Åtkomstkontroll för resursgrupp](../media/4-resource-group-access-control.png)

1. Klicka på **rolltilldelning**.

    Du kan se vem som har åtkomst till den här resursgruppen. Observera att vissa roller är begränsade till **Den här resursen** medan andra är **(Ärvda)** från ett överordnat omfång.

   ![Åtkomstkontroll - rolltilldelning för resursgrupp](../media/4-resource-group-role-assignment.png)

## <a name="list-roles"></a>Visa en lista över roller

Som du lärde dig i den föregående kursdelen är en roll en uppsättning behörigheter. Azure har över 70 inbyggda roller som du kan använda i dina rolltilldelningar. Följ det här steget om du vill visa rollerna.

- Under rolltilldelningen, klickar du på **roller** att se en lista över alla inbyggda och anpassade roller.

   Du kan se hur många användare och grupper som är kopplade till varje roll.

   ![Lista över roller](../media/4-roles-list.png)

## <a name="summary"></a>Sammanfattning

I den här kursdelen har du lärt dig hur du visar en lista över de roller som du har tilldelats på Azure Portal. Du har också lärt dig visa en lista över rolltilldelningar för en resursgrupp. I nästa kursdel ska vi gå vidare och använda rollbaserad åtkomstkontroll (RBAC) för att bevilja åtkomst till en användare.

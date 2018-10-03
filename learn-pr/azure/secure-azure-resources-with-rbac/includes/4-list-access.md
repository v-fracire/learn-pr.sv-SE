> [!NOTE]
> När du har startat labbuppgiften finns användarnamnet och lösenordet som du behöver på fliken **Resurser** bredvid instruktionerna.

Du arbetar på First Up Consultants och har beviljats åtkomst till en resursgrupp för marknadsföringsteamet. Du vill bekanta dig med Azure-portalen och se vilka roller som är tilldelade för närvarande.

## <a name="list-role-assignments-for-yourself"></a>Visa en lista med egna rolltilldelningar

Följ stegen nedan om du vill se vilka roller som du har tilldelats.

1. Öppna menyn genom att klicka på ditt användarnamn i det övre högra hörnet i Azure-portalen.

    ![Menyn Mina behörigheter](../media/4-my-permissions-menu.png)

1. Klicka på **Mina behörigheter** för att öppna fönstret Mina behörigheter.

    ![Fönstret Mina behörigheter](../media/4-my-permissions-pane.png)

    I fönstret Mina behörigheter kan du se en lista över de roller som du har tilldelats samt omfattningen. Din lista ser annorlunda ut.

## <a name="list-role-assignments-for-a-resource-group"></a>Visa rolltilldelningar för en resursgrupp

Följ stegen nedan om du vill se vilka roller som har tilldelats till omfånget för en resursgrupp.

1. Klicka på **Resursgrupper** i navigeringslistan.

   ![Resursgrupper](../media/4-resource-groups.png)

1. Leta reda på och klicka på resursgruppen med namnet **FirstUpConsultantsRG1-_XXXXXXX_**.

1. Klicka på **Åtkomstkontroll (IAM)**.

   ![Åtkomstkontroll för resursgrupp](../media/4-resource-group-access-control.png)

    Du kan se vem som har åtkomst till den här resursgruppen. Observera att vissa roller är begränsade till **Den här resursen** medan andra är **(Ärvda)** från ett överordnat omfång.

   ![Åtkomstkontroll – rolltilldelning för resursgrupp](../media/4-resource-group-role-assignment.png)

## <a name="list-roles"></a>Visa en lista över roller

Som du lärde dig i den föregående kursdelen är en roll en uppsättning behörigheter. Azure har över 70 inbyggda roller som du kan använda i dina rolltilldelningar. Följ det här steget om du vill visa rollerna.

- Klicka på **Roller** längst upp i fönstret för att visa en lista över alla inbyggda och anpassade roller.

   Du kan se hur många användare och grupper som är kopplade till varje roll.

   ![Lista över roller](../media/4-roles-list.png)

I den här kursdelen har du lärt dig hur du visar en lista över de roller som du har tilldelats på Azure-portalen. Du har också lärt dig hur du visar en lista över rolltilldelningarna för en resursgrupp.
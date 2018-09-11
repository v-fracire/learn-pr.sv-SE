Du arbetar på First Up Consultants och har beviljats åtkomst till Azure-prenumerationen för marknadsföringsteamet. Du vill bekanta dig med Azure Portal och se vilka roller som är tilldelade för närvarande.

## <a name="list-role-assignments-for-yourself"></a>Visa en lista med egna rolltilldelningar

Följ stegen nedan om du vill se vilka roller som du har tilldelats.

1. Klicka på **Azure Active Directory** i navigeringslistan.

1. Öppna **Alla användare** genom att klicka på **Användare**.

    ![Azure Active Directory-användare](../media-draft/4-aad-all-users.png)

1. Leta upp och klicka på användarnamnet **LabAdmin-_XXXXXXX_**.

    ![Azure Active Directory-labbanvändare](../media-draft/4-aad-all-users-lab.png)

1. Klicka på **Azure-resurser** i avsnittet **Hantera**.

    ![Azure-resurser](../media-draft/4-aad-user-azure-resources.png)

    På bladet Azure-resurser ser du de resurser och roller som du har åtkomst till. Din lista ser annorlunda ut.

## <a name="list-role-assignments-for-a-resource-group"></a>Visa rolltilldelningar för en resursgrupp

Följ stegen nedan om du vill se vilka roller som har tilldelats till omfånget för en resursgrupp.

1. Klicka på **Resursgrupper** i navigeringslistan.

   ![Resursgrupper](../media-draft/4-resource-groups.png)

1. Klicka på resursgruppen med namnet **FirstUpConsultantsRG1-_XXXXXXX_**.

1. Klicka på **Åtkomstkontroll (IAM)**.

   På bladet Åtkomstkontroll (IAM) kan du se vem som har åtkomst till den här resursgruppen. Observera att vissa roller är begränsade till **Den här resursen** medan andra är **(Ärvda)** från ett överordnat omfång.

   ![Åtkomstkontroll (IAM) för resursgrupp](../media-draft/4-resource-group-access-control.png)

## <a name="list-roles"></a>Visa en lista över roller

Som du lärde dig i den föregående kursdelen är en roll en uppsättning behörigheter. Azure har över 70 inbyggda roller som du kan använda i dina rolltilldelningar. Följ stegen nedan för att visa en lista över rollerna.

- Klicka på **Roller** längst upp på bladet Åtkomstkontroll (IAM) för att visa en lista över alla inbyggda och anpassade roller.

   ![Alternativet Roller](../media-draft/4-roles-option.png)

   Du kan se hur många användare och grupper som är kopplade till varje roll.

   ![Lista över roller](../media-draft/4-roles-list.png)

## <a name="summary"></a>Sammanfattning

I den här kursdelen har du lärt dig hur du visar en lista över de roller som du har tilldelats på Azure Portal. Du har också lärt dig hur du visar en lista över rolltilldelningarna i olika omfång. I nästa kursdel ska vi gå vidare och använda rollbaserad åtkomstkontroll (RBAC) för att bevilja åtkomst till en användare.

En medarbetare som heter Alain på First Up Consultants behöver kunna skapa och hantera virtuella datorer till ett projekt han arbetar med. Din chef har bett dig att hantera denna begäran. Utifrån regelverket om att bevilja användare lägsta möjliga behörighet för att utföra sitt arbete väljer du att skapa en ny resursgrupp och tilldela Alain rollen Virtuell datordeltagare.

## <a name="sign-in-to-the-azure-portal"></a>Logga in på Azure Portal

- Kontrollera att du fortfarande är inloggad på Azure Portal som **LabAdmin-_XXXXXXX_@_xxxxxxxxxxxx_.onmicrosoft.com**. Du hittar användarnamn och lösenord på fliken **Resurser** högst upp i det här fönstret.

## <a name="grant-access"></a>Bevilja åtkomst

Följ dessa steg om du vill tilldela rollen Virtuell datordeltagare till en användare i resursgruppomfånget.

1. Klicka på **Resursgrupper** i navigeringslistan.

1. Hitta och klicka på resursgruppen **FirstUpConsultantsRG1-_XXXXXXX_**.

   ![Lista över resursgrupper](../media-draft/5-resource-groups.png)

1. Klicka på **Åtkomstkontroll (IAM)** för att visa den aktuella listan med rolltilldelningar.

   ![Bladet Åtkomstkontroll (IAM) för resursgruppen](../media-draft/5-resource-group-access-control.png)

1. Klicka på **Lägg till** högst upp för att öppna fönstret **Lägg till behörigheter**.

   ![Fönstret Lägg till behörigheter](../media-draft/5-add-permissions.png)

1. I listrutan **Roll** väljer du **Virtuell datordeltagare**.

1. I listan **Välj** väljer du **LabUser-_XXXXXXX_**.

   ![Fönstret Lägg till behörigheter ifyllt](../media-draft/5-add-permissions-save.png)

1. Klicka på **Spara** för att skapa rolltilldelningen.

   Efter en liten stund tilldelas användaren **LabUser-_XXXXXXX_** rollen Virtuell datordeltagare i resursgruppomfånget **FirstUpConsultantsRG1-_XXXXXXX_**. Användaren kan nu skapa och hantera virtuella datorer bara i den här resursgruppen.

   ![Rolltilldelningen Virtuell datordeltagare](../media-draft/5-vm-contributor-assignment.png)

## <a name="remove-access"></a>Ta bort åtkomst

I RBAC tar du bort en rolltilldelning för att ta bort åtkomst.

1. I listan över rolltilldelningar väljer du användaren **LabUser-_XXXXXXX_** med rollen Virtuell datordeltagare.

1. Klicka på **Ta bort**.

   ![Meddelandet Ta bort rolltilldelning](../media-draft/5-remove-role-assignment.png)

1. När du ser meddelandet **Ta bort rolltilldelningar** klickar du på **Ja**.

## <a name="summary"></a>Sammanfattning

I det här avsnittet lärde du dig att bevilja en användare åtkomst för att skapa och hantera virtuella datorer i en resursgrupp med hjälp av Azure Portal. I nästa avsnitt får du se hur du beviljar åtkomst med hjälp av PowerShell.

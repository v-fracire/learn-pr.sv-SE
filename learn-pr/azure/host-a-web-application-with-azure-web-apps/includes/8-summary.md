Du har skapat och distribuerat ett komplett webbprogram med Azure App Services.

Azure App Services gör det enklare att hantera och kontrollera webbappar jämfört med traditionella värdalternativ. Med Azure Web Apps krävs mindre tid och arbete för att köra och hantera en webbapp, samtidigt som du har tillgång till avancerade funktioner som automatisk skalning och git-integrering.

## <a name="clean-up-resources"></a>Rensa resurser

När du är klar med det här programmet kan du följa stegen nedan för att ta bort alla resurser som skapats i självstudien.

Som du vet innehåller en resursgrupp alla andra resurser som skapas och kopplas till webbappen. När du rensar upp i Azure behöver du alltså bara ta bort resursgruppen så tas alla resurser som skapats i den bort automatiskt.

Nu ska vi se hur du kan ta bort en resursgrupp med hjälp av Azure Portal:

1. Logga in på [Azure Portal](https://portal.azure.com?azure-portal=true)

1. Leta upp och klicka på menyalternativet **Resursgrupper** till vänster på instrumentpanelen. Bladet **Resursgrupper** visar en lista över alla resursgrupper som du har skapat på Azure Portal.

1. Leta upp och klicka på namnet på den resursgrupp som du skapade i del 2. Du kommer till bladet Webbapp.

    ![Resursgrupper](../media-draft/8-resource-groups.png)

1. Bladet **Resursgrupp** öppnas. Här kan du se en lista över alla resurser som du har skapat i den här modulen under den här resursgruppen.

    ![Bladet Resursgrupp](../media-draft/8-resource-group-blade.png)

1. Leta upp och klicka på länken **Ta bort resursgrupp** längst upp på bladet.

1. Azure verifierar att du verkligen vill ta bort resursgruppen genom att uppmana dig att ange namnet på den. Om du vill fortsätta skriver du namnet på resursgruppen.

    ![Bekräfta borttagning av resursgrupp](../media-draft/8-resource-group-delete.png)

1. Leta upp och klicka på knappen **Ta bort** längst ned i bekräftelsefönstret.

1. Det tar några sekunder att rensa resursgruppen och alla relaterade resurser.

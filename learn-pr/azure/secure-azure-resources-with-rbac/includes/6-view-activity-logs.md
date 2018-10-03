First Up Consultants granskar ändringar i rollbaserad åtkomstkontroll (RBAC) kvartalsvis för att hitta eventuella fel. Du vet att ändringarna loggas i [Azure-aktivitetsloggen](/azure/monitoring-and-diagnostics/monitoring-overview-activity-logs). Din chef har bett dig att generera en rapport över rolltilldelningen och ändringar i anpassade roller under den senaste månaden.

## <a name="view-activity-logs"></a>Visa aktivitetsloggar

Det enklaste sättet att komma igång på är att visa aktivitetsloggarna i Azure Portal.

1. Klicka på **Alla tjänster** och leta reda på **Aktivitetslogg**.

    ![Aktivitetsloggar i portalen](../media/6-all-services-activity-log.png)

1. Klicka på **Aktivitetslogg** för att öppna aktivitetsloggen.

    ![Aktivitetsloggar i portalen](../media/6-activity-log-portal.png)

1. Ange filtret **Tidsintervall** för att filtrera fram **Förra månaden**.

1. Lägg till ett **åtgärdsfilter** och skriv **roll** för att filtrera listan.

1. Välj följande RBAC-åtgärder:

    - Skapa rolltilldelning (roleAssignments)
    - Ta bort rolltilldelning (roleAssignments)
    - Skapa eller uppdatera en anpassad rolldefinition (roleDefinitions)
    - Ta bort anpassad rolldefinition (roleDefinitions)

    ![Åtgärdsfilter](../media/6-operation-filter.png)

    Efter en liten stund ser du alla rolltilldelnings- och rolldefinitionsåtgärder under den senaste månaden. Där finns också en länk för att hämta aktivitetsloggen som en CSV-fil.

I det här avsnittet har du lärt dig hur du använder Azure-aktivitetsloggen för att visa en lista med RBAC-ändringar på portalen och generera en enkel rapport.
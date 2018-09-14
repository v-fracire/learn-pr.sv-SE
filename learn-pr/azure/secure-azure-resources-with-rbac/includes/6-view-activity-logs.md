First Up Consultants granskar ändringar i rollbaserad åtkomstkontroll (RBAC) kvartalsvis för att hitta eventuella fel. Du vet att ändringarna loggas i [Azure-aktivitetsloggen](/azure/monitoring-and-diagnostics/monitoring-overview-activity-logs). Din chef har bett dig att generera en rapport över rolltilldelningen och ändringar i anpassade roller under den senaste månaden.

## <a name="view-activity-logs"></a>Visa aktivitetsloggar

Det enklaste sättet att komma igång på är att visa aktivitetsloggarna i Azure Portal.

1. Klicka på **Alla tjänster** och leta reda på **Aktivitetslogg**.

    ![Aktivitetsloggar i portalen](../media/6-all-services-activity-log.png)

1. Klicka på **aktivitetsloggen** att öppna aktivitetsloggen.

    ![Aktivitetsloggar i portalen](../media/6-activity-log-portal.png)

1. Ange filtret **Tidsintervall** för att filtrera fram **Förra månaden**.

1. Ange filtret **Händelsekategori** för att filtrera till **Administrativ**.

1. I filtret **Åtgärd** skriver du **roll** för att filtrera listan.

1. Välj följande RBAC-åtgärder:

    - Skapa rolltilldelning (roleAssignments)
    - Ta bort rolltilldelning (roleAssignments)
    - Skapa eller uppdatera en anpassad rolldefinition (roleDefinitions)
    - Ta bort anpassad rolldefinition (roleDefinitions)

    ![Filtret Åtgärd](../media/6-operation-filter.png)

1. Klicka på **Tillämpa** för att tillämpa dina filter.

    Du ser alla rolltilldelnings- och rolldefinitionsåtgärder under den senaste månaden. Där finns också en länk för att hämta aktivitetsloggen som en CSV-fil.

    ![RBAC-aktivitetsloggar](../media/6-activity-log-portal-filter.png)

## <a name="end-lab"></a>Avsluta labbuppgift

1. Om du vill avsluta labbuppgiften klickar du på hamburgermenyn i det övre högra hörnet i fönstret och sedan på **Avsluta**.

1. Klicka på **Ja, avsluta min labbuppgift**.

## <a name="summary"></a>Sammanfattning

I det här avsnittet har du lärt att använda Azure-aktivitetsloggen till att göra en lista med RBAC-ändringar i portalen och generera en enkel rapport.

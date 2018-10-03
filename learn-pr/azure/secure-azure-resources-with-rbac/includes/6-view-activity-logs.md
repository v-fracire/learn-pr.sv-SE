<span data-ttu-id="2302c-101">First Up Consultants granskar ändringar i rollbaserad åtkomstkontroll (RBAC) kvartalsvis för att hitta eventuella fel.</span><span class="sxs-lookup"><span data-stu-id="2302c-101">First Up Consultants reviews role-based access control (RBAC) changes quarterly for auditing and troubleshooting purposes.</span></span> <span data-ttu-id="2302c-102">Du vet att ändringarna loggas i [Azure-aktivitetsloggen](/azure/monitoring-and-diagnostics/monitoring-overview-activity-logs).</span><span class="sxs-lookup"><span data-stu-id="2302c-102">You know that changes get logged in [Azure Activity Log](/azure/monitoring-and-diagnostics/monitoring-overview-activity-logs).</span></span> <span data-ttu-id="2302c-103">Din chef har bett dig att generera en rapport över rolltilldelningen och ändringar i anpassade roller under den senaste månaden.</span><span class="sxs-lookup"><span data-stu-id="2302c-103">Your manager has asked if you can generate a report of the role assignment and custom role changes for the last month.</span></span>

## <a name="view-activity-logs"></a><span data-ttu-id="2302c-104">Visa aktivitetsloggar</span><span class="sxs-lookup"><span data-stu-id="2302c-104">View activity logs</span></span>

<span data-ttu-id="2302c-105">Det enklaste sättet att komma igång på är att visa aktivitetsloggarna i Azure Portal.</span><span class="sxs-lookup"><span data-stu-id="2302c-105">The easiest way to get started is to view the activity logs with the Azure portal.</span></span>

1. <span data-ttu-id="2302c-106">Klicka på **Alla tjänster** och leta reda på **Aktivitetslogg**.</span><span class="sxs-lookup"><span data-stu-id="2302c-106">Click **All services** and then find **Activity log**.</span></span>

    ![Aktivitetsloggar i portalen](../media/6-all-services-activity-log.png)

1. <span data-ttu-id="2302c-108">Klicka på **Aktivitetslogg** för att öppna aktivitetsloggen.</span><span class="sxs-lookup"><span data-stu-id="2302c-108">Click **Activity log** to open the activity log.</span></span>

    ![Aktivitetsloggar i portalen](../media/6-activity-log-portal.png)

1. <span data-ttu-id="2302c-110">Ange filtret **Tidsintervall** för att filtrera fram **Förra månaden**.</span><span class="sxs-lookup"><span data-stu-id="2302c-110">Set the **Timespan** filter to **Last month**.</span></span>

1. <span data-ttu-id="2302c-111">Lägg till ett **åtgärdsfilter** och skriv **roll** för att filtrera listan.</span><span class="sxs-lookup"><span data-stu-id="2302c-111">Add an **Operation** filter and type **role** to filter the list.</span></span>

1. <span data-ttu-id="2302c-112">Välj följande RBAC-åtgärder:</span><span class="sxs-lookup"><span data-stu-id="2302c-112">Select the following RBAC operations:</span></span>

    - <span data-ttu-id="2302c-113">Skapa rolltilldelning (roleAssignments)</span><span class="sxs-lookup"><span data-stu-id="2302c-113">Create role assignment (roleAssignments)</span></span>
    - <span data-ttu-id="2302c-114">Ta bort rolltilldelning (roleAssignments)</span><span class="sxs-lookup"><span data-stu-id="2302c-114">Delete role assignment (roleAssignments)</span></span>
    - <span data-ttu-id="2302c-115">Skapa eller uppdatera en anpassad rolldefinition (roleDefinitions)</span><span class="sxs-lookup"><span data-stu-id="2302c-115">Create or update custom role definition (roleDefinitions)</span></span>
    - <span data-ttu-id="2302c-116">Ta bort anpassad rolldefinition (roleDefinitions)</span><span class="sxs-lookup"><span data-stu-id="2302c-116">Delete custom role definition (roleDefinitions)</span></span>

    ![Åtgärdsfilter](../media/6-operation-filter.png)

    <span data-ttu-id="2302c-118">Efter en liten stund ser du alla rolltilldelnings- och rolldefinitionsåtgärder under den senaste månaden.</span><span class="sxs-lookup"><span data-stu-id="2302c-118">After a few moments, you'll see all the role assignment and role definition operations for the last month.</span></span> <span data-ttu-id="2302c-119">Där finns också en länk för att hämta aktivitetsloggen som en CSV-fil.</span><span class="sxs-lookup"><span data-stu-id="2302c-119">It also includes a link to download the activity log as a CSV file.</span></span>

<span data-ttu-id="2302c-120">I det här avsnittet har du lärt dig hur du använder Azure-aktivitetsloggen för att visa en lista med RBAC-ändringar på portalen och generera en enkel rapport.</span><span class="sxs-lookup"><span data-stu-id="2302c-120">In this unit, you learned how to use Azure Activity Log to list RBAC changes in the portal and generate a simple report.</span></span>
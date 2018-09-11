<span data-ttu-id="9bd56-101">First Up Consultants granskar ändringar i rollbaserad åtkomstkontroll (RBAC) kvartalsvis för att hitta eventuella fel.</span><span class="sxs-lookup"><span data-stu-id="9bd56-101">First Up Consultants reviews role-based access control (RBAC) changes quarterly for auditing and troubleshooting purposes.</span></span> <span data-ttu-id="9bd56-102">Du vet att ändringarna loggas i [Azure-aktivitetsloggen](/azure/monitoring-and-diagnostics/monitoring-overview-activity-logs).</span><span class="sxs-lookup"><span data-stu-id="9bd56-102">You know that changes get logged in [Azure Activity Log](/azure/monitoring-and-diagnostics/monitoring-overview-activity-logs).</span></span> <span data-ttu-id="9bd56-103">Din chef har bett dig att generera en rapport över rolltilldelningen och ändringar i anpassade roller under den senaste månaden.</span><span class="sxs-lookup"><span data-stu-id="9bd56-103">Your manager has asked if you can generate a report of the role assignment and custom role changes for the last month.</span></span>

## <a name="view-activity-logs"></a><span data-ttu-id="9bd56-104">Visa aktivitetsloggar</span><span class="sxs-lookup"><span data-stu-id="9bd56-104">View activity logs</span></span>

<span data-ttu-id="9bd56-105">Det enklaste sättet att komma igång på är att visa aktivitetsloggarna i Azure Portal.</span><span class="sxs-lookup"><span data-stu-id="9bd56-105">The easiest way to get started is to view the activity logs with the Azure portal.</span></span>

1. <span data-ttu-id="9bd56-106">Klicka på **Alla tjänster** och leta reda på **Aktivitetslogg**.</span><span class="sxs-lookup"><span data-stu-id="9bd56-106">Click **All services** and then find **Activity log**.</span></span>

    ![Aktivitetsloggar i portalen](../media-draft/7-all-services-activity-log.png)

1. <span data-ttu-id="9bd56-108">Klicka på **Aktivitetslogg**.</span><span class="sxs-lookup"><span data-stu-id="9bd56-108">Click **Activity log**.</span></span>

    ![Aktivitetsloggar i portalen](../media-draft/7-activity-log-portal.png)

1. <span data-ttu-id="9bd56-110">Ange filtret **Tidsintervall** för att filtrera fram **Förra månaden**.</span><span class="sxs-lookup"><span data-stu-id="9bd56-110">Set the **Timespan** filter to **Last month**.</span></span>

1. <span data-ttu-id="9bd56-111">Ange filtret **Händelsekategori** för att filtrera till **Administrativ**.</span><span class="sxs-lookup"><span data-stu-id="9bd56-111">Set the **Event category** filter to **Administrative**.</span></span>

1. <span data-ttu-id="9bd56-112">I filtret **Åtgärd** skriver du **roll** för att filtrera listan.</span><span class="sxs-lookup"><span data-stu-id="9bd56-112">In the **Operation** filter, type **role** to filter the list.</span></span>

1. <span data-ttu-id="9bd56-113">Välj följande RBAC-åtgärder:</span><span class="sxs-lookup"><span data-stu-id="9bd56-113">Select the following RBAC operations:</span></span>

    - <span data-ttu-id="9bd56-114">Skapa rolltilldelning (roleAssignments)</span><span class="sxs-lookup"><span data-stu-id="9bd56-114">Create role assignment (roleAssignments)</span></span>
    - <span data-ttu-id="9bd56-115">Ta bort rolltilldelning (roleAssignments)</span><span class="sxs-lookup"><span data-stu-id="9bd56-115">Delete role assignment (roleAssignments)</span></span>
    - <span data-ttu-id="9bd56-116">Skapa eller uppdatera en anpassad rolldefinition (roleDefinitions)</span><span class="sxs-lookup"><span data-stu-id="9bd56-116">Create or update custom role definition (roleDefinitions)</span></span>
    - <span data-ttu-id="9bd56-117">Ta bort anpassad rolldefinition (roleDefinitions)</span><span class="sxs-lookup"><span data-stu-id="9bd56-117">Delete custom role definition (roleDefinitions)</span></span>

    ![Filtret Åtgärd](../media-draft/7-operation-filter.png)

1. <span data-ttu-id="9bd56-119">Klicka på **Tillämpa** för att tillämpa dina filter.</span><span class="sxs-lookup"><span data-stu-id="9bd56-119">Click **Apply** to apply your filters.</span></span>

    <span data-ttu-id="9bd56-120">Du ser alla rolltilldelnings- och rolldefinitionsåtgärder under den senaste månaden.</span><span class="sxs-lookup"><span data-stu-id="9bd56-120">You'll see all the role assignment and role definition operations for the last month.</span></span> <span data-ttu-id="9bd56-121">Där finns också en länk för att hämta aktivitetsloggen som en CSV-fil.</span><span class="sxs-lookup"><span data-stu-id="9bd56-121">It also includes a link to download the activity log as a CSV file.</span></span>

    ![RBAC-aktivitetsloggar](../media-draft/7-activity-log-portal-filter.png)

## <a name="end-lab"></a><span data-ttu-id="9bd56-123">Avsluta labbuppgift</span><span class="sxs-lookup"><span data-stu-id="9bd56-123">End lab</span></span>

1. <span data-ttu-id="9bd56-124">Om du vill avsluta labbuppgiften klickar du på hamburgermenyn i det övre högra hörnet i fönstret och sedan på **Avsluta**.</span><span class="sxs-lookup"><span data-stu-id="9bd56-124">To end the lab, click the hamburger menu in the upper-right corner of this window and then click **End**.</span></span>

1. <span data-ttu-id="9bd56-125">Klicka på **Ja, avsluta min labbuppgift**.</span><span class="sxs-lookup"><span data-stu-id="9bd56-125">Click **Yes, end my lab**.</span></span>

## <a name="summary"></a><span data-ttu-id="9bd56-126">Sammanfattning</span><span class="sxs-lookup"><span data-stu-id="9bd56-126">Summary</span></span>

<span data-ttu-id="9bd56-127">I det här avsnittet har du lärt att använda Azure-aktivitetsloggen till att göra en lista med RBAC-ändringar i portalen och generera en enkel rapport.</span><span class="sxs-lookup"><span data-stu-id="9bd56-127">In this unit, you learned how to use Azure Activity Log to list RBAC changes in the portal and generate a simple report.</span></span>

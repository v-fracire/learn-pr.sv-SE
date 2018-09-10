<span data-ttu-id="b7104-101">I den förra övningen gick vi igenom grunderna i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="b7104-101">In the previous exercises, we gathered a basic understanding of Azure AD.</span></span> <span data-ttu-id="b7104-102">Vi skapade en katalog, skapade en användare och en grupp och skapade sedan en regel för villkorlig åtkomst som kräver multifaktorautentisering för åtkomst till Azure Portal.</span><span class="sxs-lookup"><span data-stu-id="b7104-102">We created a directory, created a user and group, and then created a conditional access rule that requires Azure Multi-Factor Authentication when accessing the Azure portal.</span></span>

<span data-ttu-id="b7104-103">Nu ska vi testa om vi kan komma åt våra resurser.</span><span class="sxs-lookup"><span data-stu-id="b7104-103">Now, we'll test if we can access our resources.</span></span>

## <a name="test-access-to-resources"></a><span data-ttu-id="b7104-104">Testa åtkomsten till resurser</span><span class="sxs-lookup"><span data-stu-id="b7104-104">Test access to resources</span></span>

<span data-ttu-id="b7104-105">Eftersom du vet att användarna kommer att logga in och komma åt alla sina SaaS-program via MyApps-portalen så väljer vi att testa just detta.</span><span class="sxs-lookup"><span data-stu-id="b7104-105">You know that your users will sign in and access all their SaaS applications using the MyApps portal, so this is what we'll test.</span></span>

1. <span data-ttu-id="b7104-106">Öppna ett InPrivate-webbläsarfönster.</span><span class="sxs-lookup"><span data-stu-id="b7104-106">Open an InPrivate browser window.</span></span>

1. <span data-ttu-id="b7104-107">Gå till https://myapps.microsoft.com.</span><span class="sxs-lookup"><span data-stu-id="b7104-107">Browse to https://myapps.microsoft.com.</span></span>

1. <span data-ttu-id="b7104-108">Logga in som användaren som vi skapade i del 2.</span><span class="sxs-lookup"><span data-stu-id="b7104-108">Sign in as the user that we created in Unit 2.</span></span>

   * <span data-ttu-id="b7104-109">Observera att du loggas in på portalen utan att utföra multifaktorautentisering.</span><span class="sxs-lookup"><span data-stu-id="b7104-109">Notice that you're signed in to the portal without requiring Multi-Factor Authentication.</span></span>

1. <span data-ttu-id="b7104-110">Gå till https://portal.azure.com i samma webbläsarfönster.</span><span class="sxs-lookup"><span data-stu-id="b7104-110">In the same browser window, browse to https://portal.azure.com.</span></span>

   * <span data-ttu-id="b7104-111">Nu uppmanas du att ange mer information för att skydda ditt konto.</span><span class="sxs-lookup"><span data-stu-id="b7104-111">Notice that you're now required to provide more information to keep your account secure.</span></span> <span data-ttu-id="b7104-112">Det här avbrottet är i själva verket Azure Multi-Factor Authentication som aktiveras via principen för villkorlig åtkomst som vi skapade.</span><span class="sxs-lookup"><span data-stu-id="b7104-112">This interrupt is Azure Multi-Factor Authentication kicking in because of the conditional access policy we created.</span></span> <span data-ttu-id="b7104-113">Du kan avbryta här och stänga webbläsarfönstret.</span><span class="sxs-lookup"><span data-stu-id="b7104-113">You can stop at this point and close the browser window.</span></span>

## <a name="summary"></a><span data-ttu-id="b7104-114">Sammanfattning</span><span class="sxs-lookup"><span data-stu-id="b7104-114">Summary</span></span>

<span data-ttu-id="b7104-115">I det här avsnittet har du lärt dig hur du beviljar behörighet till en användare att skapa och hantera virtuella datorer i en resursgrupp via Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="b7104-115">In this unit, you learned how to grant a user access to create and manage virtual machines in a resource group using Azure PowerShell.</span></span> <span data-ttu-id="b7104-116">I nästa del ska vi se hur du skapar en anpassad roll och definierar dina egna behörigheter.</span><span class="sxs-lookup"><span data-stu-id="b7104-116">In the next unit, you look at how to create a custom role and define your own permissions.</span></span>
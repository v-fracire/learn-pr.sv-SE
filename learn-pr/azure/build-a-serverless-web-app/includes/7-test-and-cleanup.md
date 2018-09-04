<span data-ttu-id="66579-101">Du har skapat ett komplett, serverlöst program med hjälp av Azure-tjänster.</span><span class="sxs-lookup"><span data-stu-id="66579-101">You've successfully created a full-featured, serverless application using Azure services.</span></span>

## <a name="clean-up-resources"></a><span data-ttu-id="66579-102">Ta bort resurserna</span><span class="sxs-lookup"><span data-stu-id="66579-102">Clean up resources</span></span>

<span data-ttu-id="66579-103">När du har arbetat klart med det här programmet kan du använda följande kommando för att ta bort alla resurser som skapades under självstudien:</span><span class="sxs-lookup"><span data-stu-id="66579-103">When you are done working with this application, you can use the following command to delete all resources created during the tutorial:</span></span>

```azurecli
az group delete --name first-serverless-app
```

<span data-ttu-id="66579-104">Skriv `y` när du uppmanas att göra det.</span><span class="sxs-lookup"><span data-stu-id="66579-104">Type `y` when prompted.</span></span>  

## <a name="next-steps"></a><span data-ttu-id="66579-105">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="66579-105">Next steps</span></span>

<span data-ttu-id="66579-106">I den här modulen har du lärt dig att:</span><span class="sxs-lookup"><span data-stu-id="66579-106">In this module, you learned how to:</span></span>
> [!div class="checklist"]
> * <span data-ttu-id="66579-107">Konfigurera Azure Blob Storage att lagra en statisk webbplats och uppladdade bilder.</span><span class="sxs-lookup"><span data-stu-id="66579-107">Configure Azure Blob storage to host a static website and uploaded images.</span></span>
> * <span data-ttu-id="66579-108">Ladda upp bilder till Azure Blob Storage med hjälp av Azure Functions.</span><span class="sxs-lookup"><span data-stu-id="66579-108">Upload images to Azure Blob storage using Azure Functions.</span></span>
> * <span data-ttu-id="66579-109">Ändra storlek på bilder med hjälp av Azure Functions.</span><span class="sxs-lookup"><span data-stu-id="66579-109">Resize images using Azure Functions.</span></span>
> * <span data-ttu-id="66579-110">Lagra bildmetadata i Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="66579-110">Store image metadata in Azure Cosmos DB.</span></span>
> * <span data-ttu-id="66579-111">Använd API:et för visuellt innehåll i Cognitive Services för att skapa bildtexter automatiskt.</span><span class="sxs-lookup"><span data-stu-id="66579-111">Use Cognitive Services Vision API to auto-generate image captions.</span></span>
> * <span data-ttu-id="66579-112">Använd Azure Active Directory för att skydda webbappen genom att autentisera användare.</span><span class="sxs-lookup"><span data-stu-id="66579-112">Use Azure Active Directory to secure the web app by authenticating users.</span></span>

<span data-ttu-id="66579-113">Om du vill lära dig hur du ansluter ännu fler tjänster till Functions fortsätter du med självstudien om Logic Apps.</span><span class="sxs-lookup"><span data-stu-id="66579-113">To learn how to connect even more services to Functions, continue to the Logic Apps tutorial.</span></span> 

> [!div class="nextstepaction"]
> [<span data-ttu-id="66579-114">Functions med Logic Apps</span><span class="sxs-lookup"><span data-stu-id="66579-114">Functions with Logic Apps</span></span>](https://docs.microsoft.com/azure/azure-functions/functions-twitter-email)
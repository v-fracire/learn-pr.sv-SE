Du har skapat ett komplett, serverlöst program med hjälp av Azure-tjänster.

## <a name="clean-up"></a>Rensa
<!---TODO: Update for sandbox--->

När du har arbetat klart med det här programmet kan du använda följande kommando för att ta bort alla resurser som skapades under självstudien:

```azurecli
az group delete --name first-serverless-app
```

Skriv `y` när du uppmanas att göra det.  

## <a name="next-steps"></a>Nästa steg

I den här modulen har du lärt dig att:
  - Konfigurera Azure Blob Storage att lagra en statisk webbplats och uppladdade bilder.
  - Ladda upp bilder till Azure Blob Storage med hjälp av Azure Functions.
  - Ändra storlek på bilder med hjälp av Azure Functions.
  - Lagra bildmetadata i Azure Cosmos DB. 
  - Använd API:et för visuellt innehåll i Cognitive Services för att skapa bildtexter automatiskt.
  - Använd Azure Active Directory för att skydda webbappen med användarautentisering.

Om du vill lära dig hur du ansluter ännu fler tjänster till Functions, fortsätter du med självstudien [Functions med Logic Apps](https://docs.microsoft.com/azure/azure-functions/functions-twitter-email).
Azure CLI är ett bra val för de som inte är så bekanta med kommandoraden och skript i Azure. Den enkla syntaxen och kompatibiliteten med olika plattformar minskar risken för fel när du utför återkommande uppgifter. I den här modulen har du använt Azure CLI-kommandon till att skapa en resursgrupp, och du har använt några få kommandon till att distribuera en webbapp. De här kommandona kan samlas i ett kommandoskript om du vill skapa en automatiserad lösning. 

## <a name="clean-up"></a>Rensa
<!---TODO: Update for sandbox?--->

När du kör webbappar genereras kostnader i din prenumeration. Ta bort de resurser du inte behöver så att du undviker onödiga kostnader. Det enklaste sättet att rensa i din Azure-prenumeration är att ta bort den associerade resursgruppen. Då tas även alla resurser i gruppen bort. När du är färdig med den här modulen kör du följande Azure-kommando:

    ```azurecli
    az group delete --resource-group popupResGroup
    ```

När du uppmanas att bekräfta borttagningen svarar du **Ja**. Det kan ta flera minuter att köra kommandot och ta bort resurserna. 
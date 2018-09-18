I den här modulen har du lärt dig om diskcachelagring i Azure och hur det kan förbättra prestanda. Vi använde Azure-portalen och Azure PowerShell för att hantera diskcachelagring för vår virtuella dator. 

När du har en strategi för diskcachelagring i virtuella Azure-datorer kan du sedan snabbt och enkelt distribuera nya virtuella datorer och diskar med optimala inställningar för diskcachelagring med skript och mallar.

## <a name="further-reading"></a>Ytterligare läsning

- [Azure Premium Storage: Design för höga prestanda](https://docs.microsoft.com/azure/virtual-machines/windows/premium-storage-performance)
- [Kom igång med Azure PowerShell](https://docs.microsoft.com/powershell/azure/get-started-azureps?view=azurermps-6.8.1)
- [Referens för cmdletar för Azure-dator](https://docs.microsoft.com/powershell/module/azurerm.compute/?view=azurermps-6.8.1#vm_disks)


## <a name="cleanup"></a>Rensa
<!---TODO: Update for sandbox?--->

När du kör virtuella Azure-datorer genereras kostnader i din prenumeration. Ta bort de resurser du inte behöver så att du undviker onödiga kostnader. Det enklaste sättet att rensa i din Azure-prenumeration är att ta bort den resursgruppen. När du tar bort en resursgrupp tas alla resurser i den gruppen bort. När du är färdig med den här modulen kör du följande Azure PowerShell-cmdlet:

    ```powershell
    Remove-AzureRmResourceGroup -Name "fotoshare-rg"
    ```

När du uppmanas att bekräfta borttagningen svarar du **Ja**. Det kan ta flera minuter att köra kommandot och ta bort resurserna.

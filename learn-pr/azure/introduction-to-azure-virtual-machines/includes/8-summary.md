I den här modulen gick du igenom de beslut som du behöver fatta innan du skapar en virtuell dator. De här besluten omfattar områden som den virtuella datorns storlek, vilka typer av diskar som används, den operativsystemavbildning som valts samt vilka typer av resurser som skapas.

Du tittade även på alternativen för att skapa och hantera virtuella datorer i Azure. Du upptäckte hur enkelt det är att skapa och hantera virtuella datorer med hjälp av portalen och när du bör använda Resource Manager-mallar, PowerShell, Azure CLI och Azure Client SDK.

Slutligen gick du igenom de tillägg och tjänster som är tillgängliga för att göra administrationen av dina virtuella datorer enklare.

## <a name="cleanup-your-resources"></a>Rensa resurserna

Kom ihåg att virtuella datorer fortsätter att generera en månatlig avgift så länge du har reserverad maskinvara (även om operativsystemet är avstängt!) och utrymme som används för diskar. Du kan stoppa den virtuella datorn i portalen för att stoppa debiteringen för beräkningstjänster, men lagringen fortsätter att generera en faktura. Om du skapar resurser i testsyfte kan du enkelt ta bort allihop genom att ta bort den **resursgrupp** som de ingår i.

> [!TIP]
> Se till att placera alla testresurser i samma resursgrupp för att underlätta hanteringen.

Om du vill ta bort en resursgrupp letar du upp den via panelen **Resursgrupper** (klicka på **Resursgrupper** i sidofältet). Klicka sedan på ellipsmenyn `...` intill resursgruppen och välj **Ta bort resursgrupp** på den snabbmeny som visas där.

![Ta bort en resursgrupp](../media-draft/7-delete-rgs.png)

I den här modulen har du fått lära dig att skapa en virtuell Windows-dator med Azure portalen. Sedan anslöt du till den offentliga IP-adressen för den virtuella datorn och hanterade den via RDP. Du har fått lära dig hur RDP möjliggör en upplevelse i Azure som är jämförbar med att logga in interaktivt till en fysisk dator.

Du fick också lära dig att RDP gör det möjligt att interagera med operativsystemet och programvara i den virtuella datorn, och att portalen gör det möjligt att konfigurera virtuell maskinvara och anslutning. Du skulle även kunna använda PowerShell eller Azure CLI om du föredrar kommandorader eller en skriptbar miljö.

## <a name="clean-up-the-resources"></a>Rensa resurserna

Du debiteras för virtuella datorer när de körs och för lagring baserat på hur mycket du använder. Du bör alltid stoppa och frigöra virtuella datorer när du inte använder dem och när du inte längre behöver resurser är det en bra idé att ta bort dem. Om du vill ta bort alla resurser som du skapade måste du ta bort dem en i taget eller tar du bort resursgruppen.

1. Logga in på Azure Portal.

1. Välj **Alla tjänster** på menyn till vänster.

1. Välj **Resursgrupper**.

1. Hitta resursgruppen som du skapade i den första övningen. Klicka på ellipsen (...) till höger i listvyn.

1. Välj **Ta bort resursgrupp**.

1. På nästa skärm anger du resursgruppens namn för att bekräfta borttagningen.

1. Klicka på **Ta bort**.

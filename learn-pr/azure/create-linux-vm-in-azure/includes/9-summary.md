I den här modulen har du lärt dig hur du skapar en virtuell Linux-dator med hjälp av Azure Portal. Därefter anslöt du till den offentliga IP-adressen för den virtuella datorn och hanterade den med en SSH-anslutning. 

Du lärde dig också att du kan interagera med operativsystemet och programvara på den virtuella datorn med hjälp av SSH, och att du kan konfigurera virtuell maskinvara och anslutningar via portalen. Vi hade även kunnat använda PowerShell eller Azure CLI, om vi hade föredragit att använda en kommandorads- eller skriptmiljö.

## <a name="clean-up-the-resources"></a>Rensa resurserna

Du debiteras för virtuella datorer när de körs och för lagring baserat på hur mycket du använder. Du bör alltid stoppa och frigöra virtuella datorer när du inte använder dem, och när du inte längre behöver resurser är det en bra idé att ta bort dem. Om du vill ta bort alla resurser som du har skapat kan du ta bort dem en i taget eller bara ta bort resursgruppen.

1. Logga in på Azure Portal.

1. Välj **Alla tjänster** på menyn till vänster.

1. Välj **Resursgrupper**.

1. Hitta resursgruppen som du skapade i den första övningen. Klicka på ellipsen (...) till höger i listvyn.

1. Välj **Ta bort resursgrupp**.

1. På nästa skärm anger du resursgruppens namn för att bekräfta borttagningen.

1. Klicka på **Ta bort**.

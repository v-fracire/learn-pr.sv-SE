I den här enheten ska du använda en Azure Resource Manager-mall för att dekryptera den virtuella Windows-dator som vi skapade tidigare. Vi krypterade operativsystemet på den virtuella Windows-datorn. Men OS-enheten skulle inte ha någon konfidentiell information, så vi kunde lämna den okrypterad. Vi ska använda en mall för att dekryptera operativsystemenheten.

## <a name="configure-and-deploy-a-new-vm-using-an-azure-resource-manager-template"></a>Konfigurera och distribuera en virtuell dator med en Azure Resource Manager-mall

Vi ska använda en mall som Microsoft har publicerat på Github för att skapa en ny virtuell dator där kryptering är aktiverad som standard.

1. Logga in på [Azure Portal](https://portal.azure.com/learn.docs.microsoft.com?azure-portal=true) med samma konto som du aktiverade sandbox-miljön.

1. Klick på **Skapa en resurs** i det vänstra sidofältet.

1. Skriv **Mall** i sökrutan.

1. Välj **Malldistribution** i listan som visas och klicka på **Skapa**.

    ![Skärmbild som visar objektet Malldistribution valt med knappen Skapa markerad](../media/6-create-template.png)

1. I sökrutan **Välj en mall** börjar du skriva ”201-decrypt” och välj mallen ”201-decrypt-running-windows-vm-without-aad”.

    ![Skärmvild som visar sökrutan Välj en mall med automatisk komplettering](../media/6-custom-deployment.png)

1. Klicka på **Välj mall** för att starta template runner.

1. Ange följande information i inställningsvyn:
    - Välj _Concierge-prenumeration_ i **Prenumeration**.
    - Välj din resursgrupp som skapats i sandbox-miljön.
    - Välj den plats där du skapade den virtuella datorn.
    - Ange ”fmdata vm01” för **namnet på den virtuell datorn**.
    - Lämna **Volymtypen** som _Alla_.

1. Markera kryssrutan **Jag godkänner villkoren**.
1. Klicka på **Köp** för att köra mallen. Obs! Det här kostar ingenting – det är en standardknapp.

Det kan ta några minuter att slutföra distributionen.

## <a name="verify-the-encryption-status-of-the-vm"></a>Kontrollera krypteringsstatusen för den virtuella datorn

1. Klicka på **Virtuella datorer** på sidopanelen i Azure-portalen och välj din virtuella dator **fmdata-vm01**. Alternativt kan du söka efter den virtuella datorn med namnet från **Alla resurser**.

1. På bladet **Virtuella datorer**, under **INSTÄLLNINGAR**, klickar du på **Diskar**.

1. På bladet **Diskar** ser du att krypteringsstatusen för OS-disken nu är **Aktiverad**.

Din bank hanterar mycket känslig kundinformation och du måste se till att dina diskar alltid är krypterade, inklusive VM-diskdistributioner. Du har fått i uppgift att automatisera en säker VM-distribution för att skydda företagets data.

I den här övningen använder du en Azure Resource Manager-mall för att automatiskt aktivera kryptering för nya virtuella Windows-datorer.

## <a name="configure-and-deploy-a-new-vm-using-an-azure-resource-manager-template"></a>Konfigurera och distribuera en virtuell dator med en Azure Resource Manager-mall

1. Gå till [Resource Manager-mall på GitHub](https://github.com/Azure/azure-quickstart-templates/tree/master/201-encrypt-create-new-vm-gallery-image), och klicka sedan på **Distribuera till Azure**.
1. Gå till bladet **Azure snabbstartsmall** i Azure Portal. Under **Resursgrupp** väljer du **Använd befintlig**. Välj **moneyapprg** i listan.
1. Ange följande information i avsnittet **INSTÄLLNINGAR**:

   - Namn på virtuell dator: **moneyappsvr02**
   - **Administratörens användarnamn**: samma som du använde i föregående övning.
   - **Administratörslösenord**: samma som du använde i föregående övning.
   - **Nytt lagringskontonamn**: ange ett unikt namn.
   - **VM-storlek**: ersätt med samma storlek som du använde i föregående övning, till exempel **Standard_B1s** (eftersom du använder samma Azure-region, se till att storleken är tillgänglig i din aktuella region).
   - **Namn på virtuellt nätverk**: **moneyapprg-vnet**
   - **Undernätsnamn**: **standard**
   - **AAD Client Id** (AAD-klient-ID): kopiera från den information du klistrade in i Anteckningar.
   - **AAD Client Secret** (AAD-klienthemlighet): kopiera från den information du klistrade in i Anteckningar.
   - **Namn på Key Vault**: **moneyappkv**
   - **Key Vault-resursgrupp**: **moneyapprg**
   - **Key Encryption Key URL** (URL för nyckelkrypteringsnyckel): kopiera från den information du klistrade in i Anteckningar.
1. Markera kryssrutan **Jag godkänner villkoren** och klicka sedan på **Köp**.

Det kan ta 5–10 minuter att slutföra distributionen.

## <a name="verify-encryption-status-of-new-vm"></a>Kontrollera krypteringsstatus för den nya virtuella datorn

1. Klicka på **Virtuella datorer** i sidopanelen på Azure-portalen.

1. På bladet **Virtuella datorer** klickar du på **moneyappsvr02**.

1. På bladet **Virtuella datorer**, under **INSTÄLLNINGAR**, klickar du på **Diskar**.

1. På bladet **Diskar** ser du att krypteringsstatus för OS-disken är **Aktiverad**.

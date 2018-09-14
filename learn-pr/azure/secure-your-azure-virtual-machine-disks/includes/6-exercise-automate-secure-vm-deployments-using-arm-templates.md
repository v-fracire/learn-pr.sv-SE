Din bank hanterar mycket känslig kundinformation och du måste se till att dina diskar alltid är krypterade, inklusive VM-diskdistributioner. Du har fått i uppgift att automatisera en säker VM-distribution för att skydda företagets data.

I den här enheten, ska du använda en Azure Resource Manager-mall för att automatiskt aktivera kryptering för nya virtuella Windows-datorer.

## <a name="configure-and-deploy-a-new-vm-using-an-azure-resource-manager-template"></a>Konfigurera och distribuera en ny virtuell dator med en Azure Resource Manager-mall

1. Gå till [Resource Manager-mall på GitHub](https://github.com/Azure/azure-quickstart-templates/tree/master/201-encrypt-create-new-vm-gallery-image), och klicka sedan på **Distribuera till Azure**.
1. I Azure-portalen på den **Azure-snabbstartsmall** bladet under **resursgrupp**väljer **Använd befintlig**. I listan, Välj **moneyapprg**.
1. Ange följande information i avsnittet **INSTÄLLNINGAR**:

   - Namn på virtuell dator: **moneyappsvr02**
   - **Administratören Username**: samma som du använde i den föregående övningen.
   - **Adminlösenord**: samma som du använde i den föregående övningen.
   - **Nytt Lagringskontonamn**: Ange ett unikt namn.
   - **VM-storlek**: Ersätt med samma storlek som du använde i den föregående övningen, till exempel **Standard_B1s** (som du använder samma Azure-region, se till att storleken är tillgängliga i din aktuella region).
   - **Namn på virtuellt nätverk**: **moneyapprg-vnet**
   - **Undernätsnamn**: **standard**
   - **Klient-ID för AAD**: kopiera från den information som du klistrade in till anteckningar.
   - **AAD-Klienthemlighet**: kopiera från den information som du klistrade in till anteckningar.
   - **Namn på Key Vault**: **moneyappkv**
   - **Key Vault-resursgrupp**: **moneyapprg**
   - **Nyckel-URL för nyckeln kryptering**: kopiera från den information som du klistrade in till anteckningar.
1. Markera kryssrutan **Jag godkänner villkoren** och klicka sedan på **Köp**.

Distributionen kan ta 5 – 10 minuter att slutföra.

## <a name="verify-encryption-status-of-new-vm"></a>Kontrollera krypteringsstatus för den nya virtuella datorn

1. Klicka på **Virtuella datorer** i sidopanelen på Azure-portalen.

1. På bladet **Virtuella datorer** klickar du på **moneyappsvr02**.

1. På bladet **Virtuella datorer**, under **INSTÄLLNINGAR**, klickar du på **Diskar**.

1. På bladet **Diskar** ser du att krypteringsstatus för OS-disken är **Aktiverad**.

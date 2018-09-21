Vi börjar med den mest uppenbara uppgiften: skapa en virtuell Azure-dator.

<!-- Activate the sandbox -->
[!include[](../../../includes/azure-sandbox-activate.md)]

## <a name="logins-subscriptions-and-resource-groups"></a>Inloggningar, prenumerationer och resursgrupper

Du får arbeta i Azure Cloud Shell till höger. När du har aktiverat sandbox-miljön loggas du in på Azure med en kostnadsfri prenumeration som hanteras av Microsoft Learn. Du behöver inte logga in på Azure på egen hand eller välja en prenumeration – det görs åt dig. Dessutom skulle du normalt skapa en _resursgrupp_ för nya resurser. I den här modulen skapar Azure-sandbox-miljön en resursgrupp åt dig som används för att köra alla kommandon.

## <a name="create-a-linux-vm-with-the-azure-cli"></a>Skapa en virtuell Linux-dator med hjälp av Azure CLI

Azure CLI innehåller kommandot `vm` som du kan använda för att arbeta med virtuella datorer i Azure. Det finns flera underkommandon för att utföra specifika aktiviteter. De vanligaste är:

| Underkommando | Beskrivning |
|-------------|-------------|
| `create`    | Skapa en ny virtuell dator |
| `deallocate` | Frigöra en virtuell dator |
| `delete` | Ta bort en virtuell dator |
| `list` | Visa en lista över de virtuella datorerna i din prenumeration |
| `open-port` | Öppna en specifik nätverksport för inkommande trafik |
| `restart` | Starta om en virtuell dator |
| `show` | Hämta information för en virtuell dator |
| `start` | Starta en stoppad virtuell dator |
| `stop` | Stoppa en virtuell dator som körs |
| `update` | Uppdatera en egenskap för en virtuell dator |

> [!NOTE]
> En fullständig kommandolista finns i [referensdokumentationen för Azure CLI](https://docs.microsoft.com/cli/azure/reference-index?view=azure-cli-latest).

Låt oss börja med det första: `az vm create`. Det här kommandot används för att skapa en virtuell dator i en resursgrupp. Du kan använda flera parametrar för att konfigurera alla aspekter av den nya virtuella datorn. De tre parametrar som måste anges är:

> [!div class="mx-tableFixed"]
> | Parameter | Beskrivning |
> |-----------|-------------|
> | `resource-group` | Den resursgrupp som ska äga den virtuella datorn använder **<rgn>[sandbox-resursgrupp]</rgn>**. |
> | `name` | Namnet på den virtuella datorn – måste vara unikt inom resursgruppen. |
> | `image` | Avbildningen av operativsystemet som ska användas för att skapa den virtuella datorn. |
> | `location` | Regionen som den virtuella datorn ska placeras i. Vanligtvis är det nära konsumenten av den virtuella datorn. I den här övningen ska du välja en plats i närheten i listan nedan. |

<!-- Resource selection -->
[!include[](../../../includes/azure-sandbox-regions-first-mention-note.md)]

Dessutom är det bra att lägga till flaggan `--verbose` så att du kan följa förloppet när den virtuella datorn skapas. 

## <a name="create-a-linux-virtual-machine"></a>Skapa en virtuell Linux-dator

Nu ska vi skapa en ny virtuell Linux-dator. Utför följande kommando i Azure Cloud Shell för att skapa en Debian Linux-dator på platsen ”USA, västra”. Ändra plats om det inte är i närheten.

```azurecli
az vm create --resource-group <rgn>[Sandbox resource group name]</rgn> --name SampleVM --image Debian --admin-username aldis --generate-ssh-keys --location westus --verbose 
```

[!include[](../../../includes/azure-cloudshell-copy-paste-tip.md)]


Det här kommandot skapar en ny **Debian**-baserad virtuell Linux-dator med namnet `SampleVM`. Observera att verktyget Azure CLI väntar medan den virtuella datorn skapas. Du kan lägga till alternativet `--no-wait` för att instruera Azure CLI-verktyget att återgå omedelbart och låta Azure fortsätta skapa den virtuella datorn i bakgrunden. Det är användbart om du kör kommandot i ett skript. Senare i skriptet använder du kommandot `azure vm wait --name [vm-name]` för att vänta tills den virtuella datorn har skapats.

Om du tittar på de utförliga svaren ser du också att namnet `SampleVM` används för att namnge olika beroenden för den virtuella datorn.

```output
Succeeded: SampleVMNSG (Microsoft.Network/networkSecurityGroups)
Accepted: SampleVMVNET (Microsoft.Network/virtualNetworks)
Succeeded: SampleVMPublicIP (Microsoft.Network/publicIPAddresses)
Accepted: SampleVMVNET (Microsoft.Network/virtualNetworks)
Succeeded: SampleVMVNET (Microsoft.Network/virtualNetworks)
Accepted: vm_deploy_vzKnQDyyq48yPUO4VrSDfFIi81vHKZ9g (Microsoft.Resources/deployments)
```

Du kan åsidosätta dessa automatiskt genererade resursnamn med hjälp av valfria parametrar till `vm create`, till exempel `--vnet-name` och `--public-ip-address-dns-name`.

Vi anger namnet på administratörskontot till **”aldis”** med hjälp av flaggan `admin-username`. Om du utelämnar detta använder kommandot `vm create` _ditt aktuella användarnamn_. Eftersom reglerna för kontonamn är olika för varje operativsystem, är det säkrast att ange ett specifikt namn. 

> [!NOTE]
> Vanliga namn, till exempel ”rot” och ”admin” tillåts inte för de flesta avbildningar.

Vi använder också flaggan `generate-ssh-keys`. Den här parametern används för Linux-distributioner och skapar ett säkerhetsnyckelpar så att vi kan använda verktyget `ssh` för att få fjärråtkomst till den virtuella datorn. De två filerna placeras i mappen `.ssh` på din dator och på den virtuella datorn. Om du redan har en SSH-nyckel med namnet `id_rsa` i målmappen, används den nyckeln i stället för att en ny nyckel genereras.

När den virtuella datorn har skapats får du ett JSON-svar som innehåller den virtuella datorns aktuella tillstånd och dess offentliga och privata IP-adresser som tilldelats av Azure:

```json
{
  "fqdns": "",
  "id": "/subscriptions/20f4b944-fc7a-4d38-b02c-900c8223c3a0/resourceGroups/2568d0d0-efe3-4d04-a08f-df7f009f822a/providers/Microsoft.Compute/virtualMachines/SampleVM",
  "location": "westus",
  "macAddress": "00-0D-3A-58-F8-45",
  "powerState": "VM running",
  "privateIpAddress": "10.0.0.4",
  "publicIpAddress": "40.83.165.85",
  "resourceGroup": "2568d0d0-efe3-4d04-a08f-df7f009f822a",
  "zones": ""
}
```

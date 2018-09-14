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

| Parameter | Beskrivning |
|-----------|-------------|
| `resource-group` | Den resursgrupp som ska äga den virtuella datorn. |
| `name` | Namnet på den virtuella datorn – måste vara unikt inom resursgruppen. |
| `image` | Avbildningen av operativsystemet för att skapa den virtuella datorn. |

Dessutom är det bra att lägga till flaggan `--verbose` så att du kan följa förloppet när den virtuella datorn skapas. 

## <a name="create-a-linux-virtual-machine"></a>Skapa en virtuell Linux-dator

Nu ska vi skapa en ny virtuell Linux-dator. Kör följande kommando i Azure Cloud Shell:

```azurecli
az vm create --resource-group <rgn>[Sandbox resource group name]</rgn> --name SampleVM --image Debian --admin-username aldis --generate-ssh-keys --verbose 
```

Det här kommandot skapar en ny **Debian**-baserad virtuell Linux-dator med namnet `SampleVM`. Observera att verktyget Azure CLI blockeras medan den virtuella datorn skapas. Om du inte vill vänta kan du använda alternativet `--no-wait` för att instruera Azure CLI att returnera utdata direkt, till exempel om du kör kommandot i ett skript. Senare i skriptet använder du kommandot `azure vm wait --name [vm-name]` för att vänta tills den virtuella datorn har skapats.

Om du tittar på de utförliga svaren ser du också att namnet `SampleVM` används för att namnge olika beroenden för den virtuella datorn.

```
Succeeded: SampleVMNSG (Microsoft.Network/networkSecurityGroups)
Accepted: SampleVMVNET (Microsoft.Network/virtualNetworks)
Succeeded: SampleVMPublicIP (Microsoft.Network/publicIPAddresses)
Accepted: SampleVMVNET (Microsoft.Network/virtualNetworks)
Succeeded: SampleVMVNET (Microsoft.Network/virtualNetworks)
Accepted: vm_deploy_vzKnQDyyq48yPUO4VrSDfFIi81vHKZ9g (Microsoft.Resources/deployments)
```

Du kan åsidosätta dessa automatiskt genererade resursnamn med hjälp av valfria parametrar till `vm create`, till exempel `--vnet-name` och `--public-ip-address-dns-name`.

Observera att vi anger namnet på administratörskontot till ”aldis” med hjälp av flaggan `admin-username`. Om du utelämnar detta använder kommandot `vm create` _ditt aktuella användarnamn_. Eftersom reglerna för kontonamn är olika för varje operativsystem, är det säkrast att ange ett specifikt namn. Vanliga namn, till exempel ”rot” och ”admin” tillåts inte för de flesta avbildningar.

Vi använder också flaggan `generate-ssh-keys`. Den här parametern används för Linux-distributioner och skapar ett säkerhetsnyckelpar så att vi kan använda verktyget `ssh` för att få fjärråtkomst till den virtuella datorn. De två filerna placeras i mappen `.ssh` på din dator och på den virtuella datorn. Om du redan har en SSH-nyckel med namnet `id_rsa` i målmappen, används den nyckeln i stället för att en ny nyckel genereras.

När den virtuella datorn har skapats får du ett JSON-svar som innehåller den virtuella datorns aktuella tillstånd och dess offentliga och privata IP-adresser som tilldelats av Azure:

<!-- TODO: find out the default location! -->

```json
{
  "fqdns": "",
  "id": "/subscriptions/abc13b0c-d2c4-64b2-9ac5-2f4cb819b752/resourceGroups/ExerciseResources/providers/Microsoft.Compute/virtualMachines/SampleVM",
  "location": "eastus",
  "macAddress": "00-0D-3A-1A-D9-74",
  "powerState": "VM running",
  "privateIpAddress": "10.0.0.4",
  "publicIpAddress": "168.61.54.62",
  "resourceGroup": "ExerciseResources",
  "zones": ""
}
```

<!-- TODO: find out the default location! -->

> [!NOTE]
> Observera att den virtuella datorn har skapats på platsen **usaöstra**. Som standard skapas den virtuella datorn på den plats som identifieras av den ägande regionen. Ibland kan det emellertid vara bra att associera den virtuella datorn med en befintlig region, men låta den starta någon annanstans i världen. Det kan du göra genom att ange parametern `--location` som en del av kommandot `az vm create`.
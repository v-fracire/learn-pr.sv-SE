Virtuella datorer måste ha lämplig storlek för förväntat arbete. En virtuell dator utan rätt mängd minne eller CPU misslyckas under belastning eller körs för långsamt för att vara effektiv. 

## <a name="pre-defined-vm-sizes"></a>Fördefinierade storlekar på virtuella datorer

När du skapar en virtuell dator kan du ange ett _VM-storleksvärde_ som bestämmer den mängd beräkningsresurser som kommer att ägnas åt den virtuella datorn. Det inbegriper processor, GPU och minne som är gjorda tillgängliga för den virtuella datorn från Azure.

Azure definierar en uppsättning fördefinierade VM-storlekar för Linux och Windows du kan välja bland, baserat på förväntad användning. 

| Typ | Storlekar | Beskrivning |
|------|-------|-------------|
| Generellt syfte   | Dsv3, Dv3, DSv2, Dv2, DS, D, Av2, A0-7 | Balanserat förhållande mellan processor och minne. Perfekt för utveckling/test samt små till medelstora lösningar för program och data. |
| Beräkningsoptimerad | Fs, F | Högt förhållande mellan processor och minne. Bra för program med medelhög trafik, nätverkstillämpningar och batchprocesser. |
| Minnesoptimerad  | Esv3, Ev3, M, GS, G, DSv2, DS, Dv2, D   | Högt förhållande mellan minne och kärna. Utmärkt för relationsdatabaser, mellanstora till stora cacheminnen och minnesinterna analyser. |
| Lagringsoptimerad | Ls | Högt diskgenomflöde och I/O. Perfekt för stordata, SQL- och NoSQL-databaser. |
| GPU-optimerad | NV, NC | Virtuella specialdatorer som är avsedda för tung grafisk rendering och videoredigering. |
| Höga prestanda | H, A8-11 | Virtuella datorer med de kraftfullaste processorerna och nätverksgränssnitt för stora dataflöden (RDMA). | 

Tillgängliga storlekar varierar beroende på vilken region du skapar den virtuella datorn i. Du kan hämta en lista över tillgängliga storlekar med kommandot `vm list-sizes`. Försök att skriva det här i Azure Cloud Shell:

```azurecli
az vm list-sizes --location eastus --output table
```

Här är ett förkortat svar för `eastus`:

```
  MaxDataDiskCount    MemoryInMb  Name                      NumberOfCores    OsDiskSizeInMb    ResourceDiskSizeInMb
------------------  ------------  ----------------------  ---------------  ----------------  ----------------------
                 2          2048  Standard_B1ms                         1           1047552                    4096
                 2          1024  Standard_B1s                          1           1047552                    2048
                 4          8192  Standard_B2ms                         2           1047552                   16384
                 4          4096  Standard_B2s                          2           1047552                    8192
                 8         16384  Standard_B4ms                         4           1047552                   32768
                16         32768  Standard_B8ms                         8           1047552                   65536
                 4          3584  Standard_DS1_v2                       1           1047552                    7168
                 8          7168  Standard_DS2_v2                       2           1047552                   14336
                16         14336  Standard_DS3_v2                       4           1047552                   28672
                32         28672  Standard_DS4_v2                       8           1047552                   57344
                64         57344  Standard_DS5_v2                      16           1047552                  114688
        ....
                64       3891200  Standard_M128-32ms                  128           1047552                 4096000
                64       3891200  Standard_M128-64ms                  128           1047552                 4096000
                64       3891200  Standard_M128ms                     128           1047552                 4096000
                64       2048000  Standard_M128s                      128           1047552                 4096000
                64       1024000  Standard_M64                         64           1047552                 8192000
                64       1792000  Standard_M64m                        64           1047552                 8192000
                64       2048000  Standard_M128                       128           1047552                16384000
                64       3891200  Standard_M128m                      128           1047552                16384000
```

## <a name="specifying-a-size-during-vm-creation"></a>Ange en storlek under skapandet av virtuell dator

Vi angav ingen storlek när vi skapade vår virtuella dator – så Azure valde en allmän standardstorlek på `Standard_DS1_v2`. Men vi kan ange storleken som en del av kommandot `vm create` med parametern `--size`. Du kan exempelvis använda följande kommando för att skapa en 16-kärnig virtuell dator:

```azurecli
az vm create --resource-group <rgn>[sandbox resource group name]</rgn> --name SampleVM2 \
  --image Debian --admin-username aldis --generate-ssh-keys --verbose \
  --size "Standard_DS5_v2"
```

> [!WARNING]
> Din prenumerationsnivå [tillämpar begränsningar](https://docs.microsoft.com/azure/azure-subscription-service-limits) på hur många resurser du kan skapa, samt den totala storleken på de resurserna. Du begränsas exempelvis till **20 virtuella processorer** med betala per användning-prenumerationen och bara **4 virtuella processorer** för en kostnadsfri nivå. Azure CLI meddelar dig när du överskrider detta med felmeddelandet **Kvoten har överskridits**. Om du stöter på det i dina egen betalda prenumeration kan du begära att höja gränserna som är associerade med din betalda prenumeration (upp till 10 000 virtuella processorer!) via en [kostnadsfri onlinebegäran](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-quota-errors).

## <a name="resizing-an-existing-vm"></a>Ändra storlek på en befintlig virtuell dator
Vi kan även ändra storlek för en befintlig virtuell dator om arbetsbelastningen ändras, eller om det blev fel storlek när den skapades. Innan en storleksändring begärs måste vi kontrollera om den önskade storleken är tillgänglig i klustret som vår virtuella dator ingår i. Det kan vi göra med kommandot `vm list-vm-resize-options`:

```azurecli
az vm list-vm-resize-options --resource-group <rgn>[sandbox resource group name]</rgn> --name SampleVM --output table
```

Det returnerar en lista över alla möjliga storlekskonfigurationer i resursgruppen. Om storleken vi vill ha inte är tillgängliga i vårt kluster, men _är_ tillgängliga i regionen, kan vi [frigöra den virtuella datorn](https://docs.microsoft.com/cli/azure/vm?view=azure-cli-latest#az-vm-deallocate). Det här kommandot stoppar den virtuella datorn som körs och tar bort den från det aktuella klustret utan att förlora några resurser. Därefter kan vi ändra storlek på den, vilket återskapar den virtuella datorn i ett nytt kluster där storlekskonfigurationen är tillgänglig.

> [!NOTE]
> Azure-sandbox-miljön är begränsad till ett mindre antal storlekar på virtuella datorer. De flesta av möjligheterna är inte tillåtna i den kostnadsfria Microsoft Learn-prenumerationen.

När vi ska ändra storleken på en virtuella dator använder vi kommandot `vm resize`. Till exempel upptäcker vi kanske att den virtuella datorn inte har tillräckligt med prestanda för den uppgift som vi vill utföra. Vi skulle kunna öka den med några nivåer till en DS3_v2-nivå med 4 virtuella kärnor och 14 G minne. Ange följande kommando i Cloud Shell:

```azurecli
az vm resize --resource-group <rgn>[sandbox resource group name]</rgn> --name SampleVM --size Standard_DS3_v2
```

Det tar några minuter för det här kommandot att minska den virtuella datorns resurser, och när det är gjort returneras en ny JSON-konfiguration.

I den föregående övningen utförde vi följande uppgifter med hjälp av Azure-portalen.

- Visa status för OS-diskcache
- Ändra inställningarna för cachelagring för OS-disken
- Lägga till en datadisk i en virtuell dator
- Ändra typ av cachelagring på en ny datadisk

Nu övar vi på dessa åtgärder med hjälp av Azure PowerShell. Vi använder den virtuella dator som vi skapade i den föregående övningen. Åtgärderna i den här labben förutsätter att:

- Vår virtuella dator finns och heter **fotoshareVM**
- Vår virtuella dator finns i en resursgrupp som heter **fotoshare-rg**

Om du har använt andra namn ersätter du bara de här värdena med dina. 

Här är det aktuella tillståndet för våra VM-diskar från föregående övning. 

![Skärmbild på våra OS- och datadiskar som båda är inställda på skrivskyddad cachelagring.](../media-draft/disks-final-config-portal.PNG)

Vi har använt portalen för att ange fältet ***HOST CACHING** (Cachelagring för värd) för både för OS- och datadiskar. Tänk på detta inledande tillstånd när vi går igenom följande steg. 

### <a name="set-up-some-variables"></a>Konfigurera några variabler
Först ska vi lagra vissa resursnamn så att vi kan använda dem senare.

Använd Azure Cloud Shell-terminalen till höger för att köra följande Powershell-kommandon. 

```powershell
$myRgName = "fotoshare-rg"
$myVMName = "fotoshareVM"
```

> [!TIP]
> Du måste ange dessa variabler igen om tidsgränsen uppnås för Cloud Shell-sessionen. Om möjligt bör du därför gå igenom hela den labben i en enda session. 

### <a name="get-info-about-our-vm"></a>Få information om vår virtuella dator

Kör följande kommando för att hämta egenskaperna för den virtuella datorn.
 
```powershell
$myVM = Get-AzureRmVM -ResourceGroupName $myRgName -VMName $myVMName
```
Vi lagrar svaret i variabeln `$myVM`. Vi kan köra följande kommando för att bara visa de egenskaper som vi anger här.

```powershell
$myVM | select-object -property ResourceGroupName, Name, Type, Location
```

Som följande skärmbild visar är den här virtuella datorn verkligen den virtuella dator som vi letar efter. Därför går vi vidare. 

![PowerShell-konsolen som visar resultatet av senaste 4 kommandona som vi körde.](../media-draft/ps-commands-1.PNG)

### <a name="view-os-disk-cache-status"></a>Visa status för OS-diskcache

Vi kan kontrollera inställningen cachelagring via `StorageProfile`-objektet på följande sätt.

```powershell
$myVM.StorageProfile.OsDisk.Caching
```
I det här exemplet är det aktuella värdet **Inget**. Vi ändrar tillbaka det till standardinställningen för en OS-disk.

![PowerShell-konsolen som visar vår OS-disk med cachelagringsvärdet ”Inget”.](../media-draft/ps-oscaching-none.PNG)

### <a name="change-the-cache-settings-of-the-os-disk"></a>Ändra inställningarna för cachelagring för OS-disken

Vi kan ange värdet för cachelagringstyp med hjälp av samma StorageProfile-objekt på följande sätt.
 
```powershell
$myVM.StorageProfile.OsDisk.Caching = "ReadWrite"
```

Det här kommandot körs snabbt, vilket bör innebär att det utför arbete lokalt. Kommandot ändrar bara egenskapen för myVM-objektet. Som följande skärmbild visar kommer cachelagringsvärdet inte att ha ändrats på den virtuella datorn om du uppdaterar variabeln `$myVM`.

![PowerShell-konsolen som visar att när ”myVM”-objektet uppdateras så återställs cachelagringsvärdet till ”Inget” eftersom vi i själva verket inte uppdaterade den virtuella datorn.](../media-draft/ps-commands-2.PNG)

För att göra ändringen i på själva den virtuella datorn anropar du `Update-AzureRmVM` på följande sätt.

```powershell
Update-AzureRmVM -ResourceGroupName $myRGName -VM $myVM
```

Observera att det här anropet tar en stund att slutföra. Det beror på att vi uppdaterar den faktiska virtuella datorn, och Azure startar om den virtuella datorn för att göra ändringen.

![PowerShell-konsolen som visar vår OS-disk med cachelagringsvärdet ”Inget”.](../media-draft/ps-oscaching-rw.PNG)

Om du uppdaterar variabeln `$myVM` igen visas ändringen för objektet. Om du tittar på disken i portalen ser du ändringen även där. Nu går vi vidare till att skapa en ny datadisk.  

### <a name="list-data-disk-info"></a>Lista information om datadisk

För att se vilka datadiskar vi har på den virtuella datorn kör du följande kommando. 

```powershell
$myVM.StorageProfile.DataDisks
```

Vi har endast en datadisk för tillfället. Fältet `Lun` är viktigt. Det är det unika **L**ogical **U**nit **N**umber (LUN). När vi lägger till en till datadisk ger vi den ett unikt `Lun`-värde. 

### <a name="add-a-new-data-disk-to-our-vm"></a>Lägga till en ny datadisk till den virtuella datorn 

För enkelhetens skull sparar vi vårt nya disknamn.

```powershell
$newDiskName = "fotoshareVM-data2"
```

Kör följande `Add-AzureRmVMDataDisk`-kommando för att definiera en ny disk. 

```powershell
Add-AzureRmVMDataDisk -VM $myVM -Name $newDiskName  -LUN 1  -DiskSizeinGB 1 -CreateOption Empty
```

Vi har gett den här disken LUN-värdet 1 eftersom det inte är upptaget. Vi har definierat den disk som vi vill skapa, så det är dags att köra `Update-AzureRmVM` för att göra den faktiska ändringen. 

```powershell
Update-AzureRmVM -ResourceGroupName $myRGName -VM $myVM
```

Vi tar en titt på vår datadiskinfo igen.

```powershell
$myVM.StorageProfile.DataDisks
```

![PowerShell-konsolen som visar våra två datadiskar.](../media-draft/2-data-disks-part1.png)

Nu har vi två diskar. Vår nya disk har **LUN**-värdet 1 och standardvärdet för **Cachelagring**, som är **Inget**. Låt oss ändra det värdet.

### <a name="change-cache-settings-of-new-data-disk"></a>Ändra inställningar för cachelagring för en ny datadisk

Vi ändrar egenskaperna för en ny virtuell datordatadisk med cmdleten `Set-AzureRmVMDataDisk` enligt följande.

```powershell
Set-AzureRmVMDataDisk -Lun "1" -Caching ReadWrite
```

Som alltid checkar vi in ändringarna med `Update-AzureRmVM`.

```powershell
Update-AzureRmVM -ResourceGroupName $myRGName -VM $myVM
```

Här är en vy från portalen för det vi har gjort i den här övningen. Vår virtuella dator har nu två datadiskar, och vi har justerat alla inställningar för **HOST Caching** (Cachelagring för värd). Vi gjorde allt detta med bara några få kommandon. Det är kraften hos Azure PowerShell.

![PowerShell-konsolen som visar våra två datadiskar.](../media-draft/disks-final-config-portal2.png)

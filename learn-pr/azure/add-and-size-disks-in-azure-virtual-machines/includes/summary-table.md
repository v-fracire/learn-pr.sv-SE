
I följande tabell visas vilka diskar som är tillgängliga som hanterade och ohanterade diskar.

|Disktyp  |Ohanterade  | Hanterad  | Storage-konto för ohanterade|
|---------|---------|---------|---------|
|[Standard HDD](https://docs.microsoft.com/azure/virtual-machines/windows/standard-storage)      |   ja      |  ja       |  Allmänt lagringskonto       |
|[Standard SSD](https://docs.microsoft.com/azure/virtual-machines/windows/disks-standard-ssd)    |   nej      |   ja      |  Inte tillämpligt       |
|[Premium SSD](https://docs.microsoft.com/azure/virtual-machines/windows/premium-storage)    |    ja     |   ja      |     Premium storage-konto    |

Här är en sammanfattning av de nackdelar som du gör när du väljer en disktyp för dina virtuella datorer.

|Disktyp  |Relativa IOPS *  | Relativa kostnaden  | Scenarier|
|---------|---------|---------|---------|
|[Standard HDD](https://docs.microsoft.com/azure/virtual-machines/windows/standard-storage)      |   lägsta      |  lägsta       |  Använd för arbetsbelastningar med låg IOPs som kräver en konsekvent prestanda och kostnadseffektiv utveckling och testning       |
|[Standard SSD](https://docs.microsoft.com/azure/virtual-machines/windows/disks-standard-ssd)    |   medium|   medium      |  Använd för arbetsbelastningar med låg IOPs som kräver en konsekvent prestanda och kostnadseffektiv utveckling och testning |
|[Premium SSD](https://docs.microsoft.com/azure/virtual-machines/windows/premium-storage)    |    högsta     |   högsta      |     Använd för produktion och resultatdrivna arbetsbelastningar där låg fördröjning och högt dataflöde är kritiska    |

* IOPS - i/o-åtgärder per sekund.
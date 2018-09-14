En Redis-cache kan förbättra prestanda och skalbarhet i system som är starkt beroende av databaser. Prestanda förbättras genom att data som används ofta tillfälligt kopieras till snabb lagring som finns nära programmet. Med Redis Cache finns den här snabba lagringen i minnet i Redis Cache, i stället för att läsas in från en disk av en databas.

# <a name="cleanup"></a>Rensa

Redis Cache-körningar medför kostnader i din prenumeration. Du bör ta bort den för att undvika dessa kostnader. Det enklaste sättet att ta bort den är att ta bort resursgruppen som den skapades i. Kom dock ihåg att när du tar bort en resursgrupp så tas allt innehåll i den bort. Du kan också ta bort själva cachen.
## <a name="docker"></a>Docker

![Docker-logotypen](../media/3-image1.PNG)

Med Docker kan utvecklare enkelt paketera, leverera och köra alla program som en enkel, portabel och oberoende container, som kan köras praktiskt taget var som helst. Varför behövs containerskapande klienter som Docker om DSVM-basavbildningen levereras med de populäraste djupinlärningsramverken förinstallerade?

Ofta när utvecklare försöker köra djupinlärningsuppgifter drabbas de av mardrömmar om beroende, som: 

- Tvingas skapa anpassade paket – djupinlärningsforskare tenderar att tänka mindre på produktion när de publicerar kod i GitHub. Om de kan få ett paket att fungera i den egna utvecklingsmiljön antar de ofta att också andra kan göra det.
- Versionshantering av GPU-drivrutin – CUDA är en parallell databehandlingsplattform och ett API (Application Programming Interface) som utvecklats av Nvidia. Med det kan programutvecklare och programvarutekniker använda en CUDA-aktiverad grafikprocessor (GPU) för allmän bearbetning. Vissa versioner av Tensorflow fungerar inte med senare versioner av CUDA än 9.1 men andra ramverk, till exempel PyTorch, verkar fungera bättre med senare versioner av CUDA.

För att undvika dessa problem och för att öka kodens användbarhet kan du använda Docker eller dess GPU-variant Nvidia-Docker för att hantera och köra djupinlärningsprojekt. 

<!--Quiz 
What is CUDA? 
What versioning issues do deep learning engineers deal with? -->
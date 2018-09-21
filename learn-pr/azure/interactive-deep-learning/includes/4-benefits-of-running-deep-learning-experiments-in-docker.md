![Docker-logotyp](../media/3-image1.PNG)

Docker är ett verktyg som gör att du kan distribuera dina program i en sandbox-miljö för att köra på valfritt värdoperativsystem. Det gör att du kan paketera din app med alla dess beroenden i en standardiserad enhet. Men om DSVM-basavbildningen levereras med de mest populära ramverken för djupinlärning förinstallerade, varför ska du då använda Docker?

När utvecklare försöker köra djupinlärningsuppgifter står de inför utmaningar om beroenden. Exempel: 

- Tvingas skapa anpassade paket – djupinlärningsforskare tenderar att tänka mindre på produktion när de publicerar kod i GitHub. Om de kan få ett paket att fungera i den egna utvecklingsmiljön antar de ofta att andra också kan göra det.
- Versionshantering av GPU-drivrutin – CUDA är en parallell databehandlingsplattform och ett API (Application Programming Interface) som utvecklats av NVIDIA. Med det kan utvecklare använda en CUDA-aktiverad grafikprocessor (GPU) för allmän bearbetning. Vissa versioner av Tensorflow fungerar inte med CUDA-versioner över 9.1. Andra ramverk, till exempel PyTorch, verkar fungera bättre med senare versioner av CUDA.

För att undvika dessa problem och för att öka kodens användbarhet kan du använda Docker eller dess GPU-variant NVIDIA Docker för att hantera och köra djupinlärningsprojekt. 

<!--Quiz 
What is CUDA? 
What versioning issues do deep learning engineers deal with? -->
## <a name="intro-to-deep-learning"></a>Introduktion till djupinlärning

Målet med Machine Learning (ML) är att hitta funktioner för att ta fram en modell som omvandlar indata (t.ex. bilder, tidsserier eller ljud) till vissa utdata (t.ex. bildtexter, prisvärden, transkriptioner). I traditionella datavetenskap handplockas funktionerna ofta.

![Ett kanoniskt exempel på ett framåtmatande djupt neuralt nätverk.](../media/2-image1.PNG)

I Deep Learning (DL) är funktionsextraheringen en process som lärs in genom att indata representeras som vektorer och transformeras med ett antal smarta linjära algebraåtgärder till givna utdata.  

Modellutdata jämförs sedan med förväntade utdata med hjälp av en förlustfunktionsekvation. Värdet som returneras av förlustfunktionen för respektive utbildnings indata används för att hjälpa modellen att extrahera funktioner som resulterar i ett lägre förlustvärde nästa pass.  
 
Den serie av matrisåtgärder som vi beräknar som en del av den linjära algebrakomponenten tenderar att vara beräkningsmässigt expansiv och är ofta tungt parallelliserbar, vilket kräver specialiserad databearbetning, t.ex. grafikprocessorer, för att kunna beräkna effektivt.

## <a name="data-science-virtual-machine"></a>Data Science Virtual Machine

![DSVM-alternativ](../media/2-image2.PNG)

DSVM:er är Azure Virtual Machine-avbilder som har förinstallerats, konfigurerats och testats med flera populära verktyg som vanligen används för dataanalys, maskininlärning och utbildning inom djupinlärning.

De tillhandahåller:

- Enhetlig installation mellan team, vilket underlättar delning och samarbete, skalning och hantering i Azure, minimal konfigurering, helt molnbaserat skrivbord för datavetenskap.
- Elastisk kapacitet på begäran med möjlighet att köra analyser på alla Azure-maskinvarukonfigurationer med vertikal och horisontell skalning. Betala endast för det du använder, och när du använder det.
- Djupinlärning med GPU:er, lättillgängliga GPU-kluster med förkonfigurerade djupinlärningsverktyg. 

DSVM innehåller flera AI-verktyg, t.ex. populära GPU-versioner av djupinlärningsramverk och verktyg som Microsoft R Server Developer Edition, Anaconda Python, Jupyter-anteckningsböcker för Python och R, IDE:er för Python och R, SQL-databaser och många andra verktyg för datavetenskap och ML.

DSVM kan köras på VM-instanser i Azure GPU NC-serien. Dessa GPU:er använder diskret enhetstilldelning, vilket resulterar i Bare Metal-liknande prestanda och som väl lämpar sig för djupinlärningsproblem.

<!--### Quiz? 

What is the goal of machine learning? 
How is traditional machine learning different from deep learning? 
Why are GPU's often used for deep learning? 
What does the DSVM provide? -->

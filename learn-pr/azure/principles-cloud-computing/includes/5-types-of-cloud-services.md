När man talar om molnbaserad databehandling finns det tre huvudkategorier. Det är viktigt att förstå dem, eftersom de används i konversationer, dokumentation och utbildning.

## <a name="explore-the-three-categories-of-cloud-computing"></a>Utforska de tre kategorierna av molnbaserad databehandling

> [!VIDEO https://www.microsoft.com/videoplayer/embed/RE2yEbs]

:::row:::
  :::column:::
    ![IaaS-ikon](../media/5-iaas.png)
  :::column-end:::
  :::column span="3"::: **Infrastructure as a service (IaaS)**

Infrastruktur som en tjänst är den mest flexibla kategorin av molntjänster. Syftet är att ge dig fullständig kontroll över den maskinvara som kör ditt program. Istället för att köpa maskinvara hyr du den med laaS.
  :::column-end:::
:::row-end:::

:::row:::
  :::column:::
    ![Ikon för PaaS](../media/5-paas.png)
  :::column-end:::
  :::column span="3"::: **Platform as a service (PaaS)**

Plattform som en tjänst är en miljö för att skapa, testa och distribuera program. Målet med PaaS är att hjälpa dig att skapa ett program så snabbt som möjligt utan att du ska behöva bekymra dig om att hantera den underliggande infrastrukturen. När du till exempel distribuerar ett webbprogram med PaaS behöver du inte installera något operativsystem, någon webbserver eller ens systemuppdateringar.
  :::column-end:::
:::row-end:::

:::row:::
  :::column:::
    ![SaaS-ikon](../media/5-saas.png)
  :::column-end:::
  :::column span="3"::: **Software as a service (SaaS)**

Programvara som en tjänst är centralt värdbaserad programvara som är hanterad för slutkunden. Det är vanligtvis baserad på en arkitektur där en version av programmet används för alla kunder och licensieras via en månads- eller årsprenumeration. Office 365 är ett perfekt exempel på ett SaaS-program.
  :::column-end:::
:::row-end:::

## <a name="think-about-service-categories-as-layers"></a>Föreställ dig tjänstekategorierna som lager

En sak att tänka på är att de här kategorierna är lager som ligger ovanpå varandra. Till exempel lägger PaaS till ett lager över IaaS genom att tillhandahålla en abstraktionsnivå. Abstraktionerna har fördelen att dölja informationen du inte bryr dig om, så att du snabbare kommer till kodningen. Men priset du får betala är att du får mindre kontroll över den underliggande maskinvaran. På bilden nedan finns en lista över alla resurser som du hanterar och som din tjänsteleverantör hanterar i varje molntjänstkategori.

![En bild som visar abstraktionsnivån för varje molntjänstkategori.](../media/5-layer-diagram.png)

## <a name="summary"></a>Sammanfattning

IaaS, PaaS och SaaS innehåller olika nivåer av hanterade tjänster. Du kan enkelt använda en kombination av dessa infrastrukturtyper. Du kan använda Office 365 på företagets datorer (SaaS). I Azure kan du vara värd för dina virtuella datorer (IaaS) och du kan använda Azure SQL Database (PaaS) för att lagra dina data. Tack vare molnets flexibilitet kan du använda valfri kombination för att få bästa möjliga resultat.

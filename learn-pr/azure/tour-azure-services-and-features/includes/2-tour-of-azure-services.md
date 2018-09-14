Microsoft Azure är en ständigt växande uppsättning molntjänster som hjälper din organisation att möta nuvarande och framtida affärsutmaningar. Azure ger dig friheten att skapa, hantera och distribuera program i ett enormt, globalt nätverk med dina favoritverktyg och -ramverk. Låt oss ta en snabb genomgång av de tjänster som Azure erbjuder på hög nivå.

> [!VIDEO https://www.microsoft.com/videoplayer/embed/RE2yuas]

## <a name="azure-services"></a>Azure-tjänster

Azure tillhandahåller en omfattande uppsättning av molnbaserade tjänster med funktioner har lagts till och förbättrad varje månad. 

[![Diagram som visar den övergripande vy över Azure-tjänster](../media/2-azure-services.png "Azure-tjänster")](../media/2-azure-services.png#lightbox)

Vi tar en närmare titt på några av de vanligaste funktionerna: 

- [Beräkning](#compute-services)
- [Nätverk ](#networking-services)
- [Lagring](#storage-services)
- [Mobile](#mobil-services)
- [Databaser](#database-services)
- [Webb](#web-services)

<a name="compute-services"></a>

### <a name="compute"></a>Beräkning

Compute Services är en av de främsta orsakerna till varför företag flyttar till Azure-plattformen. Azure tillhandahåller flera olika värdalternativ för program och tjänster, bland annat:

![Jämförelse mellan IaaS, PaaS och FaaS](../media/2-iaas-paas-faas.png)

Här följer några exempel på IaaS, PaaS och FaaS i Azure.

|  Typ  |  Tjänstnamn             | Tjänstfunktion                                                         |
|--------|---------------------------|--------------------------------------------------------------------------|
| IaaS   | Azure Virtual Machines    | Virtuella Windows- eller Linux-datorer i Azure                                     | 
| IaaS   | Azure Kubernetes Service  | Möjliggör hantering av ett kluster av virtuella datorer som kör tjänster i containrar   |
| PaaS   | Azure Service Fabric      | Plattform för distribuerade system. Körs i Azure eller lokalt               |
| PaaS   | Azure Batch               | Hanterad tjänst för parallella och högpresterande databehandlingsprogram |
| PaaS   | Azure Cloud Services      | Hanterad tjänst för att köra molnprogram                           |
| FaaS   | Azure Container Instances | Tillhandahåller containrar utan att kräva VM-etablering eller högre tjänster    |
| FaaS   | Azure Functions           | Hanterad FaaS-tjänst                                                     |

<a name="network-services"></a>

### <a name="networking"></a>Nätverk

Att länka beräkningsresurser och ge åtkomst till program är huvudfunktionen i Azure-nätverk. Nätverksfunktioner i Azure omfattar en mängd alternativ för att ansluta omvärlden till tjänster och funktioner i de globala Microsoft Azure-datacentren.

Azure-nätverksresurser har följande funktioner:

|  Tjänstnamn             | Tjänstfunktion                                                                     |
| -------------             | -------------                                                                        |
| Azure Virtual Network     | Ansluter virtuella datorer till inkommande VPN-anslutningar (virtuellt privat nätverk)                   |
| Azure Load Balancer       | Balanserar inkommande och utgående anslutningar till program och tjänstslutpunkter       |
| Azure Application Gateway | Optimerar appservergruppens leverans samtidigt som programsäkerheten ökar             |
| Azure VPN Gateway         | Ansluter till virtuella Azure-nätverk via högpresterande VPN-gatewayer                |
| Azure DNS                 | Tillhandahåller ultrasnabba DNS-svar och ultrahög domäntillgänglighet                 |
| Azure Content Delivery Network  | Levererar innehåll med hög bandbredd till kunder världen över                          |
| Azure DDoS Protection     | Skyddar program i Azure mot distribuerade överbelastningsattacker (DDoS) |
| Azure Traffic Manager     | Distribuerar nätverkstrafik mellan Azure-regioner över hela världen                           |
| Azure ExpressRoute        | Ansluter till Azure via dedikerade säkra anslutningar med hög bandbredd                   |
| Azure Network Watcher     | Övervakar och diagnostiserar nätverksproblem med hjälp av scenariobaserad analys                  |
| Azure Firewall            | Implementerar brandvägg med hög säkerhet, hög tillgänglighet och obegränsad skalbarhet      |
| Azure Virtual WAN         | Skapar ett enhetligt WAN-nätverk, som kopplar samman lokala och fjärranslutna platser         |

<a name="storage-services"></a>

### <a name="storage"></a>Lagring

Azure tillhandahåller fyra huvudsakliga typer av lagringstjänster. Dessa tjänster är:

- **Azure Blob Storage** – tillhandahåller lagring för mycket stora objekt, till exempel videofiler eller bitmappar
- **Azure File Storage** – skapar filresurser som du kan komma åt och hantera som en filserver
- **Azure Queue Storage** – implementerar ett lager för köer och tillförlitlig leverans av meddelanden mellan program
- **Azure Table Storage** – består av ett NoSQL-lager som är värd för ostrukturerade data oberoende av scheman

Alla dessa tjänster har gemensamma egenskaper, som är följande:

- Pålitlig och mycket tillgänglig med redundans och replikering.
- Säker med hjälp av automatisk kryptering och rollbaserad åtkomstkontroll.
- Skalbar med nästan obegränsad lagring.
- Hanterad, tar hand om underhåll och kritiska problem åt dig.
- Åtkomlig från hela världen via HTTP eller HTTPS varifrån som helst.

<a name="mobile-services"></a>

### <a name="mobile"></a>Mobilt

Med Azure kan utvecklare skapa engagerande iOS-, Android- och Windows-appar snabbt och enkelt på en mängd olika språk med den utvecklingsmiljö de väljer. Funktioner som används för att ta tid och öka risker, till exempel att lägga till företagsövergripande inloggning på och sedan ansluta till lokala resurser som SAP, Oracle, SQL Server och SharePoint, nu är enkla att inkludera.

Andra funktioner i den här tjänsten:

- Synkronisering av offlinedata.
- Anslutning till lokala data.
- Sändning av push-meddelanden.
- Automatisk skalning utifrån företagets behov.

<a name="database-services"></a>

### <a name="databases"></a>Databaser

Azure tillhandahåller flera databastjänster för att lagra en mängd olika datatyper och volymer. Och med global anslutning blir dessa data tillgängliga för användare direkt.

|  Tjänstnamn              | Tjänstfunktion                                                                                |
| -------------              | -------------                                                                                   |
| Azure Cosmos DB            | Globalt distribuerad databas med stöd för NoSQL-alternativ                                       |
| Azure SQL Database         | Helt hanterad relationsdatabas med automatisk skalning, integrerad intelligens och robust säkerhet    |
| Azure Database for MySQL   | Fullständigt hanterad och skalbar MySQL-relationsdatabas med hög tillgänglighet och säkerhet        |
| Azure Database for PostgreSQL   | Fullständigt hanterad och skalbar PostgreSQL-relationsdatabas med hög tillgänglighet och säkerhet   |
| SQL Server på virtuella datorer          | Var värd för SQL Server-företagsappar i molnet                                                    |
| Azure SQL Data Warehouse   | Fullständigt hanterat informationslager med integrerad säkerhet på alla skalningsnivåer utan extra kostnader    |
| Azure Database Migration Service    | Migrerar databaser till molnet utan ändringar i programkoden                  |
| Azure Redis Cache          | Cachelagrar data som används ofta och statiska data för att minska svarstiden för data och program                   |
| Azure Database for MariaDB | Fullständigt hanterad och skalbar MySQL-relationsdatabas med hög tillgänglighet och säkerhet        |

<a name="web-services"></a>

### <a name="web"></a>Webb

Webbtjänster i Azure omfattar följande resurser:

| Tjänstnamn | Beskrivning |
|--------------|-------------|
| Azure App Service | Skapa snabbt kraftfulla molnappar för webben och mobilen. |
| Azure Notification Hubs |Skicka push-meddelanden till valfri plattform från valfri serverdel. |
| Azure API Management | Publicera API: er till utvecklare, partner och anställda på ett säkert sätt och efter behov. |
| Azure Search | Fullständigt hanterad sökning som en tjänst. |
| Funktionen Web Apps i Azure App Service | Skapa och distribuera verksamhetskritiska webbappar efter behov. |
| Azure SignalR Service | Lägg till funktioner i realtid web enkelt. |

Nu när vi har identifierat några av de områden som kan vara intressanta ett företag som vill migrera till Azure ska vi titta på vad som krävs för att använda tjänster och funktioner.

Microsoft Azure är en ständigt växande uppsättning molntjänster som hjälper din organisation att möta nuvarande och framtida affärsutmaningar. Azure ger dig friheten att skapa, hantera och distribuera program i ett enormt, globalt nätverk med dina favoritverktyg och -ramverk. I det här avsnittet får du en övergripande genomgång av de resurser Azure erbjuder.

## <a name="azure-services"></a>Azure-tjänster

Microsoft Azure tillhandahåller en mängd olika molnbaserade tjänster, och funktioner läggs till och förbättras varje månad.

![Azure-tjänster](../images/image204.png)

Vi tar en närmare titt på några av de vanligaste funktionerna: 

- Databearbetning
- Nätverk 
- Storage
- Mobilt 
- Databaser 
- Webb

### <a name="compute"></a>Databearbetning

Compute Services är en av de främsta orsakerna till varför företag flyttar till Azure-plattformen. Azure tillhandahåller flera olika värdalternativ för program och tjänster, bland annat:

- Infrastruktur som en tjänst (**IaaS**)
- Plattform som en tjänst (**PaaS**)
- Funktioner som en tjänst (**FaaS**)

Tjänsterna är följande:

|  Tjänstnamn             | Tjänstfunktion                                                          |  Typ     |
| -------------             | -------------                                                             | --------- |
| Virtuella datorer          | Virtuella Windows- eller Linux-datorer i Azure                                      | IaaS      |
| Azure Container Service   | Möjliggör hantering av ett kluster av virtuella datorer som kör tjänster i containrar    | IaaS      |
| Service Fabric            | Plattform för distribuerade system. Körs i Azure eller lokalt                | PaaS      |
| Azure Batch               | Hanterad tjänst för parallella och högpresterande databehandlingsprogram  | PaaS      |
| Cloud Services            | Hanterad tjänst för att köra molnprogram                            | PaaS      |
| Azure Container Instances | Tillhandahåll containrar utan att kräva VM-etablering eller högre tjänster      | FaaS      |
| Azure Functions           | Hanterad FaaS-tjänst                                                      | FaaS      |

### <a name="networking"></a>Nätverk

Att länka beräkningsresurser och ge åtkomst till program är huvudfunktionen i Azure-nätverk. Nätverksfunktioner i Azure omfattar en mängd alternativ för att ansluta omvärlden till tjänster och funktioner i de globala Microsoft Azure-datacentren.

Azure-nätverksresurser har följande funktioner:

|  Tjänstnamn             | Tjänstfunktion                                                                      |
| -------------             | -------------                                                                         |
| Virtual Network           | Anslut virtuella datorer till inkommande VPN-anslutningar (virtuellt privat nätverk)                     |
| Load Balancer             | Balansera inkommande och utgående anslutningar till program och tjänstslutpunkter         |
| Application Gateway       | Optimera appservergruppens leverans samtidigt som programsäkerheten ökar               |
| VPN Gateway               | Anslut till Azure Virtual Networks via högpresterande VPN-gatewayer                   |
| Azure DNS                 | Tillhandahåll ultrasnabba DNS-svar och ultrahög domäntillgänglighet                   |
| Content Delivery Network  | Leverera innehåll med hög bandbredd till kunder världen över                                  |
| Azure DDoS Protection     | Skydda program i Azure mot DDoS-attacker (Distributed Denial of Service)   |
| Traffic Manager           | Distribuera nätverkstrafik mellan Azure-regioner över hela världen                             |
| ExpressRoute              | Anslut till Azure via dedikerade säkra anslutningar med hög bandbredd                     |
| Network Watcher           | Övervaka och diagnostisera nätverksproblem med hjälp av scenariobaserad analys                     |
| Azure Firewall            | Implementera brandvägg med hög säkerhet, hög tillgänglighet och obegränsad skalbarhet         |
| Virtuellt WAN               | Skapa ett enhetligt WAN-nätverk, som kopplar samman lokala och fjärranslutna platser           |

### <a name="storage"></a>Storage

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

### <a name="mobile"></a>Mobilt

Med Azure kan utvecklare skapa engagerande iOS-, Android- och Windows-appar snabbt och enkelt på en mängd olika språk med den utvecklingsmiljö de väljer. Funktioner som tidigare tog tid och ökade risken för projektet, till exempel att lägga till företagsinloggning och sedan ansluta till lokala resurser som SAP, Oracle, SQL Server och SharePoint, är nu enkla att lägga till.

Andra funktioner i den här tjänsten:

- Synkronisering av offlinedata.
- Anslutning till lokala data.
- Sändning av push-meddelanden.
- Automatisk skalning utifrån företagets behov.

### <a name="databases"></a>Databaser

Azure tillhandahåller flera databastjänster för att lagra en mängd olika datatyper och volymer. Och med global anslutning blir dessa data tillgängliga för användare direkt.

|  Tjänstnamn             | Tjänstfunktion                                                                                |
| -------------             | -------------                                                                                   |
| Azure Cosmos DB           | Globalt distribuerad databas med stöd för NoSQL-alternativ                                       |
| Azure SQL Database        | Fullständigt hanterad relationsdatabas med automatisk skalning, integrerad intelligens och robust säkerhet    |
| Azure Database for MySQL  | Fullständigt hanterad och skalbar MySQL-relationsdatabas med hög tillgänglighet och säkerhet        |
| Azure DB för PostgreSQL   | Fullständigt hanterad och skalbar PostgreSQL-relationsdatabas med hög tillgänglighet och säkerhet   |
| SQL Server på virtuella datorer         | var värd för SQL Server-företagsappar i molnet                                                  |
| SQL Data Warehouse        | Fullständigt hanterat informationslager med integrerad säkerhet på alla skalningsnivåer utan extra kostnader    |
| Azure DB-migreringstjänst    | Migrera databaser till molnet utan ändringar i programkoden                            |
| Redis Cache               | Cachelagra data som används ofta och statiska data för att minska svarstiden för data och program                    |
| Azure DB för MariaDB      | Fullständigt hanterad och skalbar MySQL-relationsdatabas med hög tillgänglighet och säkerhet        |

### <a name="web"></a>Webb

Webbtjänster i Azure omfattar följande resurser:

- App Service – skapa snabbt kraftfulla molnappar för webben och mobilen
- Content Delivery Network – säkerställ säker och tillförlitlig innehållsleverans med bred global räckvidd
- Notification Hubs – skicka push-meddelanden till valfri plattform från valfri serverdel
- API Management – publicera API:er till utvecklare, partner och anställda på ett säkert sätt och efter behov
- Azure Search – fullständigt hanterad sökning som en tjänst
- Web Apps – skapa och distribuera affärskritiska webbappar snabbt och efter behov
- Azure SignalR Service – lägg enkelt till webbfunktioner i realtid

## <a name="summary"></a>Sammanfattning

Nu har du fått en översikt över de tjänster som Azure erbjuder. Du har också identifierat några av de viktigaste områdena som kan vara av intresse för ett företag som vill migrera till Azure för att hantera problem avseende skalbarhet och hantering av hög belastning. I nästa enhet registrerar du dig för ett kostnadsfritt Azure-konto och loggar in på portalen.

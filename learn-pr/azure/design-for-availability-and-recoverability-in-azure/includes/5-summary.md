Vi har diskuterat hur du gör ditt program tillgängligt och återställningsbart för hög tillgänglighet, haveriberedskap och säkerhetskopior. Låt oss ta en titt på några av huvudpunkterna.

## <a name="high-availability"></a>Hög tillgänglighet

- Hög tillgänglighet (HA, High Availability) är förmågan att hantera förlust av eller allvarlig försämring av en komponent i ett system.
- Utvärdera ditt program för hög tillgänglighet genom att fokusera på tre huvudområden:
  - Det definierade serviceavtalet.
  - Programmets HA-funktioner.
  - HA-funktionerna i beroende program.
- Använd tillgänglighetsuppsättningar och tillgänglighetszoner i Azure för att tillhandahålla HA för VM-arbetsbelastningar.
- Använd tjänster för belastningsutjämning såsom Azure Traffic Manager, Azure Application Gateway och Azure Load Balancer för att distribuera belastningen över de tillgängliga systemen.
- PaaS-tjänster har HA inbyggt, så du bör utnyttja de här tjänsterna i din arkitektur där det är möjligt.

## <a name="disaster-recovery"></a>Haveriberedskap

- Haveriberedskap (DR, Disaster Recovery) handlar om att *återställa från händelser med stor inverkan* som leder till driftstopp och dataförlust.
- Skapa en haveriberedskapsplan för att definiera procedurer, ansvarsområden och aktiviteter som krävs för att återställa efter en katastrof.
- Definiera RPO och RTO för ditt program för att fastställa DR-kraven för ditt program.
- Använd säkerhetskopiering och replikering för att tillhandahålla kopior av systemen och data som det går att återställa till.
- Använd Azure Site Recovery för återställningsfunktioner för DR-processen för programmet.
- Testa din haveriberedskapsplan för att identifiera brister och relevans hos stegen.

## <a name="backup-and-restore"></a>Säkerhetskopiering och återställning

- Använd säkerhetskopiering för att återställa dina data som en del av din DR-strategi eller för mer isolerade scenarier med dataförlust.
- Använd Azure Backup för fullständig säkerhetskopiering av virtuella datorer, filer och mappar samt system i en lokal miljö.
- Funktioner för säkerhetskopiering är ofta inbyggda i PaaS-tjänster, till exempel Azure SQL Database och Azure App Service. Dra nytta av säkerhetskopiering när det är möjligt, och se till att känna till standardkonfigurationen och de övergripande funktionerna.
- Testa dina processer och procedurer för återställning regelbundet för att säkerställa att de är giltiga och tillräckliga.

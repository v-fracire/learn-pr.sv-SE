Vi har beskrivs hur du gör ditt program och data kan återställas genom att utforma för hög tillgänglighet, haveriberedskap och säkerhetskopiering. Låt oss ta en titt på några av de viktiga takeaways.

## <a name="high-availability"></a>Hög tillgänglighet

- Hög tillgänglighet är förmågan att hantera förlust av eller allvarlig försämring av en komponent i ett system.
- Utvärdera ditt program för hög tillgänglighet genom att fokusera på tre huvudområden:
  - Definierade serviceavtalet.
  - Funktioner för hög tillgänglighet för ditt program.
  - Funktioner för hög tillgänglighet för beroende program.
- Använd tillgänglighetsuppsättningar och tillgänglighetszoner i Azure för att tillhandahålla hög tillgänglighet för VM-arbetsbelastningar.
- Använda belastningsutjämning för tjänster som Azure Traffic Manager, Azure Application Gateway och Azure Load Balancer för att distribuera belastningen över tillgängliga system.
- PaaS-tjänster har hög tillgänglighet inbyggda, så att utnyttja de här tjänsterna i din arkitektur där det är möjligt.

## <a name="disaster-recovery"></a>Haveriberedskap

- Haveriberedskap handlar om att *återställa från kraftfulla händelser* som leder till driftstopp och dataförlust.
- Skapa en återställningsplan för att definiera procedurer, ansvarsområden och aktiviteter som krävs för att återställa efter en katastrof.
- Definiera RPO och RTO för ditt program för att fastställa krav för DR för ditt program.
- Använda säkerhetskopiering och replikering för att tillhandahålla kopior av dina system och data att återställa till.
- Använd Azure Site Recovery för återställningsfunktioner för DR-processen för ditt program.
- Testa din haveriberedskapsplan för att identifiera brister och relevans steg.

## <a name="backup-and-restore"></a>Säkerhetskopiering och återställning

- Använd säkerhetskopior för att återställa dina data som en del av din strategi för Katastrofåterställning eller för mer isolerade data går förlorade.
- Använda Azure Backup för fullständig säkerhetskopiering, fil och säkerhetskopiering av mappar och säkerhetskopiering av system i en lokal miljö i virtuell dator.
- Säkerhetskopiering är ofta inbyggda i PaaS-tjänster, till exempel Azure SQL Database och Azure App Service. Dra nytta av funktionerna säkerhetskopiering när det är möjligt, förstå sina standardkonfigurationen och övergripande funktioner.
- Testa dina återställning processer och procedurer för regelbundet för att säkerställa att de är giltiga och tillräckligt med.

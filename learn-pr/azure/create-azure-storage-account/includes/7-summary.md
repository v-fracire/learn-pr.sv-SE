Med lagringskonton kan du skapa en grupp med regler för datahantering och tillämpa dem allihop samtidigt på en grupp med Azure-blobar, Azure-filer, Azure-köer och Azure-tabeller. 

Att försöka uppnå samma sak utan lagringskonton skulle vara tidsödande och felbenäget. Hur stor skulle till exempel chansen vara att lyckas med att tillämpa exakt samma regeluppsättning på tusentals blobar?

I stället sparar du reglerna i inställningarna för ett lagringskonto, och de reglerna tillämpas automatiskt på varje datatjänst i kontot.
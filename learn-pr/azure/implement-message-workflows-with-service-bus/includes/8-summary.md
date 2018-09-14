I den här modulen skapade du ett Service Bus-namnområde, en kö och ett ämne i din prenumeration med hjälp av Azure-portalen och du skickar och tar emot meddelanden via kön och ämnet.

Service Bus-köer och ämnen är utmärkta verktyg som du kan använda för att öka flexibiliteten för kommunikationen inom ett distribuerat program. Genom att agera som en temporär lagringsplats, undviker de kravet på direkt kommunikation mellan komponenter och hanterar toppar i efterfrågan smidigt. Överväg att använda dem när du har en komponent som kan kommunicera med en annan komponent i en löst kopplade konfiguration.

[!include[](../../../includes/azure-sandbox-cleanup.md)]
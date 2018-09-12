I den här modulen skapade du ett Service Bus-namnområde, en kö och ett ämne i din prenumeration med hjälp av Azure-portalen och du skickar och tar emot meddelanden via kön och ämnet.

Service Bus-köer och ämnen är utmärkta verktyg som du kan använda för att öka flexibiliteten för kommunikationen inom ett distribuerat program. Genom att agera som en temporär lagringsplats, undviker de kravet på direkt kommunikation mellan komponenter och hanterar toppar i efterfrågan smidigt. Överväg att använda dem när du har en komponent som kan kommunicera med en annan i en löst kopplade konfiguration.

## <a name="clean-up"></a>Rensa

Service Bus-köer och ämnen i Azure-prenumerationen debiteras en kostnad även om den sannolikt är rätt liten när det finns få, små meddelanden. Det enklaste sättet att rensa i din Azure-prenumeration är att ta bort den resursgrupp du skapade under vår första övning. Då raderas även alla ämnen, köer, namnområden och andra resurser i gruppen. När du är färdig med den här modulen vidtar du följande steg:

1. I **Azure-portalen** i navigeringen på vänster sida, klickar du på **Resursgrupper**.
1. I listan med resursgrupper klickar du på **SalesTeamRG**.
1. I bladet **Resursgrupp**, klickar du på **Ta bort resursgrupp**.
1. I textrutan **Skriv in resursgruppnamnet** skriver du **SalesTeamAppRG** och klickar sedan på **Ta bort**. Det tar bort resursgruppen och alla dess resurser.
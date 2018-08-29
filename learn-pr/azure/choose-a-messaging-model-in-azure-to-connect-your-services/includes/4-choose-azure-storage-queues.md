Anta att du planerar arkitekturen för ditt musikdelningsprogram. Du vill se till att musikfilerna överförs till webb-API:n på ett tillförlitligt sätt från mobilappen.

Eftersom du har förstått att den här kommunikationen ska vara ett meddelande, måste du välja om du ska använda Azure Storage-köer eller Azure Service Bus. Båda kan användas för att lagra köade meddelanden som väntar på bearbetning.

## <a name="delivery-guarantees"></a>Leveransgarantier

Kösystem garanterar vanligtvis att varje meddelande i kön levereras till en målkomponent. Dessa garantier kan dock fungera på olika sätt:

- **Leverans minst en gång.** Den här metoden garanterar att varje meddelande levereras till minst en av de komponenter som hämtar meddelanden från kön. Observera att i vissa fall kan det hända att samma meddelande levereras mer än en gång. Om det till exempel finns två instanser av en webbapp som hämtar meddelanden från en kö, skickas vanligtvis varje meddelande till endast en av dessa instanser. Men om det tar lång tid för en instans att bearbeta meddelandet och en tidsgräns går ut, kan meddelandet skickas till den andra instansen också. Webbappkoden bör utformas med den här möjligheten i åtanke.
- **Leverans högst en gång.** Med den här metoden kan man inte garantera att varje meddelande levereras och det finns en mycket liten risk att det inte kommer fram alls. Det finns dock ingen risk att meddelandet levereras två gånger som med alternativet Leverans minst en gång.
- **Först-in-först-ut (FIFO).** I de flesta meddelandesystem lämnar meddelanden vanligtvis kön i samma ordning som de har lagts till, men du bör fundera på om den här ordningen ska vara garanterad. Om det distribuerade programmet kräver att meddelanden behandlas i exakt rätt ordning, måste du välja ett kösystem med FIFO-säkerhet.

## <a name="transactions"></a>Transaktioner

Vissa nära relaterade grupper av meddelanden kan orsaka problem om leveransen misslyckas för ett av meddelandena i gruppen.

Tänk dig exempelvis ett e-handelsprogram. När användaren klickar på ”Köp”-knappen skickas ett meddelande med orderinformationen, tillsammans med ett meddelande som innehåller kreditkortsinformationen. Om kreditkortsmeddelandet inte levereras kan ordern komma att skickas utan att någon betalning gjorts.

Du kan undvika dessa typer av problem genom att gruppera de två meddelandena i en transaktion. Transaktionerna lyckas eller misslyckas som en enda enhet. Om leveransen av kreditkortsinformationen misslyckas, kommer även orderinformationen att göra det.

## <a name="when-to-use-storage-queues"></a>När ska man använda Storage-köer?

Köer används för meddelanden, men inte händelser.

Azure Storage-konton innehåller köer som kan användas av distribuerade program som enkla tillfälliga lagringsplatser för meddelanden. En källkomponent kan lägga till ett meddelande i kön. Målkomponenter hämtar det meddelande som ligger först i kön för bearbetning. Sådana köer ökar tillförlitligheten för meddelanden eftersom det vid hög efterfrågan kan innebära att meddelanden får vänta tills en målkomponent är redo att bearbeta dem.

Köer ingår också som en del av Azure Service Bus-meddelandefunktionen. Om du har valt att använda en kö i Azure för en viss kommunikation, måste du bestämma om du vill använda en Storage-kö eller en Service Bus-kö.

Tänk på följande när du ska göra detta val:

- Behöver du en garanti om leverans högst en gång?
- Behöver du FIFO-säkerhet?
- Behöver du gruppera meddelanden i transaktioner?
- Behöver du lagra meddelanden som är större än 64 KB?

Om svaret på någon av dessa frågor är ja måste du använda en Service Bus-kö.

Tänk dessutom på följande:

- Behöver du serverloggar för alla meddelanden som skickas via kön?
- Förväntar du dig att kön kommer att överskrida 80 GB i storlek?

Om svaret på någon av dessa frågor är ja måste du välja en Storage-kö.

## <a name="summary"></a>Sammanfattning

En kö är en enkel tillfällig lagringsplats för meddelanden som skickas mellan komponenterna i ett distribuerat program. Använd en kö till att ordna meddelanden och hantera oväntade toppar i efterfrågan.

Använd Azure Storage-köer om du vill ha ett enkelt kösystem som är lätt att koda. Använd Azure Service Bus-köer för mer avancerade behov.
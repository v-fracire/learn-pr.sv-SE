Som nätverksadministratör på ett dataanalysföretag är du ansvarig för att ansluta till virtuella datorer i Azure och kontrollera att de är rätt konfigurerade. Detta kan göras med en Remote Desktop Protocol-klient som ansluter till den virtuella Windows-datorns användargränssnitt.

## <a name="what-is-the-remote-desktop-protocol"></a>Vad är Remote Desktop Protocol?

Med Remote Desktop Protocol (RDP) kan du fjärransluta till användargränssnittet i Windows-baserade datorer. Du kan logga in på en fjärransluten fysisk eller virtuell Windows-dator och styra datorn, precis som när du sitter vid den.

> [!Note]
> Windows Hyper-V i Windows Server 2016 och Windows 10 ansluter till virtuella datorer som körs i hypervisor-programmet med Remote Desktop Protocol.

Med en RDP-anslutning kan du utföra merparten av de åtgärder som du kan göra från konsolen på en fysisk dator, med undantag för vissa ström- och maskinvarurelaterade funktioner.

En RDP-anslutning kräver en RDP-klient. Microsoft tillhandahåller RDP-klienter för följande operativsystem:

* Windows
* iOS
* MacOS
* Android

Det finns även Linux-klienter med öppen källkod, till exempel Remmina som gör det möjligt att ansluta till en Windows-dator från en Ubuntu-distribution.

Windows 10 innehåller en RDP-klient.

![Windows RDP-klient](../media-drafts/4-rdp-client.PNG)

## <a name="what-functionality-does-an-rdp-connection-support"></a>Vilka funktioner stöds med en RDP-anslutning?

Du kan interagera med användargränssnittet med en RDP-anslutning. Du kan även ansluta till andra tjänster på fjärrdatorn. Några exempel är:

* Skrivaranslutningar så att du kan skriva ut till den lokala utskriftsenheten från fjärrdatorn.
* Ljuduppspelning där ljud kan spelas upp på den lokala datorn eller på den fjärranslutna enheten.
* Ljudinspelning där du kan spela in ljud från den lokala datorn och dirigera ljudet till den fjärranslutna enheten.
* Portar som kan omdirigeras till portar på den lokala datorn.
* Enheter – inklusive mappade nätverksenheter – som visas som anslutna till fjärrdatorn.
* Videoinspelningsenheter som integrerade webbkameror.
* Andra plug and play-enheter som stöds.

Alla dessa funktioner stöds inte i Azure. Det finns begränsningar för vilka fysiska resurser som är tillgängliga på plattformen. Till exempel kan endast programvaruskrivare omdirigeras via en RDP-anslutning från en Azure-baserad virtuell dator till den anslutna klientens utskriftsenheter.

## <a name="configure-network-settings-for-rdp-access-to-virtual-machines"></a>Konfigurera nätverksinställningarna för RDP-åtkomst till virtuella datorer

Du kan ansluta till virtuella Azure-datorer på tre sätt:

1. Direkt till den virtuella datorns offentliga IP-adress.
2. Via en anslutning för ett virtuellt privat nätverk (VPN).
3. Via en ExpressRoute-anslutning.

En offentlig IP-adress kan vara permanent tilldelad eller dynamisk. Du måste också se till att ha åtkomst till den virtuella datorns offentliga adress på porten 3389 (RDP). Det här kan innebära förhandlingar med teamet som hanterar din organisations brandväggssäkerhet, och att en regel konfigureras för din virtuella dators offentliga IP-adress på porten 3389.

Dynamiska offentliga IP-adresser tilldelas varje gång den virtuella datorn startas om. Statiska adresser är beständiga men kostar mer.

Om du vill ansluta via ett VPN måste det lokala nätverket ha en aktiv VPN-anslutning till Microsoft Azure.

ExpressRoute-länkmetoden hanterar anslutningen till den virtuella datorn. Den här metoden ger även lägst svarstid och högst bandbredd.

> [!Note]
> Oavsett vilket alternativ du använder för att ansluta till Azure måste du se till att den virtuella datorn nås via RDP, vanligtvis på standardporten 3389. Du kan konfigurera den virtuella datorn så att den nås enbart från din egen klient-IP-adress. Att ansluta till en virtuell dator via porten 3389 på en offentlig IP-adress rekommenderas endast för testmiljöer. Använd alternativ 2 eller 3 i produktionsmiljöer.

## <a name="how-do-you-connect-to-a-vm-in-azure-using-rdp"></a>Hur ansluter du till en virtuell dator i Azure med RDP?

Det är enkelt att ansluta till en virtuell dator i Azure med RDP. Gå till egenskaperna för din virtuella dator i Azure-portalen och klicka på **Anslut** högst upp. Genom den här åtgärden hämtas en förkonfigurerad .RDP-fil som Windows öppnar i RDP-klienten. Du kan välja att ansluta via den offentliga IP-adressen till den virtuella datorn i RDP-filen. Om du ansluter via VPN eller ExpressRoute kan du välja den interna IP-adressen. Du kan också välja portnumret för anslutningen.

Om du använder en statisk offentlig IP-adress för den virtuella datorn kan du spara .RDP-filen på skrivbordet. Om du använder dynamisk IP-adresshantering är .RDP-filen endast giltig när den virtuella datorn körs. Om du stoppar och startar om den virtuella datorn måste du hämta en annan .RDP-fil.

> [!Note]
> Du kan också ange den offentliga IP-adressen till den virtuella datorn i Windows RDP-klient och klicka på **Anslut**.

När du ansluter får du troligtvis två varningar. Dessa är:

* Utgivarvarning – .RDP-filen har inte signerats offentligt.
* Certifikatvarning – datorcertifikatet är inte betrott.

Dessa varningar kan ignoreras i testmiljö. I produktionsmiljö kan .RDP-filen signeras med hjälp av RDPSIGN. EXE och datorcertifikatet kan placeras i klientens arkiv för **betrodda rotcertifikatutfärdare**.

## <a name="summary"></a>Sammanfattning

Du kan nu ansluta till en virtuell Windows-dator med hjälp av RDP. I nästa övning kommer du att använda RDP för att ansluta till en virtuell dator, och sedan installerar du en serverroll på datorn.

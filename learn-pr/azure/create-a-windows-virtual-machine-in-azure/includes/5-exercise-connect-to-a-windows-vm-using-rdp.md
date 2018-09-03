Från vårt föregående scenario är du nu en student som vill lära dig om att lägga till roller och funktioner i en Windows Server-dator. Nätverksadministratören vill dock inte att du gör det här på en fysisk dator i nätverket, och universitetets datorer är inte tillräckligt kraftiga för att köra Windows Hyper-V. Azure har den perfekta lösningen – vi kan vara värd för dessa virtuella Windows-datorer i molnet och du kan komma åt dem med hjälp av Windows Remote Desktop Protocol (RDP). Nu ska vi använda fjärrskrivbordsklienten för att ansluta till den virtuella Windows-dator du skapade i föregående utbildningsenhet. Du kan upprätta anslutningen genom att hämta och köra en **.RDP**-fil från Azure Portal. 

Den här RDP-filen innehåller följande:

* den virtuella datorns offentliga IP-adress
* portnumret.

## <a name="configure-network-and-public-ip-address-settings"></a>Konfigurera inställningar för nätverk och offentliga IP-adresser

1. Se till att bladet för den virtuella dator du skapade tidigare är öppen i Azure Portal. Du hittar bladet under **Alla resurser** om du behöver öppna det.

1. Gå till avsnittet **Nätverk**. Längst upp i det här avsnittet finns länkar till det virtuella undernätet och dynamiska IP-adresser som skapades tillsammans med den virtuella datorn eftersom vi använde standardinställningarna. Vi skulle följa dessa länkar om vi ville ändra resurserna (till exempel för att byta till en statisk IP-adress).

1. Klicka på **Lägg till regel för inkommande port**.

1. Längst upp i dialogrutan **Lägg till inkommande säkerhetsregel** klickar du på **grundläggande**.

1. Under **Tjänst** väljer du **RDP** och klickar sedan på **Lägg till**.

## <a name="connect-to-the-vm-by-using-rdp"></a>Ansluta till den virtuella datorn med RDP

Så här hämtar du RDP-filen och ansluter till den virtuella datorn:

### <a name="download-the-rdp-file"></a>Ladda ned RDP-filen

1. I avsnittet **Översikt** på bladet för den virtuella datorn klickar du på **Anslut**.

1. På bladet **Anslut till virtuell dator** ska du notera inställningarna för **IP-adress** och **Portnummer**. Klicka sedan på **Ladda ned RDP-fil**.

1. Klicka på **Öppna** eller **Kör** i webbläsaren för att öppna RDP-filen.

### <a name="connect-to-the-windows-vm"></a>Ansluta till den virtuella Windows-datorn

1. I dialogrutan **Anslutning till fjärrskrivbord** noterar du säkerhetsvarningen och fjärrdatorns IP-adress, och klickar sedan på **Anslut**.

1. I dialogrutan **Windows-säkerhet** anger du samma användarnamn och lösenord som i steg 6 och 7.

1. I den andra dialogrutan **Anslutning till fjärrskrivbord** noterar du certifikatfelen och klickar sedan på **Ja**.

   > [!Note]
   > Det tar en stund innan den virtuella datorns skrivbord visas. Det här beror på B1-avbildningens bristfälliga specifikation. Du kan se ett meddelande om låg minnesallokering.

1. I dialogrutan **Nätverk** klickar du på **Nej**.

### <a name="resize-the-vm-in-the-azure-portal"></a>Ändra storlek på den virtuella datorn i Azure Portal

1. Växla tillbaka till Azure Portal. Under **Inställningar** på den virtuella datorns egenskapssida klickar du på **Storlek**.

1. Klicka på **D2s_v3** (2 virtuella processorer, 8 GB RAM-minne) och klicka sedan på **Välj**. Ett meddelande om storleksändringen visas. Den virtuella datorn i RDP-fönstret stängs också.

1. Växla tillbaka till Azure Portal. Klicka på **Virtuella datorer** i den vänstra rutan.

1. Under **Virtuella datorer** väntar du tills den virtuella datorn har statusen **Körs**. Du kan behöva klicka på **Uppdatera**.

1. Klicka på den virtuella datorns namn. Under **Inställningar** klickar du på **Storlek**. Nu är den virtuella datorns storlek inställd på **D2s_v3**.

### <a name="reconnect-to-the-resized-vm"></a>Återansluta till den ändrade virtuella datorn

1. Klicka på **Virtuella datorer** och sedan på din virtuella dator. Värdet för **Offentlig IP-adress** har förmodligen ändrats. Klicka på **Anslut** och sedan på **Kör** eller **Öppna** i webbläsaren.

1. I dialogrutan **Anslutning till fjärrskrivbord** noterar du säkerhetsvarningen och fjärrdatorns IP-adress, och klickar sedan på **Anslut**.

1. I dialogrutan **Windows-säkerhet** anger du samma användarnamn och lösenord som i steg 6 och 7.

1. I den andra dialogrutan **Anslutning till fjärrskrivbord** noterar du certifikatfelen och klickar sedan på **Ja**. Notera hur mycket snabbare det går att logga in på den virtuella datorn.

1. Högerklicka i aktivitetsfältet och klicka på **Aktivitetshanteraren**.

1. I **Aktivitetshanteraren** klickar du på **Mer information**.

1. Klicka på fliken **Prestanda**. Den totala mängden tillgängligt minnet är 8 GB, och ungefär 1,2 GB används. Stäng **Aktivitetshanteraren**.

## <a name="summary"></a>Sammanfattning

Du har nu anslutit en virtuell Windows-dator med hjälp av RDP. Du kan administrera den här virtuella datorn precis som vanliga Windows-datorer via skrivbordet.

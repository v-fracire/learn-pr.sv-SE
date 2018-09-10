Nu när vi har en virtuell Linux-dator i Azure bör du som nästa steg konfigurera den för uppgifter som vi vill flytta till Azure.

Om du inte har konfigurerat plats-till-plats-VPN-anslutning till Azure kan dina virtuella datorer i Azure inte nås från det lokala nätverket. Om du precis har kommit igång med Azure har du troligen inte en fungerande plats-till-plats-VPN-anslutning. Så hur kan du ansluta till den virtuella datorn?

## <a name="azure-vm-ip-addresses"></a>IP-adresser för virtuell Azure-dator

Som vi såg alldeles nyss kommunicerar virtuella Azure-datorer i ett virtuellt nätverk. De kan också ha en valfri offentlig IP-adress som har tilldelats. Med en offentlig IP-adress kan vi interagera med den virtuella datorn via Internet. Vi kan också konfigurera ett virtuellt privat nätverk (VPN) som ansluter det lokala nätverket till Azure. Då kan vi på ett säkert sätt ansluta till den virtuella datorn utan att exponera en offentlig IP-adress. Den här metoden beskrivs i en annan modul och är fullständigt dokumenterad om du vill utforska det alternativet.

En sak att tänka på med offentliga IP-adresser i Azure är att de ofta tilldelas dynamiskt. Det innebär att IP-adressen kan ändras över tid – för virtuella datorer sker det när den virtuella datorn startas om. Du kan betala mer för att tilldela statiska adresser om du vill ansluta direkt till en IP-adress i stället för ett namn och se till att IP-adressen inte ändras.

## <a name="connect-to-an-azure-linux-vm-with-ssh"></a>Ansluta till en virtuell Azure Linux-dator med SSH

Att ansluta till en virtuell dator i Azure med hjälp av SSH är enkelt. Öppna egenskaperna för din virtuella dator i Azure-portalen och klicka på **Anslut** högst upp. Då visas IP-adresser tilldelade till den virtuella datorn tillsammans med alla inloggningsuppgifterna för SSH. 

![SSH-anslutningsinformation för virtuell Linux-dator](../media-drafts/5-connect-ssh.png)

Med dessa uppgifter kan vi använda följande kommando för att komma in den virtuella Linux-datorn:

```bash
ssh jim@137.117.101.249
```

Första gången vi ansluter ber SSH oss om att autentisera mot en okänd värd. SSH berättar att du aldrig har anslutit till den här servern tidigare. Om det är sant är det helt normalt och du kan svara ”ja” om du vill spara fingeravtrycket för servern i den kända värdfilen.

```output
The authenticity of host '137.117.101.249 (137.117.101.249)' can't be established.
ECDSA key fingerprint is SHA256:w1h08h4ie1iMq7ibIVSQM/PhcXFV7O7EEhjEqhPYMWY.
Are you sure you want to continue connecting (yes/no)? yes
Warning: Permanently added '137.117.101.249' (ECDSA) to the list of known hosts.
```

## <a name="transferring-files-to-the-vm"></a>Överföra filer till den virtuella datorn

En annan vanlig uppgift är att kopiera lokala filer och data från en server till en annan. I vårt fall vill vi i slutänden kopiera våra webbplatsdata till den virtuella Azure-datorn. Med virtuella Linux-datorer och SSH kan vi använda kommandot `scp`. Kommandot liknar att kopiera lokala filer med `cp`, men du måste ange fjärranvändaren och värden i kommandot. 

Om du till exempel vill kopiera `somefile.txt` från vår aktuella katalog till `~/folder` på en Linux-dator med IP-adressen `192.168.1.25` kan du skriva:

```bash
scp somefile.txt someuser@192.168.1.25:~/folder/
```

Detta autentiserar som `someuser` på fjärrsystemet, frågar om ett lösenord eller använder din privata SSH-nyckeln. Användaren måste ha skrivbehörighet för `~/folder/` på målservern.

> [!WARNING]
> `scp` gör lokala filkopior om kommandoraden inte blir helt rätt. Den vanligaste biten som saknas är målmappen.

Nu ska vi prova att använda SSH för att ansluta till den virtuella Linux-datorn som körs.
Innan vi kan skapa en virtuell Linux-dator i Azure måste vi fundera på fjärråtkomst. Vi vill kunna logga in på Linux-webbservern så att vi kan konfigurera programvaran och utföra underhåll. Standardmetoden för administration av virtuella Linux-datorer i Azure är SSH.

## <a name="what-is-ssh"></a>Vad är SSH?

Secure Shell (SSH) är ett krypterat anslutningsprotokoll som möjliggör säkra inloggningar via oskyddade anslutningar. Med SSH kan du ansluta till ett terminalgränssnitt från en fjärransluten plats med hjälp av en nätverksanslutning.

Det finns två metoder som vi kan använda för att autentisera en SSH-anslutning: **användarnamn/lösenord ** eller ett **SSH-nyckelpar**. 

> [!TIP]
> Även om SSH ger en krypterad anslutning blir den virtuella datorn sårbar för råstyrkeattacker och gissade lösenord om du använder lösenord med SSH-anslutningar. En säkrare och lämpligare metod för att ansluta till en virtuell Linux-dator med SSH är med ett offentligt/privat nyckelpar, som även kallas SSH-nycklar.

Med ett SSH-nyckelpar kan du logga in på Linux-baserade virtuella Azure-datorer utan lösenord. Det här är en säkrare metod om du bara planerar att logga in från några få datorer. Om du behöver kunna komma åt den virtuella Linux-datorn från ett antal olika platser kanske en kombination av användarnamn och lösenord är en bättre metod. Ett SSH-nyckelpar består av två delar: en offentlig nyckel och en privat nyckel.

* Den offentliga nyckeln placeras på den virtuella Linux-datorn eller en annan tjänst som du vill använda med kryptering med offentliga nycklar. Den kan delas med vem som helst.

* Den privata nyckeln är den du anger på den virtuella Linux-datorn när du upprättar en SSH-anslutning, för att verifiera din identitet. Det här är konfidentiell information som du måste skydda precis som ett lösenord eller andra privata uppgifter.

Du kan använda samma enskilda offentliga/privata nyckelpar för att få åtkomst till flera virtuella Azure-datorer och tjänster.

## <a name="create-the-ssh-key-pair"></a>Skapa SSH-nyckelpar

I Linux, Windows 10 och MacOS kan du använda det inbyggda kommandot `ssh-keygen` för att generera de offentliga och privata SSH-nyckelfilerna. 

> [!TIP]
> Windows 10 innehåller en SSH-klient i **Fall Creators Update**. Tidigare versioner av Windows kräver ytterligare programvara för att använda SSH – [dokumentationen innehåller fullständig information](https://docs.microsoft.com/azure/virtual-machines/linux/ssh-from-windows) Du kan också installera Linux-undersystemet för Windows och få samma funktioner.

> [!NOTE]
> Vi använder Azure Cloud Shell som lagrar de genererade nycklarna i Azure på ditt privata lagringskonto. Du kan också skriva dessa kommandon direkt i det lokala gränssnittet om du vill. Du behöver anpassa instruktionerna i hela den här modulen utifrån en lokal session om du använder den här metoden.

Här är minimikommandot som krävs för att generera nyckelparet för en virtuell Azure-dator. Det skapar ett offentligt/privat SSH-2 (SSH-protokoll 2) RSA-nyckelpar med 2 048 bitars längd (minimilängden). 

Ange det här kommandot i Cloud Shell.

```bash
ssh-keygen -t rsa -b 2048
```

Verktyget efterfrågar filnamn och en valfri lösenfras. Välj standardvärdena. Två filer skapas: `id_rsa` och `id_rsa.pub` i katalogen `~/.ssh`. Filerna skrivs över om de finns. Det finns olika alternativ som du kan använda för att ange filnamnet eller en lösenfras och undvika frågan.

### <a name="private-key-passphrase"></a>Lösenfras för privat nyckel

Du kan också ange en lösenfras när du genererar din privata nyckel. Det är ett lösenord som du måste ange när du använder nyckeln. Den här lösenfrasen används för att få åtkomst till den privata SSH-nyckelfilen och är inte lösenordet för användarkontot. 

När du lägger till en lösenfras för SSH-nyckeln krypteras den privata nyckeln med 128-bitars AES, så att den inte kan användas utan lösenfrasen som krävs för att dekryptera den. 

> [!IMPORTANT]
> Vi rekommenderar **starkt** att du skapar en lösenfras. Om en angripare stjäl din privata nyckel och om nyckeln inte skyddas med en lösenfras, kan angriparen använda den privata nyckeln för att logga in på alla servrar som den offentliga nyckeln används för. Om en lösenfras skyddar en privat nyckel kan den inte användas av angriparen, vilket ger infrastrukturen i Azure ett extra säkerhetslager.

Här är ett exempel som visar hur du anger lösenfrasen. Du behöver inte köra det här kommandot (men du kan göra det om du vill).

```bash
ssh-keygen -t rsa -b 4096 \
    -C "azureuser@myserver" \
    -f ~/.ssh/mykeys/myprivatekey \
    -N someReallySecurePhraseYouWillRemember
```

| Parameter | Vad läget gör |
|-----------|--------------|
| `-t` | Typ av nyckel som ska skapas. Måste vara **rsa**. |
| `-b` | Antal bitar i nyckeln. Minsta längd är 2 048, högsta är 4 096. |
| `-C` | En valfri kommentar som läggs till för den offentliga nyckeln och kan användas för att identifiera den. Det här är normalt en e-postadress, men det är enkel text och du kan använda den identifieringsmetod som du föredrar. |
| `-f` | Plats och filnamn för den privata nyckelfilen. En motsvarande offentlig nyckelfil med tillägget **.pub** genereras i samma katalog. Katalogen måste finnas. |
| `-N` | Lösenfrasen som används för att kryptera den privata nyckeln. |

## <a name="use-the-ssh-key-pair-with-an-azure-linux-vm"></a>Använda SSH-nyckelparet med en virtuell Linux-dator i Azure

När du har genererat nyckelparet kan du använda det med en virtuell Linux-dator i Azure. Du kan ange den offentliga nyckeln när den virtuella datorn skapas eller lägga till den efter att den virtuella datorn har skapats. 

Du kan visa filens innehåll i Azure Cloud Shell med följande kommando.

```bash
cat ~/.ssh/id_rsa.pub
```

Det är en enda rad och ser ut ungefär så här:

```output
ssh-rsa XXXXXXXXXXc2EAAAADAXABAAABAXC5Am7+fGZ+5zXBGgXS6GUvmsXCLGc7tX7/rViXk3+eShZzaXnt75gUmT1I2f75zFn2hlAIDGKWf4g12KWcZxy81TniUOTjUsVlwPymXUXxESL/UfJKfbdstBhTOdy5EG9rYWA0K43SJmwPhH28BpoLfXXXXXGX/ilsXXXXXKgRLiJ2W19MzXHp8z3Lxw7r9wx3HaVlP4XiFv9U4hGcp8RMI1MP1nNesFlOBpG4pV2bJRBTXNXeY4l6F8WZ3C4kuf8XxOo08mXaTpvZ3T1841altmNTZCcPkXuMrBjYSJbA8npoXAXNwiivyoe3X2KMXXXXXdXXXXXXXXXXCXXXXX/ azureuser@myserver
```

Kopiera det här värdet så att du kan använda det i nästa övning.

### <a name="use-the-ssh-key-when-creating-a-linux-vm"></a>Använda SSH-nyckeln när du skapar en virtuell Linux-dator

Om du vill tillämpa SSH-nyckeln när du skapar en ny virtuell Linux-dator behöver du kopiera innehållet i den offentliga nyckeln och ange det i Azure Portal, _eller_ ange den offentliga nyckelfilen i Azure CLI- eller Azure PowerShell-kommandot. Vi använder den här metoden när vi skapar vår virtuella Linux-dator.

### <a name="add-the-ssh-key-to-an-existing-linux-vm"></a>Lägga till SSH-nyckeln på en befintlig virtuell Linux-dator

Om du redan har skapat en virtuell dator kan du installera den offentliga nyckeln på den virtuella Linux-datorn med kommandot `ssh-copy-id`. När nyckeln har auktoriserats för SSH beviljar den åtkomst till servern utan ett lösenord.

Skicka den offentliga nyckelfilen och användarnamnet som ska associeras med nyckeln.

```bash
ssh-copy-id -i ~/.ssh/id_rsa.pub azureuser@myserver
```
Nu när vi har vår offentliga nyckel går vi till Azure Portal och skapar en virtuell Linux-dator.


<span data-ttu-id="8c5b1-101">När du skapar en virtuell dator får den en offentlig IP-adress som kan nås via Internet och en privat IP-adress som används i Azure-datacentret.</span><span class="sxs-lookup"><span data-stu-id="8c5b1-101">When you create a virtual machine, it gets assigned a public IP address that is reachable over the Internet, and a private IP address used within the Azure data center.</span></span> <span data-ttu-id="8c5b1-102">Du får båda dessa värden när JSON-blocket returneras med kommandot `create`:</span><span class="sxs-lookup"><span data-stu-id="8c5b1-102">You get both of those values in the returning JSON block from the `create` command:</span></span>

```json
{
   ...
  "privateIpAddress": "10.0.0.4",
  "publicIpAddress": "40.83.165.85",
   ...
}
```

## <a name="connecting-to-the-vm-with-ssh"></a><span data-ttu-id="8c5b1-103">Ansluta till den virtuella datorn med SSH</span><span class="sxs-lookup"><span data-stu-id="8c5b1-103">Connecting to the VM with SSH</span></span>

<span data-ttu-id="8c5b1-104">Med den offentliga IP-adressen kan vi snabbt testa om en virtuell Linux-dator körs med hjälp av Secure Shell-verktyget (`ssh`).</span><span class="sxs-lookup"><span data-stu-id="8c5b1-104">With the public IP address we can quickly test that the Linux VM is up and running using the Secure Shell (`ssh`) tool.</span></span> <span data-ttu-id="8c5b1-105">Kom ihåg att vi angav `aldis` som administratörsnamn, så vi måste använda det nu.</span><span class="sxs-lookup"><span data-stu-id="8c5b1-105">Remember that we set our admin name to `aldis`, so we have to specify that.</span></span> <span data-ttu-id="8c5b1-106">Se till att använda den offentliga IP-adressen från _din_ instans som körs.</span><span class="sxs-lookup"><span data-stu-id="8c5b1-106">Make sure to use the public IP address from _your_ running instance.</span></span>

```azurecli
ssh <public-ip-address> -l aldis
```

> [!NOTE]
> <span data-ttu-id="8c5b1-107">Vi behöver inget lösenord eftersom vi genererade ett SSH-nyckelpar när vi skapade den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="8c5b1-107">We don't need a password because we generated an SSH key pair as part of the VM creation.</span></span> <span data-ttu-id="8c5b1-108">Första gången du ansluter till den virtuella datorn via shell uppmanas du att autentisera värden.</span><span class="sxs-lookup"><span data-stu-id="8c5b1-108">The first time you shell into the VM, it will give you a prompt about the authenticity of the host.</span></span> 
> 
> <span data-ttu-id="8c5b1-109">Det beror på att vi ansluter direkt till en IP-adress i stället för till ett värdnamn.</span><span class="sxs-lookup"><span data-stu-id="8c5b1-109">This is because we are hitting an IP address directly instead of a host name.</span></span> <span data-ttu-id="8c5b1-110">Om du svarar ”yes” sparas IP-adressen som en giltig värd för anslutning och anslutningen upprättas.</span><span class="sxs-lookup"><span data-stu-id="8c5b1-110">Answering "yes" will save the IP as a valid host for connection and allow the connection to proceed.</span></span>

```output
The authenticity of host '40.83.165.85 (40.83.165.85)' can't be established.
RSA key fingerprint is SHA256:hlFnTCAzgWVFiMxHK194I2ap6+5hZoj9ex8+/hoM7rE.
Are you sure you want to continue connecting (yes/no)? yes
Warning: Permanently added '40.83.165.85' (RSA) to the list of known hosts.
```

<span data-ttu-id="8c5b1-111">Sedan visas ett fjärrgränssnitt där du kan ange Linux-kommandon.</span><span class="sxs-lookup"><span data-stu-id="8c5b1-111">Then you'll be presented with a remote shell where you can enter Linux commands.</span></span>

```output
The programs included with the Debian GNU/Linux system are free software;
the exact distribution terms for each program are described in the
individual files in /usr/share/doc/*/copyright.

Debian GNU/Linux comes with ABSOLUTELY NO WARRANTY, to the extent
permitted by applicable law.
aldis@SampleVM:~$
```

<span data-ttu-id="8c5b1-112">Prova några kommandon som träning. När du är färdig loggar du ut från gränssnittet (skriv `logout` eller `exit` i gränssnittet).</span><span class="sxs-lookup"><span data-stu-id="8c5b1-112">Try a few commands as practice and when you are finished, sign out of the shell (type `logout` or `exit` in the shell).</span></span>
<span data-ttu-id="14263-101">När du skapar en virtuell dator får den en offentlig IP-adress som kan nås via internet och en privat IP-adress som används i Azure-datacentret.</span><span class="sxs-lookup"><span data-stu-id="14263-101">When you create a virtual machine, it gets assigned a public IP address that is reachable over the Internet, and a private IP address used within the Azure data center.</span></span> <span data-ttu-id="14263-102">Vi kan snabbt testa att en virtuell Linux-dator körs med verktyget `ssh`.</span><span class="sxs-lookup"><span data-stu-id="14263-102">We can quickly test that the Linux VM is up and running using the `ssh` tool.</span></span> <span data-ttu-id="14263-103">Kom ihåg att vi angav `aldis` som administratörsnamn, så vi måste använda det nu.</span><span class="sxs-lookup"><span data-stu-id="14263-103">Remember that we set our admin name to `aldis`, so we have to specify that.</span></span>

```azurecli
ssh 168.61.54.62 -l aldis
```

> [!NOTE]
> <span data-ttu-id="14263-104">Vi behöver inget lösenord eftersom vi genererade ett SSH-nyckelpar när vi skapade den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="14263-104">We don't need a password because we generated an SSH key pair as part of the VM creation.</span></span> <span data-ttu-id="14263-105">Första gången du ansluter till den virtuella datorn via shell uppmanas du att autentisera värden.</span><span class="sxs-lookup"><span data-stu-id="14263-105">The first time you shell into the VM, it will give you a prompt about the authenticity of the host.</span></span> 
> 
> <span data-ttu-id="14263-106">Det beror på att vi ansluter direkt till en IP-adress i stället för till ett värdnamn.</span><span class="sxs-lookup"><span data-stu-id="14263-106">This is because we are hitting an IP address directly instead of a host name.</span></span> <span data-ttu-id="14263-107">Om du svarar ”yes” sparas IP-adressen som en giltig värd för anslutning och anslutningen upprättas.</span><span class="sxs-lookup"><span data-stu-id="14263-107">Answering "yes" will save the IP as a valid host for connection and allow the connection to proceed.</span></span>

```output
The authenticity of host '168.61.54.62 (168.61.54.62)' can't be established.
RSA key fingerprint is SHA256:hlFnTCAzgWVFiMxHK194I2ap6+5hZoj9ex8+/hoM7rE.
Are you sure you want to continue connecting (yes/no)? yes
Warning: Permanently added '168.61.54.62' (RSA) to the list of known hosts.
```

<span data-ttu-id="14263-108">Sedan visas ett fjärr-shell där du kan ange Linux-kommandon.</span><span class="sxs-lookup"><span data-stu-id="14263-108">Then you'll be presented with a remote shell where you can enter Linux commands.</span></span>

```output
The programs included with the Debian GNU/Linux system are free software;
the exact distribution terms for each program are described in the
individual files in /usr/share/doc/*/copyright.

Debian GNU/Linux comes with ABSOLUTELY NO WARRANTY, to the extent
permitted by applicable law.
aldis@SampleVM:~$
```

<span data-ttu-id="14263-109">Prova några kommandon som träning. När du är färdig loggar du ut från kontot (skriv `logout` eller `exit` i shell).</span><span class="sxs-lookup"><span data-stu-id="14263-109">Try a few commands as practice and when you are finished, sign out of your account (type `logout` or `exit` in the shell).</span></span>
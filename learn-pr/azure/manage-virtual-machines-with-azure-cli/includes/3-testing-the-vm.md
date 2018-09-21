När du skapar en virtuell dator får den en offentlig IP-adress som kan nås via Internet och en privat IP-adress som används i Azure-datacentret. Du får båda dessa värden när JSON-blocket returneras med kommandot `create`:

```json
{
   ...
  "privateIpAddress": "10.0.0.4",
  "publicIpAddress": "40.83.165.85",
   ...
}
```

## <a name="connecting-to-the-vm-with-ssh"></a>Ansluta till den virtuella datorn med SSH

Med den offentliga IP-adressen kan vi snabbt testa om en virtuell Linux-dator körs med hjälp av Secure Shell-verktyget (`ssh`). Kom ihåg att vi angav `aldis` som administratörsnamn, så vi måste använda det nu. Se till att använda den offentliga IP-adressen från _din_ instans som körs.

```azurecli
ssh <public-ip-address> -l aldis
```

> [!NOTE]
> Vi behöver inget lösenord eftersom vi genererade ett SSH-nyckelpar när vi skapade den virtuella datorn. Första gången du ansluter till den virtuella datorn via shell uppmanas du att autentisera värden. 
> 
> Det beror på att vi ansluter direkt till en IP-adress i stället för till ett värdnamn. Om du svarar ”yes” sparas IP-adressen som en giltig värd för anslutning och anslutningen upprättas.

```output
The authenticity of host '40.83.165.85 (40.83.165.85)' can't be established.
RSA key fingerprint is SHA256:hlFnTCAzgWVFiMxHK194I2ap6+5hZoj9ex8+/hoM7rE.
Are you sure you want to continue connecting (yes/no)? yes
Warning: Permanently added '40.83.165.85' (RSA) to the list of known hosts.
```

Sedan visas ett fjärrgränssnitt där du kan ange Linux-kommandon.

```output
The programs included with the Debian GNU/Linux system are free software;
the exact distribution terms for each program are described in the
individual files in /usr/share/doc/*/copyright.

Debian GNU/Linux comes with ABSOLUTELY NO WARRANTY, to the extent
permitted by applicable law.
aldis@SampleVM:~$
```

Prova några kommandon som träning. När du är färdig loggar du ut från gränssnittet (skriv `logout` eller `exit` i gränssnittet).
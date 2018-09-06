**Data Science Virtual Machine** för Linux är en VM-avbildning som gör det enklare att komma igång med data science. Många verktyg har redan skapats, installerats och konfigurerats av oss för att du ska kunna komma igång så snabbt som möjligt. NVIDIA GPU-drivrutinen, NVIDIA CUDA och NVIDIA CUDA Deep Neural Network-biblioteket (cuDNN) ingår, liksom Jupyter och TensorFlow. Alla förinstallerade ramverk är GPU-kompatibla, men fungerar även bra tillsammans med vanliga processorer.

## <a name="what-is-covered-in-this-lab"></a>Vad går vi igenom i den här labbuppgiften?

 I den här labbuppgiften kommer du att göra följande:
* skapa en Data Science VM för Linux i Azure
* ansluta till din DSVM via fjärrskrivbordet
* träna upp en TensorFlow-modell att klassificera bilder som innehåller varmkorvar och bilder som INTE innehåller varmkorvar
* använda modellen i en Python-app.

För att kunna göra den här labbuppgiften behöver du en Azure-prenumeration och fjärrskrivbordsklienten Xfce. Mer information finns i avsnittet om förutsättningar. Om du inte har en Azure-prenumeration kan du skapa ett [kostnadsfritt konto](https://azure.microsoft.com/en-us/free/?WT.mc_id=A261C142F) innan du börjar.

### <a name="prerequisites-for-the-lab"></a>Förutsättningar för labbuppgiften

 1. **Microsoft Azure-konto**: du behöver ett giltigt och aktivt Azure-konto för den här labbuppgiften. Om du inte har ett sådant kan du registrera dig för en [kostnadsfri provperiod](https://azure.microsoft.com/en-us/free/)

    * Om du har en aktiv prenumeration på Visual Studio har du en kredit på 50–150 USD per månad. Om du följer [den här länken](https://azure.microsoft.com/en-us/pricing/member-offers/msdn-benefits-details/) kan du läsa mer om att aktivera och börja använda din månatliga Azure-kredit.

    * Om du inte prenumererar på Visual Studio kan du registrera dig för [Visual Studio Dev Essentials](https://www.visualstudio.com/dev-essentials/) utan kostnad och skapa ett **kostnadsfritt Azure-konto** (gäller för 1 års kostnadsfria tjänster, 200 USD för första månaden).

    * Om du är student kan du registrera dig för [Azure for Students](https://aka.ms/azure4students) utan kostnad och få 100 USD i Azure-krediter + ett års kostnadsfria tjänster, och du behöver inget kreditkort. 

1. En [Xfce](https://xfce.org/)-skrivbordsklient som [X2Go](https://wiki.x2go.org/doku.php/download:start)
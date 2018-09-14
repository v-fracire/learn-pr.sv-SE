Den **Data Science Virtual Machine** för Linux är en avbildning av virtuell dator som förenklar Kom igång med data science. Flera olika verktyg är redan skapats, installeras och konfigureras för att komma igång snabbt. NVIDIA GPU-drivrutinen, NVIDIA CUDA och NVIDIA CUDA Deep Neural Network-biblioteket (cuDNN) ingår, liksom Jupyter och TensorFlow. Alla förinstallerade ramverk är GPU-kompatibla, men fungerar även bra tillsammans med vanliga processorer.

## <a name="learning-objectives"></a>Utbildningsmål

I den här modulen kommer du att göra följande:

- skapa en Data Science VM för Linux i Azure.
- ansluta till din DSVM via fjärrskrivbordet.
- träna upp en TensorFlow-modell att klassificera bilder som innehåller varmkorvar och bilder som INTE innehåller varmkorvar.
- använda modellen i en Python-app.

### <a name="prerequisites"></a>Förutsättningar
<!---TODO: This is really long, need to make more concise and also add to index.yml--->
<!---TODO: Update for free sandbox.--->

För att slutföra den här modulen behöver du en Azure-prenumeration och en Xfce remote desktop-klient.

 1. **Microsoft Azure-konto**: du behöver ett giltigt och aktivt Azure-konto för den här modulen. Om du inte har ett sådant kan du registrera dig för en [kostnadsfri provperiod](https://azure.microsoft.com/free/)

    * Om du har en aktiv prenumeration på Visual Studio har du en kredit på 50–150 USD per månad. Om du följer [den här länken](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) kan du läsa mer om att aktivera och börja använda din månatliga Azure-kredit.

    * Om du inte prenumererar på Visual Studio kan du registrera dig för [Visual Studio Dev Essentials](https://www.visualstudio.com/dev-essentials/) utan kostnad och skapa ett **kostnadsfritt Azure-konto** (gäller för ett års kostnadsfria tjänster, 200 USD för den första månaden).

    * Om du är student kan du registrera dig för [Azure for Students](https://aka.ms/azure4students) utan kostnad och få 100 USD i Azure-krediter plus ett års kostnadsfria tjänster, och du behöver inget kreditkort. 

1. En [Xfce](https://xfce.org/)-skrivbordsklient som [X2Go](https://wiki.x2go.org/doku.php/download:start)
Det finns flera verktyg som skapar ett lagringskonto. Vanligtvis väljer du baserat på om du vill ha ett grafiskt användargränssnitt och huruvida det behövs automatisering.

## <a name="available-tools"></a>Tillgängliga verktyg

De tillgängliga verktygen är:

- Portalen
- Kommandoradsgränssnittet (CLI)
- Azure PowerShell
- Klientbibliotek för hantering

Portalen har ett grafiskt användargränssnitt med knappbeskrivningar för varje inställning. Detta gör den enkel att använda och användbar för att lära dig mer om alternativen.

Alla de andra verktygen stödjer automatisering. Med CLI och Azure PowerShell kan du skriva skript medan hanteringsbiblioteken gör det möjligt att införliva det du skapar i en klientapp.

## <a name="how-to-choose-a-tool"></a>Så väljer du verktyg

Lagringskonton baseras vanligtvis på en analys av dina data, så de brukar vara relativt stabila. Det innebär att skapande av lagringskonton vanligtvis är en engångsåtgärd som görs i början av ett projekt. Portalen är det vanligaste valet för engångsaktiviteter.

I sällsynta fall där du behöver automatisering ligger beslutet mellan ett programmatiskt API eller en skriptlösning. Skript går normalt snabbare att skapa och kräver mindre arbete för att underhålla eftersom det inte finns något behov av en IDE, NuGet-paket eller kompileringssteg. Om du har ett befintlig klientprogram kan hanteringsbiblioteken vara ett bra val; i annat fall är skript troligen ett bättre alternativ.
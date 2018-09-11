Som programvaruutvecklare vill du så klart spara så mycket tid som möjligt när du skriver kod. När du ska interagera med Azure Storage kan du använda dess REST-slutpunkt, men det kan kräva en hel del arbete. Det här problemet har lett till utvecklingen av flera bibliotek som gör det enklare att ansluta till och använda Azure Storage. Dessa kallas ofta för *klientbibliotek* och är normalt det enklaste sättet att interagera med Azure Storage. 

Här får du lära dig att lägga till ett lämpligt klientbibliotek som referens i din .NET-app.

## <a name="application-programming-interface-api"></a>Application Programming Interface (API)

Azure Storage exponerar tjänster via en uppsättning API:er på internet med hjälp av REST-protokoll (Representational State Transfer). Du hittar fullständig dokumentation för dessa API:er i [referensen till Azure Storage Services REST-API:er](https://docs.microsoft.com/en-us/rest/api/storageservices/). Även om du kan interagera direkt med dessa API:er hanteras åtkomsten till Azure Storage normalt med hjälp av klientbibliotek. Apputvecklare kan spara mycket tid med de här klientbiblioteken eftersom API:t har genomgått utförliga tester.

## <a name="language-support"></a>Språkstöd

Det finns ett antal klientbibliotek för åtkomst till Azure Storage som har stöd för ett antal olika språk. På [sidan med dokumentation om SDK-verktyg](https://docs.microsoft.com/en-us/azure/#pivot=sdkstools) finns länkar till klientbibliotek eller SDK:er för alla språk som stöds för tillfället. De är bland annat .NET, Java, Python, Node.js och Go.

## <a name="summary"></a>Sammanfattning

Azure Storage har en REST-slutpunkt för interaktion med alla tjänster som ingår. Även om du kan använda den är det vanligare att använda ett klientbibliotek. Azure Storage har stöd för ett antal språk som .NET, Java, Python och Node.js.



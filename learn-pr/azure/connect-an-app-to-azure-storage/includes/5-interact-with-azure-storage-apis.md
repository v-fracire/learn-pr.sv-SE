Azure Storage tillhandahåller en REST-baserad webb-API för de containrar och data som lagras på varje konto. Det finns fristående API:er som kan arbeta med varje typ av data som du kan lagra. Du kanske kommer ihåg att vi har fyra specifika datatyper:

- **Blobbar** för ostrukturerade data, till exempel binära filer och textfiler.
- **Köer** för beständiga meddelandefunktioner.
- **Tabeller** för strukturerad lagring av nyckel/värden.
- **Filer** för traditionella SMB-filresurser.

## <a name="using-the-rest-api"></a>Använda REST API

REST API:er i Storage är tillgängliga från tjänster som körs i Azure via ett virtuellt nätverk, eller via Internet från program som kan skicka en HTTP/HTTPS-begäran och få ett HTTP/HTTPS-svar.

Om du vill visa alla blobbar i en container skulle du till exempel skicka något som liknar:

```http
GET https://[url-for-service-account]/?comp=list&include=metadata
```

Detta returnerar ett XML-block med data som är specifika för kontot:

```xml
<?xml version="1.0" encoding="utf-8"?>  
<EnumerationResults AccountName="https://[url-for-service-account]/">  
  <Containers>  
    <Container>  
      <Name>container1</Name>  
      <Url>https://[url-for-service-account]/container1</Url>  
      <Properties>  
        <Last-Modified>Sun, 24 Sep 2018 18:09:03 GMT</Last-Modified>  
        <Etag>0x8CAE7D0C4AF4487</Etag>  
      </Properties>  
      <Metadata>  
        <Color>orange</Color>  
        <ContainerNumber>01</ContainerNumber>  
        <SomeMetadataName>SomeMetadataValue</SomeMetadataName>  
      </Metadata>  
    </Container>  
    <Container>  
      <Name>container2</Name>  
      <Url>https://[url-for-service-account]/container2</Url>  
      <Properties>  
        <Last-Modified>Sun, 24 Sep 2018 17:26:40 GMT</Last-Modified>  
        <Etag>0x8CAE7CAD8C24928</Etag>  
      </Properties>  
      <Metadata>  
        <Color>pink</Color>  
        <ContainerNumber>02</ContainerNumber>  
        <SomeMetadataName>SomeMetadataValue</SomeMetadataName>  
      </Metadata>  
    </Container>  
    <Container>  
      <Name>container3</Name>  
      <Url>https://[url-for-service-account]/container3</Url>  
      <Properties>  
        <Last-Modified>Sun, 24 Sep 2018 17:26:40 GMT</Last-Modified>  
        <Etag>0x8CAE7CAD8EAC0BB</Etag>  
      </Properties>  
      <Metadata>  
        <Color>brown</Color>  
        <ContainerNumber>03</ContainerNumber>  
        <SomeMetadataName>SomeMetadataValue</SomeMetadataName>  
      </Metadata>  
    </Container>  
  </Containers>  
  <NextMarker>container4</NextMarker>  
</EnumerationResults>  
```

Den här metoden kräver dock en hel del manuell parsning och att HTTP-paket skapas som kan arbeta med varje API. Därför erbjuder Azure färdiga _klientbibliotek_ som gör det lättare att arbeta med tjänsten för vanliga språk och ramverk.

## <a name="using-a-client-library"></a>Använda ett klientbibliotek

Klientbibliotek kan spara mycket arbete för programutvecklarna, eftersom API:n har testats. Det ger ofta bättre omslutningar runt datamodeller som skickas och tas emot av REST API:n.

:::row:::
    :::column:::
        Microsoft har Azure-klientbibliotek som stöder ett antal språk och ramverk, till exempel: – .NET – Java – Python – Node.js – Go :::column-end:::: :::column:::
        <br> ![Exempel på logotyper av ramverk som stöds och som du kan använda med Azure](../media/4-common-tools.png)
    :::column-end:::
:::row-end:::

Vi kan till exempel använda följande kodfragment till att hämta samma lista med blobbar i C#:

```csharp
CloudBlobDirectory directory = ...;
foreach (IEnumerable<IListBlobItem> blob in directory.ListBlobs(
                useFlatBlobListing: true,
                blobListingDetails: BlobListingDetails.All))
{
    // Work with blob item .. could be page blob, block blob, etc.
}
```

Eller i JavaScript:

```javascript
const containerName = "...";
const blobService = storage.createBlobService();

blobService.listBlobsSegmented(containerName, null, function (error, results) {
    if (results) {
        for (var i = 0, blob; blob = results.entries[i]; i++) {
            // Work with blob item .. could be page blob, block blob, etc.
        }
    }
});
```

> [!NOTE]
> Klientbiblioteken är bara tunna _omslutningar_ över REST API:n. De gör exakt vad du skulle göra om du använde webbtjänsterna direkt. Dessa bibliotek består även av öppen källkod, vilket gör dem mycket transparenta. Leta efter dem på GitHub.

Nu ska vi lägga till klientbiblioteksstöd för vårt program.
Azure Storage tillhandahåller ett webb-baserade Representational State Transfer (REST) API att arbeta med behållare och data som lagras i varje konto. Det finns oberoende API: er kan arbeta med varje typ av data som du kan lagra. Kom ihåg att vi har fyra specifika datatyper:

- **Blobbar** för Ostrukturerade data, till exempel binära och textfiler.
- **Köer** för beständig meddelandefunktion.
- **Tabeller** för strukturerade lagringen av nyckel/värde.
- **Filer** för traditionella SMB-filresurser.

## <a name="using-the-rest-api"></a>Med hjälp av REST-API

Storage REST API: er är tillgängliga från tjänster som körs i Azure via ett virtuellt nätverk och över Internet från alla program som kan skicka en HTTP/HTTPS-begäran och får en HTTP/HTTPS-svar.

Om du vill visa alla blobbar i en behållare, skulle du till exempel skicka något som liknar:

```http
GET https://[url-for-service-account]/?comp=list&include=metadata
```

Detta returnerar ett XML-block med data specifika till kontot:

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

Den här metoden kräver dock en mängd manuella parsning och skapandet av HTTP-paket för att arbeta med varje API. Därför erbjuder Azure färdiga _klientbibliotek_ som gör att arbeta med tjänsten enklare för vanligt språk och ramverk.

## <a name="using-a-client-library"></a>Använder ett klientbibliotek

Klientbibliotek kan spara mycket arbete för programutvecklare eftersom API: et har testats och ger ofta nicer omslutningar runt datamodeller skickas och tas emot av REST-API: et.

:::row:::
    :::column:::
        Microsoft har Azure-klientbiblioteken som stöder ett antal språk och ramverk, till exempel: - .NET - Java - Python - Node.js – Go :::column-end:::: :::column:::
        <br> ![Exemplet logotyper av ramverk som stöds och som du kan använda med Azure](../media/4-common-tools.png)
    :::column-end:::
:::row-end:::

Vi kan till exempel använda följande kodavsnitt för att hämta samma lista över blobbar i C#:

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
> Klientbiblioteken är bara tunna _omslutningar_ REST-API: n. De gör exakt vad du ska göra om du använde webbtjänsterna direkt. Dessa bibliotek finns även öppen källkod, vilket gör dem mycket transparent. Leta efter dem på GitHub.

Vi lägger till biblioteket klientstöd till vårt program.
<span data-ttu-id="79fec-101">Azure Storage tillhandahåller en REST API för de containrar och data som lagras på varje konto.</span><span class="sxs-lookup"><span data-stu-id="79fec-101">Azure Storage provides a REST API to work with the containers and data stored in each account.</span></span> <span data-ttu-id="79fec-102">Det finns fristående API:er som kan arbeta med varje typ av data som du kan lagra.</span><span class="sxs-lookup"><span data-stu-id="79fec-102">There are independent APIs available to work with each type of data you can store.</span></span> <span data-ttu-id="79fec-103">Du kanske kommer ihåg att vi har fyra specifika datatyper:</span><span class="sxs-lookup"><span data-stu-id="79fec-103">Recall that we have four specific data types:</span></span>

- <span data-ttu-id="79fec-104">**Blobbar** för ostrukturerade data, till exempel binära filer och textfiler.</span><span class="sxs-lookup"><span data-stu-id="79fec-104">**Blobs** for unstructured data such as binary and text files.</span></span>
- <span data-ttu-id="79fec-105">**Köer** för beständiga meddelandefunktioner.</span><span class="sxs-lookup"><span data-stu-id="79fec-105">**Queues** for persistent messaging.</span></span>
- <span data-ttu-id="79fec-106">**Tabeller** för strukturerad lagring av nyckel/värden.</span><span class="sxs-lookup"><span data-stu-id="79fec-106">**Tables** for structured storage of key/values.</span></span>
- <span data-ttu-id="79fec-107">**Filer** för traditionella SMB-filresurser.</span><span class="sxs-lookup"><span data-stu-id="79fec-107">**Files** for traditional SMB file shares.</span></span>

## <a name="using-the-rest-api"></a><span data-ttu-id="79fec-108">Använda REST API</span><span class="sxs-lookup"><span data-stu-id="79fec-108">Using the REST API</span></span>

<span data-ttu-id="79fec-109">REST API:er i Storage är tillgängliga från valfri plats via Internet, från program som kan skicka en HTTP/HTTPS-begäran och få ett HTTP/HTTPS-svar.</span><span class="sxs-lookup"><span data-stu-id="79fec-109">The Storage REST APIs are accessible from anywhere on the Internet, by any application that can send an HTTP/HTTPS request and receive an HTTP/HTTPS response.</span></span>

<span data-ttu-id="79fec-110">Om du vill visa alla blobbar i en container skulle du till exempel skicka något som liknar:</span><span class="sxs-lookup"><span data-stu-id="79fec-110">For example, if you wanted to list all the blobs in a container, you would send something like:</span></span>

```http
GET https://[url-for-service-account]/?comp=list&include=metadata
```

<span data-ttu-id="79fec-111">Detta returnerar ett XML-block med data som är specifika för kontot:</span><span class="sxs-lookup"><span data-stu-id="79fec-111">This would return an XML block with data specific to the account:</span></span>

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

<span data-ttu-id="79fec-112">Den här metoden kräver dock en hel del manuell parsning och att HTTP-paket skapas som kan arbeta med varje API.</span><span class="sxs-lookup"><span data-stu-id="79fec-112">However, this approach requires a lot of manual parsing and the creation of HTTP packets to work with each API.</span></span> <span data-ttu-id="79fec-113">Därför erbjuder Azure färdiga _klientbibliotek_ som gör det lättare att arbeta med tjänsten för vanliga språk och ramverk.</span><span class="sxs-lookup"><span data-stu-id="79fec-113">For this reason, Azure provides pre-built _client libraries_ that make working with the service easier for common languages and frameworks.</span></span>

## <a name="using-a-client-library"></a><span data-ttu-id="79fec-114">Använda ett klientbibliotek</span><span class="sxs-lookup"><span data-stu-id="79fec-114">Using a client library</span></span>

<span data-ttu-id="79fec-115">Klientbibliotek kan spara mycket arbete för programutvecklarna, eftersom API:n har testats. Det ger ofta bättre omslutningar runt datamodeller som skickas och tas emot av REST API:n.</span><span class="sxs-lookup"><span data-stu-id="79fec-115">Client libraries can save a significant amount of work for application developers because the API is tested and it often provides nicer wrappers around the data models sent and received by the REST API.</span></span>

:::row:::
    :::column:::
        <span data-ttu-id="79fec-116">Microsoft har Azure-klientbibliotek som stöder ett antal språk och ramverk, till exempel: – .NET – Java – Python – Node.js – Go :::column-end::::</span><span class="sxs-lookup"><span data-stu-id="79fec-116">Microsoft has Azure client libraries that support a number of languages and frameworks including: - .NET - Java - Python - Node.js - Go :::column-end::::</span></span> :::column:::
        <br> <span data-ttu-id="79fec-117">![Exempel på logotyper av ramverk som stöds och som du kan använda med Azure](../media/4-common-tools.png)</span><span class="sxs-lookup"><span data-stu-id="79fec-117">![Sample logos of supported frameworks you can use with Azure](../media/4-common-tools.png)</span></span> 
    :::column-end:::
:::row-end:::

<span data-ttu-id="79fec-118">Vi kan till exempel använda följande kodfragment till att hämta samma lista med blobbar i C#:</span><span class="sxs-lookup"><span data-stu-id="79fec-118">For example, to retrieve the same list of blobs in C#, we could use the following code snippet:</span></span>

```csharp
CloudBlobDirectory directory = ...;
foreach (IEnumerable<IListBlobItem> blob in directory.ListBlobs(
                useFlatBlobListing: true,
                blobListingDetails: BlobListingDetails.All))
{
    // Work with blob item .. could be page blob, block blob, etc.
}
```

<span data-ttu-id="79fec-119">Eller i JavaScript:</span><span class="sxs-lookup"><span data-stu-id="79fec-119">Or in JavaScript:</span></span>

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
> <span data-ttu-id="79fec-120">Klientbiblioteken är bara tunna _omslutningar_ över REST API:n.</span><span class="sxs-lookup"><span data-stu-id="79fec-120">The client libraries are just thin _wrappers_ over the REST API.</span></span> <span data-ttu-id="79fec-121">De gör exakt vad du skulle göra om du använde webbtjänsterna direkt.</span><span class="sxs-lookup"><span data-stu-id="79fec-121">They are doing exactly what you would do if you used the web services directly.</span></span> <span data-ttu-id="79fec-122">Dessa bibliotek består även av öppen källkod, vilket gör dem mycket transparenta.</span><span class="sxs-lookup"><span data-stu-id="79fec-122">These libraries are also open source, making them very transparent.</span></span> <span data-ttu-id="79fec-123">Leta efter dem på GitHub.</span><span class="sxs-lookup"><span data-stu-id="79fec-123">Look for them on GitHub.</span></span>

<span data-ttu-id="79fec-124">Nu ska vi lägga till klientbiblioteksstöd i vårt program.</span><span class="sxs-lookup"><span data-stu-id="79fec-124">Let's add client library support to our application.</span></span>
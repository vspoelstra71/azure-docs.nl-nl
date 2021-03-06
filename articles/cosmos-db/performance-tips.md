---
title: Tips voor Azure Cosmos DB prestaties voor .NET
description: Meer informatie over client configuratie opties voor het verbeteren van de prestaties van Azure Cosmos DB.
author: SnehaGunda
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 01/15/2020
ms.author: sngun
ms.openlocfilehash: ee2743af2f8499aec04d8b6b733e1ba4c2a82083
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/28/2020
ms.locfileid: "80546079"
---
# <a name="performance-tips-for-azure-cosmos-db-and-net"></a>Tips voor betere prestaties van Azure Cosmos DB en .NET

> [!div class="op_single_selector"]
> * [Async Java](performance-tips-async-java.md)
> * [Java](performance-tips-java.md)
> * [.NET](performance-tips.md)
> 

Azure Cosmos DB is een snelle en flexibele gedistribueerde data base die naadloos kan worden geschaald met gegarandeerde latentie en door voer. U hoeft geen grote architectuur wijzigingen aan te brengen of complexe code te schrijven om uw data base te schalen met Azure Cosmos DB. Omhoog en omlaag schalen is net zo eenvoudig als het maken van één API-aanroep. Zie voor meer informatie [over het inrichten van container doorvoer](how-to-provision-container-throughput.md) of [het inrichten van de doorvoer capaciteit van de data base](how-to-provision-database-throughput.md). Maar omdat Azure Cosmos DB via netwerk aanroepen wordt geopend, zijn er optimalisaties aan de client zijde die u kunt uitvoeren om piek prestaties te bereiken wanneer u de [SQL .NET SDK](sql-api-sdk-dotnet-standard.md)gebruikt.

Als u de prestaties van uw Data Base wilt verbeteren, kunt u de volgende opties overwegen:

## <a name="hosting-recommendations"></a>Aanbevelingen hosten

**Gebruik Windows 64-bits in plaats van Linux of Windows 32-bits host processing voor query-intensieve werk belastingen.**

U wordt aangeraden Windows 64-bits host te verwerken voor betere prestaties. De SQL SDK bevat een systeem eigen ServiceInterop. dll om query's lokaal te parseren en te optimaliseren. ServiceInterop. dll wordt alleen ondersteund op het Windows x64-platform. Voor Linux en andere niet-ondersteunde platforms waarbij ServiceInterop. dll niet beschikbaar is, wordt er een extra netwerk aanroep naar de gateway verzonden om de geoptimaliseerde query te krijgen. De volgende typen toepassingen gebruiken standaard 32-bits host verwerking. Voer de volgende stappen uit op basis van het type van uw toepassing om de verwerking van de host te wijzigen in 64-bits verwerking:

- Voor uitvoer bare toepassingen kunt u de verwerking van een host wijzigen door het [platform doel](https://docs.microsoft.com/visualstudio/ide/how-to-configure-projects-to-target-platforms?view=vs-2019) in te stellen op **x64** in het venster **project eigenschappen** op het tabblad **opbouwen** .

- Voor VSTest test projecten kunt u de verwerking van de host wijzigen door test **Test** > **instellingen** > testen**standaard processor architectuur als x64** te selecteren in het menu van Visual Studio **testen** .

- Voor lokaal geïmplementeerde ASP.NET-webtoepassingen kunt u de verwerking van de host wijzigen door **de 64-bits versie van IIS Express voor websites en projecten** te selecteren onder **extra** > **Opties** > **projecten en oplossingen** > **webprojecten**.

- Voor ASP.NET-webtoepassingen die in azure zijn geïmplementeerd, kunt u de verwerking van de host wijzigen door het **64-bits** platform te selecteren in **Toepassings instellingen** in de Azure Portal.

> [!NOTE] 
> Nieuwe Visual Studio-projecten worden standaard ingesteld op **elke CPU**. We raden u aan om uw project in te stellen op **x64** zodat het niet wordt overgeschakeld naar **x86**. Een project ingesteld op **een wille keurige CPU** kan eenvoudig overschakelen naar **x86** als er een alleen-x86-afhankelijkheid is toegevoegd.<br/>
> ServiceInterop. dll moet zich in de map bevindt waarin de SDK-DLL wordt uitgevoerd. Dit is alleen een probleem als u hand matig Dll's kopieert of aangepaste build/Deployment-systemen hebt.
    
**Garbage Collection aan de server zijde inschakelen (GC)**

Het verminderen van de frequentie van garbagecollection kan in sommige gevallen helpen. Stel in .NET [gcServer](https://msdn.microsoft.com/library/ms229357.aspx) in op `true`.

**De werk belasting van uw client uitschalen**

Als u test met hoge doorvoer niveaus (meer dan 50.000 RU/s), kan de client toepassing het knel punt worden als gevolg van het uitvallen van CPU-of netwerk gebruik. Als u dit punt bereikt, kunt u het Azure Cosmos DB-account nog verder pushen door uw client toepassingen op meerdere servers te schalen.

> [!NOTE] 
> Hoog CPU-gebruik kan leiden tot grotere latentie en time-outuitzonderingen voor aanvragen.

## <a name="networking"></a>Netwerken
<a id="direct-connection"></a>

**Verbindings beleid: modus directe verbinding gebruiken**

Hoe een client verbinding maakt met Azure Cosmos DB heeft belang rijke gevolgen voor de prestaties, met name voor de waargenomen latentie aan de client zijde. Er zijn twee belang rijke configuratie-instellingen beschikbaar voor het configureren van beleid voor client verbindingen: de verbindings *modus* en het verbindings *protocol*.  De twee beschik bare modi zijn:

   * Gateway modus
      
     De gateway modus wordt op alle SDK-platforms ondersteund en is de geconfigureerde standaard instelling voor de [micro soft. Azure. DocumentDB-SDK](sql-api-sdk-dotnet.md). Als uw toepassing wordt uitgevoerd in een bedrijfs netwerk met strikte firewall beperkingen, is de gateway modus de beste keuze, omdat deze gebruikmaakt van de standaard HTTPS-poort en een enkel eind punt. De verhoudingen van de prestaties zijn echter wel dat de gateway modus een extra netwerk-hop omvat telkens wanneer gegevens worden gelezen vanuit of geschreven naar Azure Cosmos DB. De directe modus biedt dus betere prestaties omdat er minder netwerk-hops zijn. We raden ook de verbindings modus voor de gateway aan wanneer u toepassingen uitvoert in omgevingen met een beperkt aantal socket verbindingen.

     Wanneer u de SDK in Azure Functions gebruikt, met name in het [verbruiks abonnement](../azure-functions/functions-scale.md#consumption-plan), moet u rekening houden met de huidige [limieten voor verbindingen](../azure-functions/manage-connections.md). In dat geval kan de gateway modus beter zijn als u ook met andere op HTTP gebaseerde clients in uw Azure Functions-toepassing werkt.

   * Directe modus

     Directe modus ondersteunt connectiviteit via TCP-protocol en is de standaard connectiviteits modus als u gebruikmaakt van de [SDK van micro soft. Azure. Cosmos/. net v3](sql-api-sdk-dotnet-standard.md).

In de gateway modus maakt Azure Cosmos DB gebruik van poort 443 en poorten 10250, 10255 en 10256 wanneer u de Azure Cosmos DB-API voor MongoDB gebruikt. Poort 10250 wordt toegewezen aan een standaard MongoDB-instantie zonder geo-replicatie. Poorten 10255 en 10256 worden toegewezen aan het MongoDB-exemplaar met geo-replicatie.
     
Wanneer u TCP in de directe modus gebruikt, moet u, naast de gateway poorten, ervoor zorgen dat het poort bereik tussen 10000 en 20000 open is, omdat Azure Cosmos DB dynamische TCP-poorten gebruikt. Als deze poorten niet zijn geopend en u TCP probeert te gebruiken, ontvangt u een fout bericht van de 503-Service die niet beschikbaar is. In deze tabel ziet u de connectiviteits modi die beschikbaar zijn voor verschillende Api's en de service poorten die worden gebruikt voor elke API:

|Verbindings modus  |Ondersteund protocol  |Ondersteunde Sdk's  |API/service poort  |
|---------|---------|---------|---------|
|Gateway  |   HTTPS    |  Alle Sdk's    |   SQL (443), MongoDB (10250, 10255, 10256), Table (443), Cassandra (10350), Graph (443)    |
|Direct    |     TCP    |  .NET SDK    | Poorten in het 10000 tot en met 20000-bereik |

Azure Cosmos DB biedt een eenvoudig open, REST-programmeer model via HTTPS. Daarnaast biedt het een efficiënt TCP-protocol, dat ook wordt doorzocht in het communicatie model en dat beschikbaar is via de .NET-client-SDK. TCP-protocol gebruikt TLS voor initiële verificatie en het versleutelen van verkeer. Gebruik, indien mogelijk, het TCP-protocol voor de beste prestaties.

Voor SDK v3 configureert u de verbindings modus wanneer u de `CosmosClient` instantie maakt, in. `CosmosClientOptions` Houd er rekening mee dat de directe modus de standaard instelling is.

```csharp
var serviceEndpoint = new Uri("https://contoso.documents.net");
var authKey = "your authKey from the Azure portal";
CosmosClient client = new CosmosClient(serviceEndpoint, authKey,
new CosmosClientOptions
{
    ConnectionMode = ConnectionMode.Gateway // ConnectionMode.Direct is the default
});
```

Voor de micro soft. Azure. DocumentDB SDK configureert u de verbindings modus tijdens het bouwen van het `DocumentClient` exemplaar met behulp `ConnectionPolicy` van de para meter. Als u de directe modus gebruikt, kunt u deze `Protocol` ook instellen met behulp van de `ConnectionPolicy` para meter.

```csharp
var serviceEndpoint = new Uri("https://contoso.documents.net");
var authKey = "your authKey from the Azure portal";
DocumentClient client = new DocumentClient(serviceEndpoint, authKey,
new ConnectionPolicy
{
   ConnectionMode = ConnectionMode.Direct, // ConnectionMode.Gateway is the default
   ConnectionProtocol = Protocol.Tcp
});
```

Omdat TCP alleen wordt ondersteund in de directe modus, wordt het HTTPS-protocol altijd gebruikt voor communicatie met de gateway en wordt de `Protocol` waarde in `ConnectionPolicy` genegeerd als u gebruikmaakt van de modus gateway.

![Het Azure Cosmos DB verbindings beleid](./media/performance-tips/connection-policy.png)

**Open async aanroepen om opstart latentie bij eerste aanvraag te voor komen**

De eerste aanvraag heeft standaard een hogere latentie omdat de routerings tabel van het adres moet worden opgehaald. Wanneer u [SDK v2](sql-api-sdk-dotnet.md)gebruikt, roept `OpenAsync()` u eenmaal aan tijdens de initialisatie om te voor komen dat deze opstart latentie bij de eerste aanvraag:

    await client.OpenAsync();

> [!NOTE] 
> `OpenAsync`genereert aanvragen voor het verkrijgen van de routerings tabel van het adres voor alle containers in het account. Voor accounts met veel containers, maar waarvan de toepassing toegang heeft tot een subset hiervan `OpenAsync` , zou een onnodige hoeveelheid verkeer genereren, waardoor de initialisatie traag wordt. Het gebruik `OpenAsync` hiervan is mogelijk niet nuttig in dit scenario, omdat het opstarten van toepassingen wordt vertraagd.

   <a id="same-region"></a>
**Voor prestaties, termijnen-clients in dezelfde Azure-regio**

Als dat mogelijk is, plaatst u toepassingen die Azure Cosmos DB aanroepen in dezelfde regio als de Azure Cosmos DB-Data Base. Hier volgt een vergelijking van benadering: aanroepen naar Azure Cosmos DB binnen dezelfde regio worden binnen 1 MS tot 2 MS voltooid, maar de latentie tussen de West-en Oost kust van de VS is meer dan 50 MS. Deze latentie kan variëren van aanvraag tot aanvraag, afhankelijk van de route die door de aanvraag wordt genomen, terwijl deze van de client wordt door gegeven aan de grens van het Azure-Data Center. U kunt de laagst mogelijke latentie verkrijgen door ervoor te zorgen dat de aanroepende toepassing zich in dezelfde Azure-regio bevindt als het ingerichte Azure Cosmos DB-eind punt. Zie [Azure-regio's](https://azure.microsoft.com/regions/#services)voor een lijst met beschik bare regio's.

![Het Azure Cosmos DB verbindings beleid](./media/performance-tips/same-region.png)<a id="increase-threads"></a>

**Het aantal threads/taken verhogen**

Omdat aanroepen naar Azure Cosmos DB via het netwerk worden gemaakt, moet u mogelijk de mate van parallelle uitvoering van uw aanvragen variëren, zodat de client toepassing mini maal tijd nodig heeft om te wachten tussen aanvragen. Als u bijvoorbeeld de [parallelle bibliotheek](https://msdn.microsoft.com//library/dd460717.aspx)van .net-taak gebruikt, maakt u op volg orde van honderden taken die worden gelezen van of schrijven naar Azure Cosmos db.

**Versneld netwerken inschakelen**
 
 Om latentie en CPU-jitter te verminderen, raden we u aan om versneld netwerken op virtuele client machines in te scha kelen. Zie [een virtuele Windows-machine maken met versneld netwerken](../virtual-network/create-vm-accelerated-networking-powershell.md) of [een virtuele Linux-machine maken met versneld netwerken](../virtual-network/create-vm-accelerated-networking-cli.md).

## <a name="sdk-usage"></a>SDK-gebruik
**De meest recente SDK installeren**

De Azure Cosmos DB Sdk's worden voortdurend verbeterd om de beste prestaties te leveren. Raadpleeg de [Azure Cosmos DB SDK](sql-api-sdk-dotnet-standard.md) -pagina's om de meest recente SDK te bepalen en verbeteringen te bekijken.

**Stream-Api's gebruiken**

[.NET SDK v3](sql-api-sdk-dotnet-standard.md) bevat stream-api's die gegevens kunnen ontvangen en retour neren zonder serialisatie. 

Middle-tier-toepassingen die geen reacties rechtstreeks van de SDK gebruiken, maar door sturen naar andere toepassings lagen, kunnen profiteren van de stream-Api's. Zie de voor beelden van [artikel beheer](https://github.com/Azure/azure-cosmos-dotnet-v3/blob/master/Microsoft.Azure.Cosmos.Samples/Usage/ItemManagement) voor voor beeld van stroom verwerking.

**Een singleton Azure Cosmos DB-client gebruiken voor de levens duur van uw toepassing**

Elk `DocumentClient` en `CosmosClient` -exemplaar is thread-safe en voert efficiënt verbindings beheer en adres caching uit wanneer deze in de directe modus worden uitgevoerd. Om efficiënt verbindings beheer en betere prestaties van de SDK-client mogelijk te maken, wordt u aangeraden `AppDomain` één exemplaar te gebruiken per voor de levens duur van de toepassing.

   <a id="max-connection"></a>

**System.Net MaxConnections per host verg Roten met de gateway modus**

Azure Cosmos DB aanvragen worden gedaan via HTTPS/REST wanneer u de gateway modus gebruikt. Deze worden onderworpen aan de standaard verbindings limiet per hostnaam of IP-adres. Mogelijk moet u een hogere `MaxConnections` waarde instellen (100 tot 1.000), zodat de client bibliotheek meerdere gelijktijdige verbindingen met Azure Cosmos db kan gebruiken. In .NET SDK 1.8.0 en hoger is de standaard waarde voor [ServicePointManager. DefaultConnectionLimit](https://msdn.microsoft.com/library/system.net.servicepointmanager.defaultconnectionlimit.aspx) 50. Als u de waarde wilt wijzigen, kunt u [Documents. client. Connection Policy. MaxConnectionLimit](https://msdn.microsoft.com/library/azure/microsoft.azure.documents.client.connectionpolicy.maxconnectionlimit.aspx) op een hogere waarde instellen.

**Parallelle query's voor gepartitioneerde verzamelingen afstemmen**

SQL .NET SDK 1.9.0 en hoger ondersteunen parallelle query's, waarmee u parallel een gepartitioneerde verzameling kunt uitvoeren. Zie [code voorbeelden](https://github.com/Azure/azure-documentdb-dotnet/blob/master/samples/code-samples/Queries/Program.cs) met betrekking tot het werken met de sdk's voor meer informatie. Parallelle query's zijn ontworpen om betere latentie en door Voer van query's te bieden dan hun seriële tegen hanger. Parallelle query's bieden twee para meters die u aan uw vereisten kunt aanpassen: 
- `MaxDegreeOfParallelism`Hiermee bepaalt u het maximum aantal partities waarmee gelijktijdig query's kunnen worden uitgevoerd. 
- `MaxBufferedItemCount`Hiermee bepaalt u het aantal vooraf opgehaalde resultaten.

***Afstemmings graad van parallelle uitvoering***

Parallelle query werkt door meerdere partities parallel te doorzoeken. Maar gegevens van een afzonderlijke partitie worden serieel opgehaald ten opzichte van de query. De `MaxDegreeOfParallelism` instelling in [SDK v2](sql-api-sdk-dotnet.md) of `MaxConcurrency` in [SDK v3](sql-api-sdk-dotnet-standard.md) voor het aantal partities is de beste kans om de meest uitvoerende query te bereiken, op voor waarde dat alle andere systeem omstandigheden hetzelfde blijven. Als u het aantal partities niet weet, kunt u de mate van parallelle uitvoering instellen op een hoog getal. Het systeem kiest het minimum (aantal partities, door de gebruiker opgegeven invoer) als de mate van parallelle uitvoering.

Houd er rekening mee dat parallelle query's het meest voor deel opleveren als de gegevens gelijkmatig worden verdeeld over alle partities met betrekking tot de query. Als de gepartitioneerde verzameling is gepartitioneerd zodat alle of de meeste gegevens die door een query zijn geretourneerd, in een paar partities worden geconcentreerd (één partitie is het ergste geval), kunnen deze partities de prestaties van de query opsporen.

***MaxBufferedItemCount afstemmen***
    
Parallelle query is ontworpen om de resultaten vooraf op te halen terwijl de huidige batch met resultaten door de client wordt verwerkt. Deze vooraf ophalen helpt de algehele latentie van een query te verbeteren. De `MaxBufferedItemCount` para meter beperkt het aantal vooraf opgehaalde resultaten. Stel `MaxBufferedItemCount` in op het verwachte aantal geretourneerde resultaten (of een hoger getal), zodat de query het maximale voor deel van het vooraf ophalen kan ontvangen.

Het vooraf ophalen werkt op dezelfde manier, ongeacht de mate van parallellisme, en er is één buffer voor de gegevens van alle partities.  

**Uitstel implementeren met RetryAfter-intervallen**

Tijdens de prestatie tests moet u de belasting verg Roten tot een klein aantal aanvragen wordt beperkt. Als aanvragen worden beperkt, moet de client toepassing worden uitgeschakeld op een vertraging voor het door de server opgegeven interval voor nieuwe pogingen. Door de uitstel te respecteren, zorgt u ervoor dat u een minimale hoeveelheid tijd kunt wachten tussen nieuwe pogingen. 

Ondersteuning voor het beleid voor opnieuw proberen is opgenomen in deze Sdk's:
- Versie 1.8.0 en hoger van de [.NET SDK voor SQL](sql-api-sdk-dotnet.md) en de [Java SDK voor SQL](sql-api-sdk-java.md)
- Versie 1.9.0 en hoger van de [node. js SDK voor SQL](sql-api-sdk-node.md) en de [python-SDK voor SQL](sql-api-sdk-python.md)
- Alle ondersteunde versies van de [.net core](sql-api-sdk-dotnet-core.md) sdk's 

Zie [RetryAfter](https://msdn.microsoft.com/library/microsoft.azure.documents.documentclientexception.retryafter.aspx)voor meer informatie.
    
In versie 1,19 en hoger van de .NET SDK is er een mechanisme voor het vastleggen van aanvullende diagnostische gegevens en latentie problemen bij problemen, zoals wordt weer gegeven in het volgende voor beeld. U kunt de diagnostische teken reeks registreren voor aanvragen met een hogere lees latentie. De vastgelegde diagnostische teken reeks helpt u inzicht te krijgen in het aantal keren dat u 429 fouten hebt ontvangen voor een bepaalde aanvraag.

```csharp
ResourceResponse<Document> readDocument = await this.readClient.ReadDocumentAsync(oldDocuments[i].SelfLink);
readDocument.RequestDiagnosticsString 
```

**Document-Uri's in cache opslaan voor minder lees latentie**

Cache waar mogelijk document-Uri's voor de beste Lees prestaties. U moet de logica definiëren om de resource-ID in de cache op te slaan wanneer u een resource maakt. Zoek acties op basis van resource-Id's zijn sneller dan op naam gebaseerde zoek acties, waardoor de prestaties worden verbeterd in de cache.

   <a id="tune-page-size"></a>
**De pagina grootte voor query's/feeds voor betere prestaties afstemmen**

Wanneer u een bulk-Lees bewerking uitvoert van documenten met behulp van de functie voor `ReadDocumentFeedAsync`Lees bewerkingen (bijvoorbeeld) of wanneer u een SQL-query uitgeeft, worden de resultaten op een gesegmenteerde manier geretourneerd als de resultatenset te groot is. Standaard worden resultaten geretourneerd in delen van 100 items of 1 MB, waarbij de limiet eerst wordt bereikt.

Om het aantal netwerk round trips te verminderen dat vereist is om alle toepasselijke resultaten op te halen, kunt u de pagina grootte verg Roten door [x-MS-maximum-item-Count](https://docs.microsoft.com/rest/api/cosmos-db/common-cosmosdb-rest-request-headers) te gebruiken om 1.000-headers aan te vragen. Als u slechts enkele resultaten wilt weer geven, bijvoorbeeld als uw gebruikers interface of toepassings-API slechts 10 resultaten per keer retourneert, kunt u ook de pagina grootte verkleinen tot 10 om de door Voer voor lees bewerkingen en query's te verminderen.

> [!NOTE] 
> De `maxItemCount` eigenschap mag niet alleen worden gebruikt voor paginering. Het belangrijkste gebruik is het verbeteren van de prestaties van query's door het maximum aantal items te verminderen dat op één pagina wordt geretourneerd.  

U kunt ook de pagina grootte instellen met behulp van de beschik bare Azure Cosmos DB Sdk's. Met [MaxItemCount](/dotnet/api/microsoft.azure.documents.client.feedoptions.maxitemcount?view=azure-dotnet) de eigenschap MaxItemCount `FeedOptions` in kunt u het maximum aantal items instellen dat in de opsommings bewerking moet worden geretourneerd. Wanneer `maxItemCount` is ingesteld op-1, detecteert de SDK automatisch de optimale waarde, afhankelijk van de grootte van het document. Bijvoorbeeld:
    
```csharp
IQueryable<dynamic> authorResults = client.CreateDocumentQuery(documentCollection.SelfLink, "SELECT p.Author FROM Pages p WHERE p.Title = 'About Seattle'", new FeedOptions { MaxItemCount = 1000 });
```
    
Wanneer een query wordt uitgevoerd, worden de resulterende gegevens verzonden binnen een TCP-pakket. Als u te laag een waarde opgeeft voor `maxItemCount`, is het aantal trips dat nodig is voor het verzenden van de gegevens in het TCP-pakket hoog. Dit is van invloed op de prestaties. Als u niet zeker weet welke waarde voor de `maxItemCount` eigenschap moet worden ingesteld, kunt u deze het beste instellen op-1 en de SDK de standaard waarde laten kiezen.

**Het aantal threads/taken verhogen**

Zie [het aantal threads/taken](#increase-threads) in het gedeelte netwerken van dit artikel verg Roten.

## <a name="indexing-policy"></a>Indexeringsbeleid
 
**Niet-gebruikte paden uitsluiten van indexeren voor snellere schrijf bewerkingen**

Met het Azure Cosmos DB indexerings beleid kunt u ook opgeven welke document paden u wilt opnemen in of uitsluiten van indexeren met behulp van index paden (IndexingPolicy. IncludedPaths en IndexingPolicy. ExcludedPaths). Indexerings paden kunnen de schrijf prestaties verbeteren en de index opslag beperken voor scenario's waarin de query patronen vooraf bekend zijn. Dit komt doordat de index kosten direct overeenkomen met het aantal unieke paden dat is geïndexeerd. Deze code laat bijvoorbeeld zien hoe u een volledige sectie van de documenten (een substructuur) uitsluit van indexeren met behulp van het Joker teken ' * ':

```csharp
var collection = new DocumentCollection { Id = "excludedPathCollection" };
collection.IndexingPolicy.IncludedPaths.Add(new IncludedPath { Path = "/*" });
collection.IndexingPolicy.ExcludedPaths.Add(new ExcludedPath { Path = "/nonIndexedContent/*");
collection = await client.CreateDocumentCollectionAsync(UriFactory.CreateDatabaseUri("db"), excluded);
```

Zie [Azure Cosmos DB Indexing policies](index-policy.md)(Engelstalig) voor meer informatie.

## <a name="throughput"></a>Doorvoer
<a id="measure-rus"></a>

**Meten en afstemmen voor lagere aanvraag eenheden/tweede gebruik**

Azure Cosmos DB biedt een uitgebreide set database bewerkingen. Deze bewerkingen omvatten relationele en hiërarchische query's met Udf's, opgeslagen procedures en triggers, die allemaal op de documenten in een database verzameling worden uitgevoerd. De kosten voor elk van deze bewerkingen variëren, afhankelijk van de CPU, IO en het geheugen die nodig zijn om de bewerking te volt ooien. In plaats van te denken over en het beheren van hardwarebronnen, kunt u een aanvraag eenheid (RU) beschouwen als een enkele maat eenheid voor de resources die nodig zijn om verschillende database bewerkingen uit te voeren en een toepassings aanvraag te onderhouden.

De door Voer is ingericht op basis van het aantal ingestelde [aanvraag eenheden](request-units.md) voor elke container. Het verbruik van de aanvraag eenheid wordt geëvalueerd als een rente per seconde. Toepassingen die het aantal ingerichte aanvraag eenheden voor hun container overschrijden, zijn beperkt tot het aantal onder het ingerichte niveau voor de container daalt. Als voor uw toepassing een hogere door Voer is vereist, kunt u de door Voer verhogen door extra aanvraag eenheden in te richten.

De complexiteit van een query is van invloed op het aantal aanvraag eenheden dat voor een bewerking wordt verbruikt. Het aantal predikaten, de aard van de predikaten, het aantal Udf's en de grootte van de bron gegevensset beïnvloeden alle de kosten van de query bewerkingen.

Als u de overhead van een bewerking (maken, bijwerken of verwijderen) wilt meten, inspecteert u de [x-MS-Request-](https://docs.microsoft.com/rest/api/cosmos-db/common-cosmosdb-rest-response-headers) factuurkop tekst ( `RequestCharge` of de `ResourceResponse\<T>` equivalente eigenschap in of `FeedResponse\<T>` in de .NET SDK) om het aantal aanvraag eenheden te meten dat door de bewerkingen wordt verbruikt:

```csharp
// Measure the performance (Request Units) of writes
ResourceResponse<Document> response = await client.CreateDocumentAsync(collectionSelfLink, myDocument);
Console.WriteLine("Insert of document consumed {0} request units", response.RequestCharge);
// Measure the performance (Request Units) of queries
IDocumentQuery<dynamic> queryable = client.CreateDocumentQuery(collectionSelfLink, queryString).AsDocumentQuery();
while (queryable.HasMoreResults)
    {
        FeedResponse<dynamic> queryResponse = await queryable.ExecuteNextAsync<dynamic>();
        Console.WriteLine("Query batch consumed {0} request units", queryResponse.RequestCharge);
    }
```             

De aanvraag kosten die in deze header worden geretourneerd, zijn een fractie van uw ingerichte door Voer (dat wil zeggen 2.000 RUs/seconde). Als de voor gaande query bijvoorbeeld 1.000 1 KB-documenten retourneert, zijn de kosten van de bewerking 1.000. Binnen één seconde worden er dus slechts twee aanvragen door de server gehonoreerd voordat het aantal latere aanvragen wordt beperkt. Zie [aanvraag eenheden](request-units.md) en de [reken eenheid voor aanvragen](https://www.documentdb.com/capacityplanner)voor meer informatie.
<a id="429"></a>

**Aantal verwerkings frequenties/aanvragen afhandelen te groot**

Wanneer een client de gereserveerde door Voer voor een account probeert te overschrijden, is er geen prestatie vermindering op de server en wordt er geen gebruik van de doorvoer capaciteit meer dan het gereserveerde niveau. De server preventief de aanvraag met RequestRateTooLarge te beëindigen (HTTP-status code 429). Het retourneert een [x-MS-retry-after-MS](https://docs.microsoft.com/rest/api/cosmos-db/common-cosmosdb-rest-response-headers) -header die de hoeveelheid tijd, in milliseconden, aangeeft dat de gebruiker moet wachten voordat de aanvraag opnieuw wordt geprobeerd.

    HTTP Status 429,
    Status Line: RequestRateTooLarge
    x-ms-retry-after-ms :100

De Sdk's ondervangen dit antwoord impliciet, respecteert de door de server opgegeven nieuwe poging na de header en voert de aanvraag opnieuw uit. Tenzij uw account gelijktijdig wordt geopend door meerdere clients, zal de volgende poging slagen.

Als u meer dan één client cumulatief op dezelfde manier hebt uitgevoerd, is het standaard aantal nieuwe pogingen dat momenteel is ingesteld op 9 intern door de client mogelijk niet voldoende. In dit geval genereert de client een DocumentClientException met de status code 429 naar de toepassing. 

U kunt het standaard aantal nieuwe pogingen wijzigen door de `RetryOptions` op het `ConnectionPolicy` exemplaar in te stellen. Standaard wordt de DocumentClientException met de status code 429 geretourneerd na een cumulatieve wacht tijd van 30 seconden als de aanvraag boven het aanvraag aantal blijft. Deze fout wordt weer gegeven zelfs wanneer het huidige aantal nieuwe pogingen kleiner is dan het maximum aantal nieuwe pogingen, of de huidige waarde de standaard instelling is van 9 of een door de gebruiker gedefinieerde waarde.

Het gedrag voor automatische pogingen helpt de flexibiliteit en bruikbaarheid voor de meeste toepassingen te verbeteren. Het is echter mogelijk niet het beste gedrag wanneer u prestatie benchmarks uitvoert, met name wanneer u een latentie meet. De door de client waargenomen latentie krijgt een waarschuwing als het experiment de server beperking bereikt en zorgt ervoor dat de client-SDK op de achtergrond opnieuw probeert. Om latentie pieken te voor komen tijdens prestatie experimenten, meet u de kosten die worden geretourneerd door elke bewerking en zorgt u ervoor dat aanvragen onder het gereserveerde aanvraag tarief vallen. Zie [aanvraag eenheden](request-units.md)voor meer informatie.

**Voor een hogere door Voer ontwerpen voor kleinere documenten**

De aanvraag kosten (dat wil zeggen, de aanvraag verwerkings kosten) van een bepaalde bewerking correleert direct naar de grootte van het document. Bewerkingen voor grote documenten kosten meer dan bewerkingen op kleine documenten.

## <a name="next-steps"></a>Volgende stappen
Zie [prestaties en schalen testen met Azure Cosmos DB](performance-testing.md)voor een voorbeeld toepassing die wordt gebruikt om Azure Cosmos DB te evalueren voor scenario's met hoge prestaties op een aantal client computers.

Zie [partitioneren en schalen in azure Cosmos DB](partition-data.md)voor meer informatie over het ontwerpen van uw toepassing voor schaal baarheid en hoge prestaties.

---
title: Azure Synapse-koppeling voor Azure Cosmos DB configureren en gebruiken (preview)
description: Meer informatie over het inschakelen van Synapse-koppeling voor Azure Cosmos-accounts, het maken van een container met een analytische opslag, het verbinden van de Azure Cosmos-data base in Synapse-werk ruimte en het uitvoeren van query's.
author: SriChintala
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 05/19/2020
ms.author: srchi
ms.openlocfilehash: 24f6f77f1371157ca1b57cfd85ac196cace822fc
ms.sourcegitcommit: 50673ecc5bf8b443491b763b5f287dde046fdd31
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/20/2020
ms.locfileid: "83676000"
---
# <a name="configure-and-use-azure-synapse-link-for-azure-cosmos-db-preview"></a>Azure Synapse-koppeling voor Azure Cosmos DB configureren en gebruiken (preview)

De Synapse-koppeling voor Azure Cosmos DB is een HTAP-functie (hybride transactionele activiteit) in de Cloud waarmee u bijna realtime analyses kunt uitvoeren via operationele gegevens in Azure Cosmos DB. Synapse-koppeling maakt een strakkere integratie tussen Azure Cosmos DB en Azure Synapse Analytics.


> [!IMPORTANT]
> Als u Azure Synapse-koppeling wilt gebruiken, zorgt u ervoor dat u uw Azure Cosmos-account inricht & Azure Synapse Analytics-werk ruimte in een van de hierboven ondersteunde regio's. Zie [Azure service-updates](https://azure.microsoft.com/updates/)voor een lijst met ondersteunde regio's. 

Gebruik de volgende stappen om analytische query's uit te voeren met de Synapse-koppeling voor Azure Cosmos DB:

* [Synapse-koppeling inschakelen voor uw Azure Cosmos-accounts](#enable-synapse-link)
* [Een Azure Cosmos-container maken waarvoor een analytische opslag is ingeschakeld](#create-analytical-ttl)
* [Uw Azure Cosmos-data base koppelen aan een Synapse-werk ruimte](#connect-to-cosmos-database)
* [Een query uitvoeren op de analytische opslag met behulp van Synapse Spark](#query-analytical-store)

## <a name="enable-azure-synapse-link-for-azure-cosmos-accounts"></a><a id="enable-synapse-link"></a>Azure Synapse-koppeling voor Azure Cosmos-accounts inschakelen

### <a name="azure-portal"></a>Azure Portal

1. Meld u aan bij [Azure Portal](https://portal.azure.com/).

1. [Maak een nieuw Azure-account](create-sql-api-dotnet.md#create-account)of selecteer een bestaand Azure Cosmos-account.

1. Ga naar uw Azure Cosmos-account en open het deel venster **functies** .

1. Selecteer **Synapse link** in de lijst met functies.

   ![Preview-functie voor Synapse link zoeken](./media/configure-synapse-link/find-synapse-link-feature.png)

1. Vervolgens wordt u gevraagd om de Synapse-koppeling in te scha kelen voor uw account. Selecteer Inschakelen.

   ![Synapse-koppelings functie inschakelen](./media/configure-synapse-link/enable-synapse-link-feature.png)

1. Uw account is nu ingeschakeld voor het gebruik van de Synapse-koppeling. Zie voor meer informatie over het maken van containers voor analytische opslag om automatisch te beginnen met het repliceren van uw operationele gegevens uit het transactionele archief naar de analytische opslag.

### <a name="azure-resource-manager-template"></a>Azure Resource Manager-sjabloon

De [Azure Resource Manager-sjabloon](manage-sql-with-resource-manager.md#azure-cosmos-account-with-analytical-store) maakt een Azure Cosmos-account met Synapse-koppeling voor SQL-API. Met deze sjabloon maakt u een core-API-account (SQL) in één regio met een container die is geconfigureerd met analytische TTL ingeschakeld en een optie om de door Voer van hand matig of automatisch schalen te gebruiken. Als u deze sjabloon wilt implementeren, klikt **u op implementeren in azure** op de pagina README.

## <a name="create-an-azure-cosmos-container-with-analytical-store"></a><a id="create-analytical-ttl"></a>Een Azure Cosmos-container maken met een analytische opslag

U kunt de analytische opslag inschakelen op een Azure Cosmos-container tijdens het maken van de container. U kunt de Azure Portal gebruiken of de `analyticalTTL` eigenschap configureren tijdens het maken van de container met behulp van de Azure Cosmos DB sdk's.

> [!NOTE]
> Op dit moment kunt u het analytische archief inschakelen voor **nieuwe** containers (zowel in nieuwe als bestaande accounts).

### <a name="azure-portal"></a>Azure Portal

1. Meld u aan bij de [Azure Portal](https://portal.azure.com/) of [Azure Cosmos Explorer](https://cosmos.azure.com/).

1. Ga naar uw Azure Cosmos-account en open het tabblad **Data Explorer** .

1. Selecteer **nieuwe container** en voer een naam in voor uw data base, container, partitie sleutel en gegevens doorvoer. Schakel de optie voor de **analytische opslag** in. Nadat u het analytische archief hebt ingeschakeld, maakt het een container waarvan `AnalyicalTTL` de eigenschap is ingesteld op de standaard waarde-1 (oneindige retentie). Dit analytische archief behoudt alle historische versies van records.

   ![Analytische opslag inschakelen voor Azure Cosmos-container](./media/configure-synapse-link/create-container-analytical-store.png)

1. Als u de Synapse-koppeling voor dit account nog niet hebt ingeschakeld, wordt u gevraagd dit te doen omdat het een vereiste is om een container voor een analytische opslag te maken. Selecteer **Synapse-koppeling inschakelen**als u hierom wordt gevraagd.

1. Selecteer **OK**om een Azure Cosmos-container met analytische opslag te maken.

### <a name="net-sdk"></a>.NET SDK

Met de volgende code wordt een container met analytische opslag gemaakt met behulp van de .NET SDK. Stel de eigenschap van de analytische TTL in op de vereiste waarde. Voor de lijst met toegestane waarden raadpleegt u het artikel [analytische TTL-waarden](analytical-store-introduction.md#analytical-ttl) die worden ondersteund:

```csharp
// Create a container with a partition key, and analytical TTL configured to  -1 (infinite retention)
string containerId = “myContainerName”;
int analyticalTtlInSec = -1;
ContainerProperties cpInput = new ContainerProperties()
            {
Id = containerId,
PartitionKeyPath = "/id",
AnalyticalStorageTimeToLiveInSeconds = analyticalTtlInSec,
};
 await this. cosmosClient.GetDatabase("myDatabase").CreateContainerAsync(cpInput);
```

### <a name="java-v4-sdk"></a>Java v4-SDK

Met de volgende code wordt een container met analytische opslag gemaakt met behulp van de Java v4-SDK. Stel de `AnalyticalStoreTimeToLiveInSeconds` eigenschap in op de vereiste waarde:

```java
// Create a container with a partition key and  analytical TTL configured to  -1 (infinite retention) 
CosmosContainerProperties containerProperties = new CosmosContainerProperties("myContainer", "/myPartitionKey");

containerProperties.setAnalyticalStoreTimeToLiveInSeconds(-1);

container = database.createContainerIfNotExists(containerProperties, 400).block().getContainer();
```

### <a name="python-v3-sdk"></a>Python v3 SDK

Met de volgende code wordt een container met analytische opslag gemaakt met behulp van de python-SDK:

```python
import azure.cosmos.cosmos_client as cosmos_client
def create_collection_if_not_exists(cosmosEndpoint, cosmosKey, databaseName, collectionName):
    client = cosmos_client.CosmosClient(url_connection=cosmosEndpoint, auth={'masterKey': cosmosKey})

db = client.QueryDatabases("select * from c where c.id = '" + databaseName + "'").fetch_next_block()[0]
options = {
    'offerThroughput': 1000
}

container_definition = {
    'id': collectionName,
    "partitionKey": {  
        "paths": [  
        "/id"  
        ],  
        "kind": "Hash" 
    },
    "indexingPolicy": {  
    "indexingMode": "consistent",  
    "automatic": True,  
    "includedPaths": [],  
    "excludedPaths": [{
        "path": "/*"
    }]  
    },
    "defaultTtl": -1,
    "analyticalStorageTtl": -1
}

container = client.CreateContainer(db['_self'], container_definition, options)
```

### <a name="update-the-analytical-store-time-to-live"></a><a id="update-analytical-ttl"></a>De time-outwaarde voor het analytische archief bijwerken

Nadat de analytische opslag met een bepaalde TTL-waarde is ingeschakeld, kunt u deze later bijwerken naar een andere geldige waarde. U kunt de waarde bijwerken met behulp van de Azure Portal of Sdk's. Zie voor meer informatie over de verschillende configuratie opties voor de analyse-TTL het artikel [analytische TTL-waarden](analytical-store-introduction.md#analytical-ttl) .

#### <a name="azure-portal"></a>Azure Portal

Als u een container voor een analytische opslag hebt gemaakt via de Azure Portal, bevat deze een standaard analyse-TTL van-1. Gebruik de volgende stappen om deze waarde bij te werken:

1. Meld u aan bij de [Azure Portal](https://portal.azure.com/) of [Azure Cosmos Explorer](https://cosmos.azure.com/).

1. Ga naar uw Azure Cosmos-account en open het tabblad **Data Explorer** .

1. Selecteer een bestaande container waarvoor het analytische archief is ingeschakeld. Vouw het item uit en wijzig de volgende waarden:

  * Open het venster **Schaal en instellingen**.
  * Onder **instelling** zoeken, * * analytische opslag time to Live * *.
  * Selecteer **On (no default)** of **Aan** en stel een TTL-waarde in
  * Klik op **Opslaan** om de wijzigingen op te slaan.

#### <a name="net-sdk"></a>.NET SDK

De volgende code laat zien hoe u de TTL voor de analyse opslag bijwerkt met behulp van de .NET SDK:

```csharp
// Get the container, update AnalyticalStorageTimeToLiveInSeconds 
ContainerResponse containerResponse = await client.GetContainer("database", "container").ReadContainerAsync();
// Update analytical store TTL
containerResponse.Resource. AnalyticalStorageTimeToLiveInSeconds = 60 * 60 * 24 * 180  // Expire analytical store data in 6 months;
await client.GetContainer("database", "container").ReplaceContainerAsync(containerResponse.Resource);
```

#### <a name="java-v4-sdk"></a>Java v4-SDK

De volgende code laat zien hoe u de TTL voor de analytische opslag bijwerkt met behulp van de Java v4 SDK:

```java
CosmosContainerProperties containerProperties = new CosmosContainerProperties("myContainer", "/myPartitionKey");

// Update analytical store TTL to expire analytical store data in 6 months;
containerProperties.setAnalyticalStoreTimeToLiveInSeconds (60 * 60 * 24 * 180 );  
 
// Update container settings
container.replace(containerProperties).block();
```

## <a name="connect-to-a-synapse-workspace"></a><a id="connect-to-cosmos-database"></a>Verbinding maken met een Synapse-werk ruimte

Volg de instructies in [verbinding maken met Azure Synapse link](../synapse-analytics/synapse-link/how-to-connect-synapse-link-cosmos-db.md) voor toegang tot een Azure Cosmos DB Data Base vanuit Azure Synapse Analytics Studio met Azure Synapse link.

## <a name="query-using-synapse-spark"></a><a id="query-analytical-store"></a>Query's uitvoeren met Synapse Spark

Volg de instructies in het artikel [query Azure Cosmos DB Analytical Store](../synapse-analytics/synapse-link/how-to-query-analytical-store-spark.md) voor informatie over het uitvoeren van Query's met Synapse Spark. In dit artikel vindt u enkele voor beelden van de manier waarop u kunt communiceren met de analytische opslag vanuit Synapse-gebaren. Deze gebaren worden weer gegeven wanneer u met de rechter muisknop op een container klikt. Met penbewegingen kunt u snel code genereren en deze aanpassen aan uw behoeften. Ze zijn ook ideaal voor het detecteren van gegevens met één klik.

## <a name="getting-started-with-azure-synpase-link---samples"></a><a id="cosmosdb-synapse-link-samples"></a>Aan de slag met Azure Synpase-koppeling-voor beelden

U kunt voor beelden vinden om aan de slag te gaan met de koppeling van Azure Synapse op [github](https://aka.ms/cosmosdb-synapselink-samples). In deze etalage worden end-to-end-oplossingen gedemonstreerd met IoT-en retail-scenario's.

## <a name="next-steps"></a>Volgende stappen

Raadpleeg de volgende documenten voor meer informatie:

* [Azure Synapse-koppeling voor Azure Cosmos DB.](synapse-link.md)

* [Overzicht van Azure Cosmos DB Analytical Store.](analytical-store-introduction.md)

* [Veelgestelde vragen over de Synapse-koppeling voor Azure Cosmos DB.](synapse-link-frequently-asked-questions.md)

* [Apache Spark in azure Synapse Analytics](../synapse-analytics/spark/apache-spark-concepts.md).

* [SQL serverloos/op aanvraag in azure Synapse Analytics](../synapse-analytics/sql/on-demand-workspace-overview.md).

---
title: Zoeken in CSV-blobs
titleSuffix: Azure Cognitive Search
description: Pak CSV-bestand uit vanuit Azure Blob-opslag en importeer dit met behulp van de delimitedText-parserings modus.
manager: nitinme
author: mgottein
ms.author: magottei
ms.devlang: rest-api
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 11/04/2019
ms.openlocfilehash: bf600890bfed570e712a159005b8ef5267298cc0
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/28/2020
ms.locfileid: "76122318"
---
# <a name="how-to-index-csv-blobs-using-delimitedtext-parsing-mode-and-blob-indexers-in-azure-cognitive-search"></a>Het indexeren van CSV-blobs met behulp van de delimitedText-verwerkings modus en BLOB-Indexeer functies in azure Cognitive Search

Standaard parseert [Azure Cognitive Search BLOB-indexering](search-howto-indexing-azure-blob-storage.md) gescheiden tekst-blobs als één tekst segment. Met blobs die CSV-gegevens bevatten, wilt u echter vaak elke regel in de BLOB behandelen als een afzonderlijk document. Als u bijvoorbeeld de volgende tekst met scheidings tekens wilt, kunt u deze in twee documenten parseren, elk met de velden ' id ', ' datePublished ' en ' Tags ': 

    id, datePublished, tags
    1, 2016-01-12, "azure-search,azure,cloud" 
    2, 2016-07-07, "cloud,mobile" 

In dit artikel leert u hoe u CSV-blobs kunt parseren met een Azure Cognitive Search BLOB-indexer door de `delimitedText` modus voor parseren in te stellen. 

> [!NOTE]
> Volg de aanbevelingen voor de configuratie van de Indexeer functie in [een-op-veel-indexering](search-howto-index-one-to-many-blobs.md) om meerdere zoek documenten uit één Azure-Blob af te voeren.

## <a name="setting-up-csv-indexing"></a>CSV-indexering instellen
Als u CSV-blobs wilt indexeren, maakt of werkt u een Indexeer `delimitedText` functie definitie met de parserings modus op een aanvraag voor het maken van een [Indexeer](https://docs.microsoft.com/rest/api/searchservice/create-indexer) functie:

    {
      "name" : "my-csv-indexer",
      ... other indexer properties
      "parameters" : { "configuration" : { "parsingMode" : "delimitedText", "firstLineContainsHeaders" : true } }
    }

`firstLineContainsHeaders`geeft aan dat de eerste (niet-lege) regel van elke BLOB kopteksten bevat.
Als blobs geen oorspronkelijke header regel bevatten, moeten de headers worden opgegeven in de Indexeer functie: 

    "parameters" : { "configuration" : { "parsingMode" : "delimitedText", "delimitedTextHeaders" : "id,datePublished,tags" } } 

U kunt het scheidings teken aanpassen met `delimitedTextDelimiter` behulp van de configuratie-instelling. Bijvoorbeeld:

    "parameters" : { "configuration" : { "parsingMode" : "delimitedText", "delimitedTextDelimiter" : "|" } }

> [!NOTE]
> Op dit moment wordt alleen de UTF-8-code ring ondersteund. Als u ondersteuning voor andere code ringen nodig hebt, stem u dan op [UserVoice](https://feedback.azure.com/forums/263029-azure-search).

> [!IMPORTANT]
> Wanneer u de modus voor het parseren van tekst met scheidings tekens gebruikt Cognitive Search, wordt ervan uitgegaan dat alle blobs in uw gegevens bron CSV zijn. Als u een combi natie van CSV-en niet-CSV-blobs in dezelfde gegevens bron wilt ondersteunen, moet u deze op [UserVoice](https://feedback.azure.com/forums/263029-azure-search)stemmen.
> 
> 

## <a name="request-examples"></a>Aanvraag voorbeelden
Als u dit alles gebruikt, zijn hier de volledige voor beelden van payload. 

Bron 

    POST https://[service name].search.windows.net/datasources?api-version=2019-05-06
    Content-Type: application/json
    api-key: [admin key]

    {
        "name" : "my-blob-datasource",
        "type" : "azureblob",
        "credentials" : { "connectionString" : "DefaultEndpointsProtocol=https;AccountName=<account name>;AccountKey=<account key>;" },
        "container" : { "name" : "my-container", "query" : "<optional, my-folder>" }
    }   

Indexeer functie

    POST https://[service name].search.windows.net/indexers?api-version=2019-05-06
    Content-Type: application/json
    api-key: [admin key]

    {
      "name" : "my-csv-indexer",
      "dataSourceName" : "my-blob-datasource",
      "targetIndexName" : "my-target-index",
      "parameters" : { "configuration" : { "parsingMode" : "delimitedText", "delimitedTextHeaders" : "id,datePublished,tags" } }
    }

## <a name="help-us-make-azure-cognitive-search-better"></a>Help ons Azure Cognitive Search beter te maken
Als u een functie verzoek of ideeën voor verbeteringen hebt, geeft u uw invoer op [UserVoice](https://feedback.azure.com/forums/263029-azure-search/)op.


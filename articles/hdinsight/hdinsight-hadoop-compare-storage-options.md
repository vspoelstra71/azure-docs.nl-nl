---
title: Opslag opties vergelijken voor gebruik met Azure HDInsight-clusters
description: Biedt een overzicht van opslag typen en hoe deze werken met Azure HDInsight.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.custom: seoapr2020
ms.date: 04/21/2020
ms.openlocfilehash: ed93ba937a843618f36bac6e88b15ff77355ca75
ms.sourcegitcommit: 50ef5c2798da04cf746181fbfa3253fca366feaa
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/30/2020
ms.locfileid: "82610697"
---
# <a name="compare-storage-options-for-use-with-azure-hdinsight-clusters"></a>Opslag opties vergelijken voor gebruik met Azure HDInsight-clusters

U kunt kiezen tussen een aantal verschillende Azure Storage-services bij het maken van HDInsight-clusters:

* [Azure Storage](./overview-azure-storage.md)
* [Azure Data Lake Storage Gen2](./overview-data-lake-storage-gen2.md)
* [Azure Data Lake Storage Gen1](./overview-data-lake-storage-gen1.md)

Dit artikel bevat een overzicht van deze opslag typen en de bijbehorende unieke functies.

## <a name="storage-types-and-features"></a>Opslag typen en-functies

De volgende tabel bevat een overzicht van de Azure Storage services die worden ondersteund door verschillende versies van HDInsight:

| Opslag service | Accounttype | Type naam ruimte | Ondersteunde services | Ondersteunde prestatie lagen | Ondersteunde toegangs lagen | HDInsight-versie | Cluster type |
|---|---|---|---|---|---|---|---|
|Azure Data Lake Storage Gen2| Voor algemeen gebruik v2 | Hiërarchisch (bestands systeem) | Blob | Standard | Hot, cool, Archive | 3.6 + | Alle behalve Spark 2,1 en 2,2|
|Azure Storage| Voor algemeen gebruik v2 | Object | Blob | Standard | Hot, cool, Archive | 3.6 + | Alle |
|Azure Storage| Algemeen v1 | Object | Blob | Standard | N.v.t. | Alle | Alle |
|Azure Storage| Blob Storage * * | Object | Blok-BLOB | Standard | Hot, cool, Archive | Alle | Alle |
|Azure Data Lake Storage Gen1| N.v.t. | Hiërarchisch (bestands systeem) | N.v.t. | N.v.t. | N.v.t. | alleen 3,6 | Alle behalve HBase |

* * Voor HDInsight-clusters kunnen alleen secundaire opslag accounts van het type BlobStorage en page BLOB worden ondersteund.

Zie [overzicht van Azure Storage-accounts](../storage/common/storage-account-overview.md) voor meer informatie over Azure Storage-account typen

Zie [Azure Blob-opslag: Premium (preview), hot, cool en archief opslag lagen](../storage/blobs/storage-blob-storage-tiers.md) voor meer informatie over Azure Storage toegangs lagen.

U kunt clusters maken met behulp van combi Naties van services voor primaire en optionele secundaire opslag. De volgende tabel bevat een overzicht van de configuraties voor cluster opslag die momenteel worden ondersteund in HDInsight:

| HDInsight-versie | Primaire opslag | Secundaire opslag | Ondersteund |
|---|---|---|---|
| 3,6 & 4,0 | Algemeen v1, Algemeen v2 | Algemeen v1, Algemeen v2, BlobStorage (blok-blobs) | Ja |
| 3,6 & 4,0 | Algemeen v1, Algemeen v2 | Data Lake Storage Gen2 | Nee |
| 3,6 & 4,0 | Data Lake Storage Gen2 * | Data Lake Storage Gen2 | Ja |
| 3,6 & 4,0 | Data Lake Storage Gen2 * | Algemeen v1, Algemeen v2, BlobStorage (blok-blobs) | Ja |
| 3,6 & 4,0 | Data Lake Storage Gen2 | Data Lake Storage Gen1 | Nee |
| 3,6 | Data Lake Storage Gen1 | Data Lake Storage Gen1 | Ja |
| 3,6 | Data Lake Storage Gen1 | Algemeen v1, Algemeen v2, BlobStorage (blok-blobs) | Ja |
| 3,6 | Data Lake Storage Gen1 | Data Lake Storage Gen2 | Nee |
| 4,0 | Data Lake Storage Gen1 | Alle | Nee |
| 4,0 | Algemeen v1, Algemeen v2 | Data Lake Storage Gen1 | Nee |

* = Dit kan een of meer Data Lake Storage Gen2 accounts zijn, zolang alle instellingen dezelfde beheerde identiteit voor toegang tot het cluster gebruiken.

> [!NOTE]
> Data Lake Storage Gen2 primaire opslag wordt niet ondersteund voor Spark 2,1-of 2,2-clusters.

## <a name="next-steps"></a>Volgende stappen

* [Azure Storage-overzicht](./overview-azure-storage.md)
* [Azure Data Lake Storage Gen1-overzicht](./overview-data-lake-storage-gen1.md)
* [Azure Data Lake Storage Gen2-overzicht](./overview-data-lake-storage-gen2.md)
* [Inleiding tot Azure Data Lake Storage Gen2](../storage/blobs/data-lake-storage-introduction.md)
* [Kennismaking met Azure Storage](../storage/common/storage-introduction.md)

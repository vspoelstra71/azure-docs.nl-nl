---
title: bestand opnemen
description: bestand opnemen
services: storage
author: tamram
ms.service: storage
ms.topic: include
ms.date: 03/05/2020
ms.author: tamram
ms.custom: include file
ms.openlocfilehash: 726fc9cc0e4eb5a8f9bed5cca8dd4d3df588ac7c
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/28/2020
ms.locfileid: "79371546"
---
Azure Storage biedt verschillende soorten opslag accounts. Elk type ondersteunt verschillende functies en heeft een eigen prijs model. Houd rekening met deze verschillen voordat u een opslag account maakt om te bepalen welk type account het meest geschikt is voor uw toepassingen. De typen opslag accounts zijn:

- **V2-accounts voor algemeen gebruik**: basis type opslag account voor blobs, bestanden, wacht rijen en tabellen. Wordt aanbevolen voor de meeste scenario's die gebruikmaken van Azure Storage.
- **Algemeen v1-accounts**: verouderd account type voor blobs, bestanden, wacht rijen en tabellen. Gebruik, indien mogelijk, v2-accounts voor algemeen doel.
- **BlockBlobStorage-accounts**: opslag accounts met Premium-prestatie kenmerken voor blok-blobs en toevoeg-blobs. Aanbevolen voor scenario's met hoge transactie tarieven of scenario's die kleinere objecten gebruiken of een consistente lage opslag latentie vereisen.
- **FileStorage-accounts**: opslag accounts met alleen bestanden met Premium-prestatie kenmerken. Aanbevolen voor schaal toepassingen voor ondernemingen of hoge prestaties.
- **BlobStorage-accounts**: verouderde alleen-Blob Storage-accounts. Gebruik, indien mogelijk, v2-accounts voor algemeen doel.

In de volgende tabel worden de typen opslag accounts en de mogelijkheden ervan beschreven:

| Type opslagaccount | Ondersteunde services                       | Ondersteunde prestatie lagen      | Ondersteunde toegangs lagen         | Replicatieopties               | Implementatiemodel<div role="complementary" aria-labelledby="deployment-model"><sup>1</sup></div> | Versleuteling<div role="complementary" aria-labelledby="encryption"><sup>2</sup></div> |
|----------------------|------------------------------------------|-----------------------------|--------------------------------|-----------------------------------|------------------------------|------------------------|
| Voor algemeen gebruik v2   | BLOB-, bestands-, wachtrij-, tabel-, schijf-en Data Lake Gen2<div role="complementary" aria-labelledby="data-lake-gen2"><sup>6</sup></div>      | Standard, Premium<div role="complementary" aria-labelledby="premium-performance"><sup>5</sup></div> | Hot, cool, Archive<div role="complementary" aria-labelledby="archive"><sup>3</sup></div> | LRS, GRS, RA-GRS, ZRS, GZRS (preview), RA-GZRS (preview-versie)<div role="complementary" aria-labelledby="zone-redundant-storage"><sup>4</sup></div> | Resource Manager             | Versleuteld              |
| Algemeen v1   | BLOB, bestand, wachtrij, tabel en schijf       | Standard, Premium<div role="complementary" aria-labelledby="premium-performance"><sup>5</sup></div> | N.v.t.                            | LRS, GRS, RA-GRS                  | Resource Manager, klassiek    | Versleuteld              |
| BlockBlobStorage   | BLOB (alleen blok-blobs en toevoeg-blobs) | Premium                       | N.v.t.                            | LRS, ZRS<div role="complementary" aria-labelledby="zone-redundant-storage"><sup>4</sup></div>                               | Resource Manager             | Versleuteld              |
| FileStorage   | Alleen bestand | Premium                       | N.v.t.                            | LRS, ZRS<div role="complementary" aria-labelledby="zone-redundant-storage"><sup>4</sup></div>                               | Resource Manager             | Versleuteld              |
| BlobStorage         | BLOB (alleen blok-blobs en toevoeg-blobs) | Standard                      | Hot, cool, Archive<div role="complementary" aria-labelledby="archive"><sup>3</sup></div> | LRS, GRS, RA-GRS                  | Resource Manager             | Versleuteld              |

<div id="deployment-model"><sup>1</sup> U wordt aangeraden het Azure Resource Manager-implementatie model te gebruiken. Opslag accounts die gebruikmaken van het klassieke implementatie model, kunnen nog steeds op sommige locaties worden gemaakt en bestaande klassieke accounts blijven worden ondersteund. Zie <a href="https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-deployment-model">Azure Resource Manager vs. klassieke implementatie: inzicht in implementatie modellen en de status van uw resources</a>voor meer informatie.</div><br/>

<div id="encryption"><sup>2</sup> Alle opslag accounts zijn versleuteld met behulp van Storage Service Encryption (SSE) voor Data-at-rest. Zie <a href="https://docs.microsoft.com/azure/storage/common/storage-service-encryption">Azure Storage-service versleuteling voor Data-at-rest</a>voor meer informatie.</div><br/>

<div id="archive"><sup>3</sup> archief opslag en lagen op BLOB-niveau bieden alleen ondersteuning voor blok-blobs. De archief laag is alleen beschikbaar op het niveau van een afzonderlijke blob, niet op het niveau van het opslag account. Zie <a href="https://docs.microsoft.com/azure/storage/blobs/storage-blob-storage-tiers">Azure Blob Storage: warme, cool en archief opslag lagen</a>voor meer informatie.</div><br/>

<div id="zone-redundant-storage"><sup>4</sup> Zone-redundante opslag (ZRS) en geo-zone-redundante opslag (GZRS/RA-GZRS) (preview) zijn alleen beschikbaar voor standaard v2-, BlockBlobStorage-en FileStorage-accounts voor algemeen gebruik in bepaalde regio's. Zie <a href="https://docs.microsoft.com/azure/storage/common/storage-redundancy">Azure Storage redundantie</a>voor meer informatie over Azure Storage redundantie opties.</div><br/>

<div id="premium-performance"><sup>5</sup> Premium-prestaties voor algemeen gebruik v2 en algemeen v1-accounts zijn alleen beschikbaar voor schijven en pagina-blobs. Premium-prestaties voor blok-of toevoeg-blobs zijn alleen beschikbaar voor BlockBlobStorage-accounts. Premium-prestaties voor bestanden zijn alleen beschikbaar voor FileStorage-accounts.</div><br/>

<div id="data-lake-gen2"><sup>6</sup> Azure Data Lake Storage Gen2 is een set mogelijkheden die is toegewezen aan big data Analytics, gebouwd op Azure Blob-opslag. Data Lake Storage Gen2 wordt alleen ondersteund in v2-opslag accounts voor algemeen gebruik waarvoor een hiërarchische naam ruimte is ingeschakeld. Zie <a href="https://docs.microsoft.com/azure/storage/blobs/data-lake-storage-introduction">Inleiding tot Azure data Lake Storage Gen2</a>voor meer informatie over data Lake Storage Gen2.</div>

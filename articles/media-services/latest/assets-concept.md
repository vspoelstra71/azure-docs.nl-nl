---
title: Activa
titleSuffix: Azure Media Services
description: Meer informatie over wat assets zijn en hoe deze worden gebruikt door Azure Media Services.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: ''
ms.topic: article
ms.date: 03/09/2020
ms.author: juliako
ms.custom: seodec18
ms.openlocfilehash: 9b04941a5799955097fbd54ad9bdf50eccb87541
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/28/2020
ms.locfileid: "79087909"
---
# <a name="assets-in-azure-media-services-v3"></a>Activa in Azure Media Services v3

In Azure Media Services is een [activum](https://docs.microsoft.com/rest/api/media/assets) een kern concept. U voert media in (bijvoorbeeld via upload-of live-opname), uitvoer media (van een taak uitvoer) en het publiceren van media van (voor streaming). 

Een Asset wordt toegewezen aan een BLOB-container in het [Azure Storage-account](storage-account-concept.md) en de bestanden in de Asset worden opgeslagen als blok-blobs in die container. Assets bevatten informatie over digitale bestanden die zijn opgeslagen in Azure Storage (inclusief video, audio, afbeeldingen, miniatuur verzamelingen, tekst sporen en ondertitelings bestanden).

Media Services ondersteunt BLOB-lagen wanneer het account gebruik maakt van de GPv2-opslag (General-Purpose v2). Met GPv2 kunt u bestanden verplaatsen naar [koele of archief opslag](https://docs.microsoft.com/azure/storage/blobs/storage-blob-storage-tiers). **Archief** opslag is geschikt voor het archiveren van bron bestanden wanneer deze niet meer nodig zijn (bijvoorbeeld nadat deze zijn gecodeerd).

De opslaglaag van het **Archief** wordt alleen aanbevolen voor zeer grote bron bestanden die al zijn gecodeerd en de uitvoer van de coderings taak in een uitvoer BLOB-container is geplaatst. De blobs in de uitvoer container die u wilt koppelen aan een activum en gebruiken om uw inhoud te streamen of te analyseren, moeten zich in een **warme** of **koud** opslaglaag bevinden.

## <a name="naming"></a>Naamgeving 

### <a name="assets"></a>Activa

De namen van het activum moeten uniek zijn. Media Services v3-resource namen (bijvoorbeeld assets, taken, trans formaties) zijn onderhevig aan Azure Resource Manager naamgevings beperkingen. Zie [naamgevings conventies](media-services-apis-overview.md#naming-conventions)voor meer informatie.

### <a name="blobs"></a>Blobs

De namen van bestanden/blobs in een Asset moeten de [vereisten voor de BLOB-naam](https://docs.microsoft.com/rest/api/storageservices/Naming-and-Referencing-Containers--Blobs--and-Metadata) en de [NTFS-naam](https://docs.microsoft.com/windows/win32/fileio/naming-a-file)volgen. De reden voor deze vereisten is dat de bestanden kunnen worden gekopieerd van Blob Storage naar een lokale NTFS-schijf voor verwerking.

## <a name="next-steps"></a>Volgende stappen

[Assets in Media Services beheren](manage-asset-concept.md)

## <a name="see-also"></a>Zie ook

[Verschillen tussen Media Services v2 en v3](migrate-from-v2-to-v3.md)

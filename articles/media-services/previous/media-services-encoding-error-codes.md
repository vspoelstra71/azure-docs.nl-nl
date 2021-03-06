---
title: Codeer fout codes Azure Media Services | Microsoft Docs
description: Dit onderwerp bevat een lijst met fout codes die kunnen worden geretourneerd als er een fout is opgetreden tijdens het uitvoeren van de coderings taak.
services: media-services
documentationcenter: ''
author: juliako
manager: femila
editor: ''
ms.assetid: ce4e939f-5aee-41f9-859d-e4429815e9f2
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/18/2019
ms.author: juliako
ms.openlocfilehash: 5c038f0be31acea52c2ef07d43f0dbaf3434a371
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/28/2020
ms.locfileid: "64709525"
---
# <a name="encoding-error-codes"></a>Foutcodes voor codering

De volgende tabel bevat de fout codes die kunnen worden geretourneerd als er een fout is opgetreden tijdens het uitvoeren van de coderings taak.  Als u fout gegevens in uw .NET-code wilt ophalen, gebruikt u de [Error Details](https://msdn.microsoft.com/library/microsoft.windowsazure.mediaservices.client.errordetail.aspx) -klasse. Als u details van de fout in uw REST code wilt weer geven, gebruikt u de [ErrorDetail](https://msdn.microsoft.com/library/jj853026.aspx) -rest API.

| ErrorDetail. code | Mogelijke oorzaken voor fout |
| --- | --- |
| Onbekend |Onbekende fout tijdens het uitvoeren van de taak |
| ErrorDownloadingInputAssetMalformedContent |Categorie fouten die fouten bevatten bij het downloaden van invoer-assets, zoals onjuiste bestands namen, bestanden met een lengte van nul, onjuiste indelingen, enzovoort. |
| ErrorDownloadingInputAssetServiceFailure |Categorie fouten die betrekking hebben op problemen aan de kant van de service, zoals netwerk-of opslag fouten tijdens het downloaden. |
| ErrorParsingConfiguration |Categorie fouten waarbij taak \<Zie CREF = "MediaTask. PrivateData"/> (configuratie) ongeldig is, bijvoorbeeld omdat de configuratie geen geldige systeem instelling is of ongeldige XML bevat. |
| ErrorExecutingTaskMalformedContent |Categorie van fouten tijdens de uitvoering van de taak waarbij problemen in de invoer media bestanden een fout veroorzaken. |
| ErrorExecutingTaskUnsupportedFormat |Categorie fouten waarbij de media processor de opgegeven bestanden niet kan verwerken. de media-indeling wordt niet ondersteund of komt niet overeen met de configuratie. U kunt bijvoorbeeld proberen een alleen audio-uitvoer te maken van een Asset met alleen video |
| ErrorProcessingTask |Categorie van andere fouten die de media processor tegen komt tijdens de verwerking van de taak die niet gerelateerd is aan inhoud. |
| ErrorUploadingOutputAsset |Categorie van fouten bij het uploaden van het uitvoer activum |
| ErrorCancelingTask |Fout met betrekking tot fouten bij het annuleren van de taak |
| TransientError |Categorie van fouten om tijdelijke problemen te bedekken (bijvoorbeeld tijdelijke netwerk problemen met Azure Storage) |

Open een [ondersteunings ticket](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade)om hulp te krijgen van het **Media Services** -team.

## <a name="media-services-learning-paths"></a>Media Services-leertrajecten
[!INCLUDE [media-services-learning-paths-include](../../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Feedback geven
[!INCLUDE [media-services-user-voice-include](../../../includes/media-services-user-voice-include.md)]

## <a name="related-articles"></a>Verwante artikelen:
* [Geavanceerde coderings taken uitvoeren door Media Encoder Standard voor instellingen aan te passen](media-services-custom-mes-presets-with-dotnet.md)
* [Quota en beperkingen](media-services-quotas-and-limitations.md)

<!--Reference links in article-->
[1]: https://azure.microsoft.com/pricing/details/media-services/

---
title: Streaming-beleid in Azure Media Services | Microsoft Docs
description: Dit artikel geeft een uitleg van het streaming-beleid en hoe deze worden gebruikt door Azure Media Services.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: ''
ms.topic: article
ms.date: 05/28/2019
ms.author: juliako
ms.openlocfilehash: 9c80056fd62173ff1e5a6ed3979adba71b7706cc
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/28/2020
ms.locfileid: "80582762"
---
# <a name="streaming-policies"></a>Beleid voor streaming

In Azure Media Services v3 kunt u met [streaming-beleid](https://docs.microsoft.com/rest/api/media/streamingpolicies) streaming-protocollen en versleutelings opties definiëren voor uw [Stream-Locators](streaming-locators-concept.md). Media Services V3 biedt een aantal vooraf gedefinieerde streaming-beleids regels, zodat u ze rechtstreeks kunt gebruiken voor proef-of productie doeleinden. 

Het momenteel beschik bare vooraf gedefinieerde beleid voor streaming:<br/>
* ' Predefined_DownloadOnly '
* ' Predefined_ClearStreamingOnly '
* ' Predefined_DownloadAndClearStreaming '
* ' Predefined_ClearKey '
* ' Predefined_MultiDrmCencStreaming ' 
* ' Predefined_MultiDrmStreaming '

De volgende ' beslissings structuur ' helpt u bij het kiezen van een vooraf gedefinieerd streaming-beleid voor uw scenario.

> [!IMPORTANT]
> * Eigenschappen van **streaming-beleid** van het type datetime zijn altijd in UTC-indeling.
> * U dient een beperkt aantal beleids regels te ontwerpen voor uw media service-account en deze opnieuw te gebruiken voor uw streaming-Locators wanneer dezelfde opties nodig zijn. Zie [quota's en limieten](limits-quotas-constraints.md)voor meer informatie.

## <a name="decision-tree"></a>Beslissingsstructuur

Klik op de afbeelding om deze in volledig formaat weer te geven.  

<a href="./media/streaming-policy/large.png" target="_blank"><img src="./media/streaming-policy/large.png"></a> 

Als u uw inhoud versleutelt, moet u een [beleid voor inhouds sleutels](content-key-policy-concept.md)maken, het **beleid voor inhouds sleutels** is niet nodig voor het wissen van streams of downloaden. 

Als u speciale vereisten hebt (bijvoorbeeld als u verschillende protocollen wilt opgeven, een aangepaste key delivery service wilt gebruiken of een leeg audio spoor wilt gebruiken), kunt u een aangepast streaming-beleid [maken](https://docs.microsoft.com/rest/api/media/streamingpolicies/create) . 

## <a name="get-a-streaming-policy-definition"></a>Een streaming-beleids definitie ophalen  

Als u de definitie van een streaming-beleid wilt zien, gebruikt u [ophalen](https://docs.microsoft.com/rest/api/media/streamingpolicies/get) en geeft u de naam van het beleid op. Bijvoorbeeld:

### <a name="rest"></a>REST

Aanvraag:

```
GET https://management.azure.com/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/contoso/providers/Microsoft.Media/mediaServices/contosomedia/streamingPolicies/clearStreamingPolicy?api-version=2018-07-01
```

Reactie:

```
{
  "name": "clearStreamingPolicy",
  "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/contoso/providers/Microsoft.Media/mediaservices/contosomedia/streamingPolicies/clearStreamingPolicy",
  "type": "Microsoft.Media/mediaservices/streamingPolicies",
  "properties": {
    "created": "2018-08-08T18:29:30.8501486Z",
    "noEncryption": {
      "enabledProtocols": {
        "download": true,
        "dash": true,
        "hls": true,
        "smoothStreaming": true
      }
    }
  }
}
```

## <a name="filtering-ordering-paging"></a>Filteren, ordenen, paginering

Zie [filteren, ordenen, pagineren van Media Services entiteiten](entities-overview.md).

## <a name="next-steps"></a>Volgende stappen

* [Een bestand streamen](stream-files-dotnet-quickstart.md)
* [Gebruik AES-128 dynamische versleuteling en de key delivery service](protect-with-aes128.md)
* [De Digital Rights Management-service gebruiken voor dynamische versleuteling en licentielevering](protect-with-drm.md)

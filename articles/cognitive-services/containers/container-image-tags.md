---
title: Tags voor Cognitive Services-containerinstallatiekopie
titleSuffix: Azure Cognitive Services
description: Een uitgebreide lijst met alle afbeeldings Tags voor de cognitieve service container.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.topic: reference
ms.date: 04/01/2020
ms.author: aahi
ms.openlocfilehash: cabc3d2a0f8eb3a75938d1768bb0085aab528391
ms.sourcegitcommit: bb0afd0df5563cc53f76a642fd8fc709e366568b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/19/2020
ms.locfileid: "83584600"
---
# <a name="azure-cognitive-services-container-image-tags"></a>Tags voor Azure Cognitive Services container installatie kopieën

Azure Cognitive Services biedt veel container installatie kopieën. De container registers en de bijbehorende opslag plaatsen variëren tussen container installatie kopieën. De naam van elke container installatie kopie biedt meerdere labels. Een container afbeeldings code is een mechanisme voor het versie beheer van de container installatie kopie. Dit artikel is bedoeld om te worden gebruikt als een uitgebreide verwijzing voor het weer geven van alle Cognitive Services container installatie kopieën en de beschik bare Tags.

> [!TIP]
> Wanneer u gebruikt [`docker pull`](https://docs.docker.com/engine/reference/commandline/pull/) , moet u zorgvuldig letten op de behuizing van het container register, de opslag plaats, de naam van de container installatie kopie en de bijbehorende tag. Dit zijn **hoofdletter gevoelig**.

## <a name="anomaly-detector"></a>Anomaly Detector

De [afwijkende detector][ad-containers] container installatie kopie vindt u in het `mcr.microsoft.com` container register Syndicate. Deze bevindt zich in de `azure-cognitive-services` opslag plaats en heet `anomaly-detector` . De volledig gekwalificeerde naam van de container installatie kopie is, `mcr.microsoft.com/azure-cognitive-services/anomaly-detector` .

Deze container installatie kopie heeft de volgende tags beschikbaar:

| Afbeeldings Tags                    | Opmerkingen |
|-------------------------------|:------|
| `latest`                      |       |

## <a name="computer-vision"></a>Computer Vision

De [Computer Vision][cv-containers] container installatie kopie vindt u in het `containerpreview.azurecr.io` container register. Deze bevindt zich in de `microsoft` opslag plaats en heet `cognitive-services-read` . De volledig gekwalificeerde naam van de container installatie kopie is, `containerpreview.azurecr.io/microsoft/cognitive-services-read` .

Deze container installatie kopie heeft de volgende tags beschikbaar:

| Afbeeldings Tags                    | Opmerkingen |
|-------------------------------|:------|
| `latest`                      |       |
| `1.1.011580001-amd64-preview` |       |
| `1.1.009920003-amd64-preview` |       |
| `1.1.009910003-amd64-preview` |       |

## <a name="face"></a>Face

De [Face][fa-containers] -container installatie kopie vindt u in het `containerpreview.azurecr.io` container register. Deze bevindt zich in de `microsoft` opslag plaats en heet `cognitive-services-face` . De volledig gekwalificeerde naam van de container installatie kopie is, `containerpreview.azurecr.io/microsoft/cognitive-services-face` .

Deze container installatie kopie heeft de volgende tags beschikbaar:

| Afbeeldings Tags                    | Opmerkingen |
|-------------------------------|:------|
| `latest`                      |       |
| `1.1.009301-amd64-preview`    |       |
| `1.1.008710001-amd64-preview` |       |
| `1.1.007750002-amd64-preview` |       |
| `1.1.007360001-amd64-preview` |       |
| `1.1.006770001-amd64-preview` |       |
| `1.1.006490002-amd64-preview` |       |
| `1.0.005940002-amd64-preview` |       |
| `1.0.005550001-amd64-preview` |       |

## <a name="form-recognizer"></a>Form Recognizer

De container installatie kopie voor de [formulier herkenning][fr-containers] vindt u in het `containerpreview.azurecr.io` container register. Deze bevindt zich in de `microsoft` opslag plaats en heet `cognitive-services-form-recognizer` . De volledig gekwalificeerde naam van de container installatie kopie is, `containerpreview.azurecr.io/microsoft/cognitive-services-form-recognizer` .

Deze container installatie kopie heeft de volgende tags beschikbaar:

| Afbeeldings Tags                    | Opmerkingen |
|-------------------------------|:------|
| `latest`                      |       |
| `1.1.009301-amd64-preview`    |       |
| `1.1.008640001-amd64-preview` |       |
| `1.1.008510001-amd64-preview` |       |

## <a name="language-understanding-luis"></a>Language Understanding (LUIS)

De [Luis][lu-containers] -container installatie kopie vindt u in het `mcr.microsoft.com` container register Syndicate. Deze bevindt zich in de `azure-cognitive-services` opslag plaats en heet `luis` . De volledig gekwalificeerde naam van de container installatie kopie is, `mcr.microsoft.com/azure-cognitive-services/luis` .

Deze container installatie kopie heeft de volgende tags beschikbaar:

| Afbeeldings Tags                    | Opmerkingen |
|-------------------------------|:------|
| `latest`                      |       |
| `1.1.010330004-amd64-preview` |       |
| `1.1.009301-amd64-preview`    |       |
| `1.1.008710001-amd64-preview` |       |
| `1.1.008510001-amd64-preview` |       |
| `1.1.008010002-amd64-preview` |       |
| `1.1.007750002-amd64-preview` |       |
| `1.1.007360001-amd64-preview` |       |
| `1.1.007020001-amd64-preview` |       |

## <a name="custom-speech-to-text"></a>Custom Speech-naar-tekst

De [Custom speech-naar-tekst][sp-cstt] container installatie kopie vindt u in het `containerpreview.azurecr.io` container register. Deze bevindt zich in de `microsoft` opslag plaats en heet `cognitive-services-custom-speech-to-text` . De volledig gekwalificeerde naam van de container installatie kopie is, `containerpreview.azurecr.io/microsoft/cognitive-services-custom-speech-to-text` .

Deze container installatie kopie heeft de volgende tags beschikbaar:

| Afbeeldings Tags            | Opmerkingen |
|-----------------------|:------|
| `latest`              |       |
| `2.2.0-amd64-preview` |       |
| `2.1.1-amd64-preview` |       |
| `2.1.0-amd64-preview` |       |
| `2.0.2-amd64-preview` |       |
| `2.0.0-amd64-preview` |       |

## <a name="custom-text-to-speech"></a>Aangepaste tekst-naar-spraak

De [aangepaste tekst-naar-spraak-][sp-ctts] container installatie kopie vindt u in het `containerpreview.azurecr.io` container register. Deze bevindt zich in de `microsoft` opslag plaats en heet `cognitive-services-custom-text-to-speech` . De volledig gekwalificeerde naam van de container installatie kopie is, `containerpreview.azurecr.io/microsoft/cognitive-services-custom-text-to-speech` .

Deze container installatie kopie heeft de volgende tags beschikbaar:

| Afbeeldings Tags            | Opmerkingen |
|-----------------------|:------|
| `latest`              |       |
| `1.3.0-amd64-preview` |       |

## <a name="speech-to-text"></a>Spraak naar tekst

De afbeelding voor de [spraak-naar-tekst-][sp-stt] container kan worden gevonden in het `containerpreview.azurecr.io` container register. Deze bevindt zich in de `microsoft` opslag plaats en heet `cognitive-services-speech-to-text` . De volledig gekwalificeerde naam van de container installatie kopie is, `containerpreview.azurecr.io/microsoft/cognitive-services-speech-to-text` .

Deze container installatie kopie heeft de volgende tags beschikbaar:

| Afbeeldings Tags                  | Opmerkingen                                    |
|-----------------------------|:-----------------------------------------|
| `latest`                    | Container installatie kopie met de `en-US` land instelling. |
| `2.2.0-amd64-ar-ae-preview` | Container installatie kopie met de `ar-AE` land instelling. |
| `2.2.0-amd64-ar-eg-preview` | Container installatie kopie met de `ar-EG` land instelling. |
| `2.2.0-amd64-ar-kw-preview` | Container installatie kopie met de `ar-KW` land instelling. |
| `2.2.0-amd64-ar-qa-preview` | Container installatie kopie met de `ar-QA` land instelling. |
| `2.2.0-amd64-ar-sa-preview` | Container installatie kopie met de `ar-SA` land instelling. |
| `2.2.0-amd64-ca-es-preview` | Container installatie kopie met de `ca-ES` land instelling. |
| `2.2.0-amd64-da-dk-preview` | Container installatie kopie met de `da-DK` land instelling. |
| `2.2.0-amd64-de-de-preview` | Container installatie kopie met de `de-DE` land instelling. |
| `2.2.0-amd64-en-au-preview` | Container installatie kopie met de `en-AU` land instelling. |
| `2.2.0-amd64-en-ca-preview` | Container installatie kopie met de `en-CA` land instelling. |
| `2.2.0-amd64-en-gb-preview` | Container installatie kopie met de `en-GB` land instelling. |
| `2.2.0-amd64-en-in-preview` | Container installatie kopie met de `en-IN` land instelling. |
| `2.2.0-amd64-en-nz-preview` | Container installatie kopie met de `en-NZ` land instelling. |
| `2.2.0-amd64-en-us-preview` | Container installatie kopie met de `en-US` land instelling. |
| `2.2.0-amd64-es-es-preview` | Container installatie kopie met de `es-ES` land instelling. |
| `2.2.0-amd64-es-mx-preview` | Container installatie kopie met de `es-MX` land instelling. |
| `2.2.0-amd64-fi-fi-preview` | Container installatie kopie met de `fi-FI` land instelling. |
| `2.2.0-amd64-fr-ca-preview` | Container installatie kopie met de `fr-CA` land instelling. |
| `2.2.0-amd64-fr-fr-preview` | Container installatie kopie met de `fr-FR` land instelling. |
| `2.2.0-amd64-gu-in-preview` | Container installatie kopie met de `gu-IN` land instelling. |
| `2.2.0-amd64-hi-in-preview` | Container installatie kopie met de `hi-IN` land instelling. |
| `2.2.0-amd64-it-it-preview` | Container installatie kopie met de `it-IT` land instelling. |
| `2.2.0-amd64-ja-jp-preview` | Container installatie kopie met de `ja-JP` land instelling. |
| `2.2.0-amd64-ko-kr-preview` | Container installatie kopie met de `ko-KR` land instelling. |
| `2.2.0-amd64-mr-in-preview` | Container installatie kopie met de `mr-IN` land instelling. |
| `2.2.0-amd64-nb-no-preview` | Container installatie kopie met de `nb-NO` land instelling. |
| `2.2.0-amd64-nl-nl-preview` | Container installatie kopie met de `nl-NL` land instelling. |
| `2.2.0-amd64-pl-pl-preview` | Container installatie kopie met de `pl-PL` land instelling. |
| `2.2.0-amd64-pt-br-preview` | Container installatie kopie met de `pt-BR` land instelling. |
| `2.2.0-amd64-pt-pt-preview` | Container installatie kopie met de `pt-PT` land instelling. |
| `2.2.0-amd64-ru-ru-preview` | Container installatie kopie met de `ru-RU` land instelling. |
| `2.2.0-amd64-sv-se-preview` | Container installatie kopie met de `sv-SE` land instelling. |
| `2.2.0-amd64-ta-in-preview` | Container installatie kopie met de `ta-IN` land instelling. |
| `2.2.0-amd64-te-in-preview` | Container installatie kopie met de `te-IN` land instelling. |
| `2.2.0-amd64-th-th-preview` | Container installatie kopie met de `th-TH` land instelling. |
| `2.2.0-amd64-tr-tr-preview` | Container installatie kopie met de `tr-TR` land instelling. |
| `2.2.0-amd64-zh-cn-preview` | Container installatie kopie met de `zh-CN` land instelling. |
| `2.2.0-amd64-zh-hk-preview` | Container installatie kopie met de `zh-HK` land instelling. |
| `2.2.0-amd64-zh-tw-preview` | Container installatie kopie met de `zh-TW` land instelling. |
| `2.1.1-amd64-en-us-preview` | Container installatie kopie met de `en-US` land instelling. |
| `2.1.1-amd64-ar-ae-preview` | Container installatie kopie met de `ar-AE` land instelling. |
| `2.1.1-amd64-ar-eg-preview` | Container installatie kopie met de `ar-EG` land instelling. |
| `2.1.1-amd64-ar-kw-preview` | Container installatie kopie met de `ar-KW` land instelling. |
| `2.1.1-amd64-ar-qa-preview` | Container installatie kopie met de `ar-QA` land instelling. |
| `2.1.1-amd64-ar-sa-preview` | Container installatie kopie met de `ar-SA` land instelling. |
| `2.1.1-amd64-ca-es-preview` | Container installatie kopie met de `ca-ES` land instelling. |
| `2.1.1-amd64-da-dk-preview` | Container installatie kopie met de `da-DK` land instelling. |
| `2.1.1-amd64-de-de-preview` | Container installatie kopie met de `de-DE` land instelling. |
| `2.1.1-amd64-en-au-preview` | Container installatie kopie met de `en-AU` land instelling. |
| `2.1.1-amd64-en-ca-preview` | Container installatie kopie met de `en-CA` land instelling. |
| `2.1.1-amd64-en-gb-preview` | Container installatie kopie met de `en-GB` land instelling. |
| `2.1.1-amd64-en-in-preview` | Container installatie kopie met de `en-IN` land instelling. |
| `2.1.1-amd64-en-nz-preview` | Container installatie kopie met de `en-NZ` land instelling. |
| `2.1.1-amd64-en-us-preview` | Container installatie kopie met de `en-US` land instelling. |
| `2.1.1-amd64-es-es-preview` | Container installatie kopie met de `es-ES` land instelling. |
| `2.1.1-amd64-es-mx-preview` | Container installatie kopie met de `es-MX` land instelling. |
| `2.1.1-amd64-fi-fi-preview` | Container installatie kopie met de `fi-FI` land instelling. |
| `2.1.1-amd64-fr-ca-preview` | Container installatie kopie met de `fr-CA` land instelling. |
| `2.1.1-amd64-fr-fr-preview` | Container installatie kopie met de `fr-FR` land instelling. |
| `2.1.1-amd64-gu-in-preview` | Container installatie kopie met de `gu-IN` land instelling. |
| `2.1.1-amd64-hi-in-preview` | Container installatie kopie met de `hi-IN` land instelling. |
| `2.1.1-amd64-it-it-preview` | Container installatie kopie met de `it-IT` land instelling. |
| `2.1.1-amd64-ja-jp-preview` | Container installatie kopie met de `ja-JP` land instelling. |
| `2.1.1-amd64-ko-kr-preview` | Container installatie kopie met de `ko-KR` land instelling. |
| `2.1.1-amd64-mr-in-preview` | Container installatie kopie met de `mr-IN` land instelling. |
| `2.1.1-amd64-nb-no-preview` | Container installatie kopie met de `nb-NO` land instelling. |
| `2.1.1-amd64-nl-nl-preview` | Container installatie kopie met de `nl-NL` land instelling. |
| `2.1.1-amd64-pl-pl-preview` | Container installatie kopie met de `pl-PL` land instelling. |
| `2.1.1-amd64-pt-br-preview` | Container installatie kopie met de `pt-BR` land instelling. |
| `2.1.1-amd64-pt-pt-preview` | Container installatie kopie met de `pt-PT` land instelling. |
| `2.1.1-amd64-ru-ru-preview` | Container installatie kopie met de `ru-RU` land instelling. |
| `2.1.1-amd64-sv-se-preview` | Container installatie kopie met de `sv-SE` land instelling. |
| `2.1.1-amd64-ta-in-preview` | Container installatie kopie met de `ta-IN` land instelling. |
| `2.1.1-amd64-te-in-preview` | Container installatie kopie met de `te-IN` land instelling. |
| `2.1.1-amd64-th-th-preview` | Container installatie kopie met de `th-TH` land instelling. |
| `2.1.1-amd64-tr-tr-preview` | Container installatie kopie met de `tr-TR` land instelling. |
| `2.1.1-amd64-zh-cn-preview` | Container installatie kopie met de `zh-CN` land instelling. |
| `2.1.1-amd64-zh-hk-preview` | Container installatie kopie met de `zh-HK` land instelling. |
| `2.1.1-amd64-zh-tw-preview` | Container installatie kopie met de `zh-TW` land instelling. |
| `2.1.0-amd64-ar-ae-preview` | Container installatie kopie met de `ar-AE` land instelling. |
| `2.1.0-amd64-ar-eg-preview` | Container installatie kopie met de `ar-EG` land instelling. |
| `2.1.0-amd64-ar-kw-preview` | Container installatie kopie met de `ar-KW` land instelling. |
| `2.1.0-amd64-ar-qa-preview` | Container installatie kopie met de `ar-QA` land instelling. |
| `2.1.0-amd64-ar-sa-preview` | Container installatie kopie met de `ar-SA` land instelling. |
| `2.1.0-amd64-ca-es-preview` | Container installatie kopie met de `ca-ES` land instelling. |
| `2.1.0-amd64-da-dk-preview` | Container installatie kopie met de `da-DK` land instelling. |
| `2.1.0-amd64-de-de-preview` | Container installatie kopie met de `de-DE` land instelling. |
| `2.1.0-amd64-en-au-preview` | Container installatie kopie met de `en-AU` land instelling. |
| `2.1.0-amd64-en-ca-preview` | Container installatie kopie met de `en-CA` land instelling. |
| `2.1.0-amd64-en-gb-preview` | Container installatie kopie met de `en-GB` land instelling. |
| `2.1.0-amd64-en-in-preview` | Container installatie kopie met de `en-IN` land instelling. |
| `2.1.0-amd64-en-nz-preview` | Container installatie kopie met de `en-NZ` land instelling. |
| `2.1.0-amd64-en-us-preview` | Container installatie kopie met de `en-US` land instelling. |
| `2.1.0-amd64-es-es-preview` | Container installatie kopie met de `es-ES` land instelling. |
| `2.1.0-amd64-es-mx-preview` | Container installatie kopie met de `es-MX` land instelling. |
| `2.1.0-amd64-fi-fi-preview` | Container installatie kopie met de `fi-FI` land instelling. |
| `2.1.0-amd64-fr-ca-preview` | Container installatie kopie met de `fr-CA` land instelling. |
| `2.1.0-amd64-fr-fr-preview` | Container installatie kopie met de `fr-FR` land instelling. |
| `2.1.0-amd64-gu-in-preview` | Container installatie kopie met de `gu-IN` land instelling. |
| `2.1.0-amd64-hi-in-preview` | Container installatie kopie met de `hi-IN` land instelling. |
| `2.1.0-amd64-it-it-preview` | Container installatie kopie met de `it-IT` land instelling. |
| `2.1.0-amd64-ja-jp-preview` | Container installatie kopie met de `ja-JP` land instelling. |
| `2.1.0-amd64-ko-kr-preview` | Container installatie kopie met de `ko-KR` land instelling. |
| `2.1.0-amd64-mr-in-preview` | Container installatie kopie met de `mr-IN` land instelling. |
| `2.1.0-amd64-nb-no-preview` | Container installatie kopie met de `nb-NO` land instelling. |
| `2.1.0-amd64-nl-nl-preview` | Container installatie kopie met de `nl-NL` land instelling. |
| `2.1.0-amd64-pl-pl-preview` | Container installatie kopie met de `pl-PL` land instelling. |
| `2.1.0-amd64-pt-br-preview` | Container installatie kopie met de `pt-BR` land instelling. |
| `2.1.0-amd64-pt-pt-preview` | Container installatie kopie met de `pt-PT` land instelling. |
| `2.1.0-amd64-ru-ru-preview` | Container installatie kopie met de `ru-RU` land instelling. |
| `2.1.0-amd64-sv-se-preview` | Container installatie kopie met de `sv-SE` land instelling. |
| `2.1.0-amd64-ta-in-preview` | Container installatie kopie met de `ta-IN` land instelling. |
| `2.1.0-amd64-te-in-preview` | Container installatie kopie met de `te-IN` land instelling. |
| `2.1.0-amd64-th-th-preview` | Container installatie kopie met de `th-TH` land instelling. |
| `2.1.0-amd64-tr-tr-preview` | Container installatie kopie met de `tr-TR` land instelling. |
| `2.1.0-amd64-zh-cn-preview` | Container installatie kopie met de `zh-CN` land instelling. |
| `2.1.0-amd64-zh-hk-preview` | Container installatie kopie met de `zh-HK` land instelling. |
| `2.1.0-amd64-zh-tw-preview` | Container installatie kopie met de `zh-TW` land instelling. |
| `2.0.3-amd64-ja-jp-preview` | Container installatie kopie met de `ja-JP` land instelling. |
| `2.0.2-amd64-ar-ae-preview` | Container installatie kopie met de `ar-AE` land instelling. |
| `2.0.2-amd64-ar-eg-preview` | Container installatie kopie met de `ar-EG` land instelling. |
| `2.0.2-amd64-ar-kw-preview` | Container installatie kopie met de `ar-KW` land instelling. |
| `2.0.2-amd64-ar-qa-preview` | Container installatie kopie met de `ar-QA` land instelling. |
| `2.0.2-amd64-ar-sa-preview` | Container installatie kopie met de `ar-SA` land instelling. |
| `2.0.2-amd64-ca-es-preview` | Container installatie kopie met de `ca-ES` land instelling. |
| `2.0.2-amd64-da-dk-preview` | Container installatie kopie met de `da-DK` land instelling. |
| `2.0.2-amd64-de-de-preview` | Container installatie kopie met de `de-DE` land instelling. |
| `2.0.2-amd64-en-au-preview` | Container installatie kopie met de `en-AU` land instelling. |
| `2.0.2-amd64-en-ca-preview` | Container installatie kopie met de `en-CA` land instelling. |
| `2.0.2-amd64-en-gb-preview` | Container installatie kopie met de `en-GB` land instelling. |
| `2.0.2-amd64-en-in-preview` | Container installatie kopie met de `en-IN` land instelling. |
| `2.0.2-amd64-en-nz-preview` | Container installatie kopie met de `en-NZ` land instelling. |
| `2.0.2-amd64-en-us-preview` | Container installatie kopie met de `en-US` land instelling. |
| `2.0.2-amd64-es-es-preview` | Container installatie kopie met de `es-ES` land instelling. |
| `2.0.2-amd64-es-mx-preview` | Container installatie kopie met de `es-MX` land instelling. |
| `2.0.2-amd64-fi-fi-preview` | Container installatie kopie met de `fi-FI` land instelling. |
| `2.0.2-amd64-fr-ca-preview` | Container installatie kopie met de `fr-CA` land instelling. |
| `2.0.2-amd64-fr-fr-preview` | Container installatie kopie met de `fr-FR` land instelling. |
| `2.0.2-amd64-gu-in-preview` | Container installatie kopie met de `gu-IN` land instelling. |
| `2.0.2-amd64-hi-in-preview` | Container installatie kopie met de `hi-IN` land instelling. |
| `2.0.2-amd64-it-it-preview` | Container installatie kopie met de `it-IT` land instelling. |
| `2.0.2-amd64-ja-jp-preview` | Container installatie kopie met de `ja-JP` land instelling. |
| `2.0.2-amd64-ko-kr-preview` | Container installatie kopie met de `ko-KR` land instelling. |
| `2.0.2-amd64-mr-in-preview` | Container installatie kopie met de `mr-IN` land instelling. |
| `2.0.2-amd64-nb-no-preview` | Container installatie kopie met de `nb-NO` land instelling. |
| `2.0.2-amd64-nl-nl-preview` | Container installatie kopie met de `nl-NL` land instelling. |
| `2.0.2-amd64-pl-pl-preview` | Container installatie kopie met de `pl-PL` land instelling. |
| `2.0.2-amd64-pt-br-preview` | Container installatie kopie met de `pt-BR` land instelling. |
| `2.0.2-amd64-pt-pt-preview` | Container installatie kopie met de `pt-PT` land instelling. |
| `2.0.2-amd64-ru-ru-preview` | Container installatie kopie met de `ru-RU` land instelling. |
| `2.0.2-amd64-sv-se-preview` | Container installatie kopie met de `sv-SE` land instelling. |
| `2.0.2-amd64-ta-in-preview` | Container installatie kopie met de `ta-IN` land instelling. |
| `2.0.2-amd64-te-in-preview` | Container installatie kopie met de `te-IN` land instelling. |
| `2.0.2-amd64-th-th-preview` | Container installatie kopie met de `th-TH` land instelling. |
| `2.0.2-amd64-tr-tr-preview` | Container installatie kopie met de `tr-TR` land instelling. |
| `2.0.2-amd64-zh-cn-preview` | Container installatie kopie met de `zh-CN` land instelling. |
| `2.0.2-amd64-zh-hk-preview` | Container installatie kopie met de `zh-HK` land instelling. |
| `2.0.2-amd64-zh-tw-preview` | Container installatie kopie met de `zh-TW` land instelling. |
| `2.0.1-amd64-ar-ae-preview` | Container installatie kopie met de `ar-AE` land instelling. |
| `2.0.1-amd64-ar-eg-preview` | Container installatie kopie met de `ar-EG` land instelling. |
| `2.0.1-amd64-ar-kw-preview` | Container installatie kopie met de `ar-KW` land instelling. |
| `2.0.1-amd64-ar-qa-preview` | Container installatie kopie met de `ar-QA` land instelling. |
| `2.0.1-amd64-ar-sa-preview` | Container installatie kopie met de `ar-SA` land instelling. |
| `2.0.1-amd64-ca-es-preview` | Container installatie kopie met de `ca-ES` land instelling. |
| `2.0.1-amd64-da-dk-preview` | Container installatie kopie met de `da-DK` land instelling. |
| `2.0.1-amd64-de-de-preview` | Container installatie kopie met de `de-DE` land instelling. |
| `2.0.1-amd64-en-au-preview` | Container installatie kopie met de `en-AU` land instelling. |
| `2.0.1-amd64-en-ca-preview` | Container installatie kopie met de `en-CA` land instelling. |
| `2.0.1-amd64-en-gb-preview` | Container installatie kopie met de `en-GB` land instelling. |
| `2.0.1-amd64-en-in-preview` | Container installatie kopie met de `en-IN` land instelling. |
| `2.0.1-amd64-en-nz-preview` | Container installatie kopie met de `en-NZ` land instelling. |
| `2.0.1-amd64-en-us-preview` | Container installatie kopie met de `en-US` land instelling. |
| `2.0.1-amd64-es-es-preview` | Container installatie kopie met de `es-ES` land instelling. |
| `2.0.1-amd64-es-mx-preview` | Container installatie kopie met de `es-MX` land instelling. |
| `2.0.1-amd64-fi-fi-preview` | Container installatie kopie met de `fi-FI` land instelling. |
| `2.0.1-amd64-fr-ca-preview` | Container installatie kopie met de `fr-CA` land instelling. |
| `2.0.1-amd64-fr-fr-preview` | Container installatie kopie met de `fr-FR` land instelling. |
| `2.0.1-amd64-gu-in-preview` | Container installatie kopie met de `gu-IN` land instelling. |
| `2.0.1-amd64-hi-in-preview` | Container installatie kopie met de `hi-IN` land instelling. |
| `2.0.1-amd64-it-it-preview` | Container installatie kopie met de `it-IT` land instelling. |
| `2.0.1-amd64-ja-jp-preview` | Container installatie kopie met de `ja-JP` land instelling. |
| `2.0.1-amd64-ko-kr-preview` | Container installatie kopie met de `ko-KR` land instelling. |
| `2.0.1-amd64-mr-in-preview` | Container installatie kopie met de `mr-IN` land instelling. |
| `2.0.1-amd64-nb-no-preview` | Container installatie kopie met de `nb-NO` land instelling. |
| `2.0.1-amd64-nl-nl-preview` | Container installatie kopie met de `nl-NL` land instelling. |
| `2.0.1-amd64-pl-pl-preview` | Container installatie kopie met de `pl-PL` land instelling. |
| `2.0.1-amd64-pt-br-preview` | Container installatie kopie met de `pt-BR` land instelling. |
| `2.0.1-amd64-pt-pt-preview` | Container installatie kopie met de `pt-PT` land instelling. |
| `2.0.1-amd64-ru-ru-preview` | Container installatie kopie met de `ru-RU` land instelling. |
| `2.0.1-amd64-sv-se-preview` | Container installatie kopie met de `sv-SE` land instelling. |
| `2.0.1-amd64-ta-in-preview` | Container installatie kopie met de `ta-IN` land instelling. |
| `2.0.1-amd64-te-in-preview` | Container installatie kopie met de `te-IN` land instelling. |
| `2.0.1-amd64-th-th-preview` | Container installatie kopie met de `th-TH` land instelling. |
| `2.0.1-amd64-tr-tr-preview` | Container installatie kopie met de `tr-TR` land instelling. |
| `2.0.1-amd64-zh-cn-preview` | Container installatie kopie met de `zh-CN` land instelling. |
| `2.0.1-amd64-zh-hk-preview` | Container installatie kopie met de `zh-HK` land instelling. |
| `2.0.1-amd64-zh-tw-preview` | Container installatie kopie met de `zh-TW` land instelling. |
| `2.0.0-amd64-ar-eg-preview` | Container installatie kopie met de `ar-EG` land instelling. |
| `2.0.0-amd64-ca-es-preview` | Container installatie kopie met de `ca-ES` land instelling. |
| `2.0.0-amd64-da-dk-preview` | Container installatie kopie met de `da-DK` land instelling. |
| `2.0.0-amd64-de-de-preview` | Container installatie kopie met de `de-DE` land instelling. |
| `2.0.0-amd64-en-au-preview` | Container installatie kopie met de `en-AU` land instelling. |
| `2.0.0-amd64-en-ca-preview` | Container installatie kopie met de `en-CA` land instelling. |
| `2.0.0-amd64-en-gb-preview` | Container installatie kopie met de `en-GB` land instelling. |
| `2.0.0-amd64-en-in-preview` | Container installatie kopie met de `en-IN` land instelling. |
| `2.0.0-amd64-en-nz-preview` | Container installatie kopie met de `en-NZ` land instelling. |
| `2.0.0-amd64-en-us-preview` | Container installatie kopie met de `en-US` land instelling. |
| `2.0.0-amd64-es-es-preview` | Container installatie kopie met de `es-ES` land instelling. |
| `2.0.0-amd64-es-mx-preview` | Container installatie kopie met de `es-MX` land instelling. |
| `2.0.0-amd64-fi-fi-preview` | Container installatie kopie met de `fi-FI` land instelling. |
| `2.0.0-amd64-fr-ca-preview` | Container installatie kopie met de `fr-CA` land instelling. |
| `2.0.0-amd64-fr-fr-preview` | Container installatie kopie met de `fr-FR` land instelling. |
| `2.0.0-amd64-hi-in-preview` | Container installatie kopie met de `hi-IN` land instelling. |
| `2.0.0-amd64-it-it-preview` | Container installatie kopie met de `it-IT` land instelling. |
| `2.0.0-amd64-ja-jp-preview` | Container installatie kopie met de `ja-JP` land instelling. |
| `2.0.0-amd64-ko-kr-preview` | Container installatie kopie met de `ko-KR` land instelling. |
| `2.0.0-amd64-nb-no-preview` | Container installatie kopie met de `nb-NO` land instelling. |
| `2.0.0-amd64-nl-nl-preview` | Container installatie kopie met de `nl-NL` land instelling. |
| `2.0.0-amd64-pl-pl-preview` | Container installatie kopie met de `pl-PL` land instelling. |
| `2.0.0-amd64-pt-br-preview` | Container installatie kopie met de `pt-BR` land instelling. |
| `2.0.0-amd64-pt-pt-preview` | Container installatie kopie met de `pt-PT` land instelling. |
| `2.0.0-amd64-ru-ru-preview` | Container installatie kopie met de `ru-RU` land instelling. |
| `2.0.0-amd64-sv-se-preview` | Container installatie kopie met de `sv-SE` land instelling. |
| `2.0.0-amd64-th-th-preview` | Container installatie kopie met de `th-TH` land instelling. |
| `2.0.0-amd64-tr-tr-preview` | Container installatie kopie met de `tr-TR` land instelling. |
| `2.0.0-amd64-zh-cn-preview` | Container installatie kopie met de `zh-CN` land instelling. |
| `2.0.0-amd64-zh-hk-preview` | Container installatie kopie met de `zh-HK` land instelling. |
| `2.0.0-amd64-zh-tw-preview` | Container installatie kopie met de `zh-TW` land instelling. |
| `1.2.0-amd64-de-de-preview` | Container installatie kopie met de `de-DE` land instelling. |
| `1.2.0-amd64-en-au-preview` | Container installatie kopie met de `en-AU` land instelling. |
| `1.2.0-amd64-en-ca-preview` | Container installatie kopie met de `en-CA` land instelling. |
| `1.2.0-amd64-en-gb-preview` | Container installatie kopie met de `en-GB` land instelling. |
| `1.2.0-amd64-en-in-preview` | Container installatie kopie met de `en-IN` land instelling. |
| `1.2.0-amd64-en-us-preview` | Container installatie kopie met de `en-US` land instelling. |
| `1.2.0-amd64-es-es-preview` | Container installatie kopie met de `es-ES` land instelling. |
| `1.2.0-amd64-es-mx-preview` | Container installatie kopie met de `es-MX` land instelling. |
| `1.2.0-amd64-fr-ca-preview` | Container installatie kopie met de `fr-CA` land instelling. |
| `1.2.0-amd64-fr-fr-preview` | Container installatie kopie met de `fr-FR` land instelling. |
| `1.2.0-amd64-it-it-preview` | Container installatie kopie met de `it-IT` land instelling. |
| `1.2.0-amd64-ja-jp-preview` | Container installatie kopie met de `ja-JP` land instelling. |
| `1.2.0-amd64-pt-br-preview` | Container installatie kopie met de `pt-BR` land instelling. |
| `1.2.0-amd64-zh-cn-preview` | Container installatie kopie met de `zh-CN` land instelling. |
| `1.1.3-amd64-de-de-preview` | Container installatie kopie met de `de-DE` land instelling. |
| `1.1.3-amd64-en-au-preview` | Container installatie kopie met de `en-AU` land instelling. |
| `1.1.3-amd64-en-ca-preview` | Container installatie kopie met de `en-CA` land instelling. |
| `1.1.3-amd64-en-gb-preview` | Container installatie kopie met de `en-GB` land instelling. |
| `1.1.3-amd64-en-in-preview` | Container installatie kopie met de `en-IN` land instelling. |
| `1.1.3-amd64-en-us-preview` | Container installatie kopie met de `en-US` land instelling. |
| `1.1.3-amd64-es-es-preview` | Container installatie kopie met de `es-ES` land instelling. |
| `1.1.3-amd64-es-mx-preview` | Container installatie kopie met de `es-MX` land instelling. |
| `1.1.3-amd64-fr-ca-preview` | Container installatie kopie met de `fr-CA` land instelling. |
| `1.1.3-amd64-fr-fr-preview` | Container installatie kopie met de `fr-FR` land instelling. |
| `1.1.3-amd64-it-it-preview` | Container installatie kopie met de `it-IT` land instelling. |
| `1.1.3-amd64-ja-jp-preview` | Container installatie kopie met de `ja-JP` land instelling. |
| `1.1.3-amd64-pt-br-preview` | Container installatie kopie met de `pt-BR` land instelling. |
| `1.1.3-amd64-zh-cn-preview` | Container installatie kopie met de `zh-CN` land instelling. |
| `1.1.2-amd64-de-de-preview` | Container installatie kopie met de `de-DE` land instelling. |
| `1.1.2-amd64-en-au-preview` | Container installatie kopie met de `en-AU` land instelling. |
| `1.1.2-amd64-en-ca-preview` | Container installatie kopie met de `en-CA` land instelling. |
| `1.1.2-amd64-en-gb-preview` | Container installatie kopie met de `en-GB` land instelling. |
| `1.1.2-amd64-en-in-preview` | Container installatie kopie met de `en-IN` land instelling. |
| `1.1.2-amd64-en-us-preview` | Container installatie kopie met de `en-US` land instelling. |
| `1.1.2-amd64-es-es-preview` | Container installatie kopie met de `es-ES` land instelling. |
| `1.1.2-amd64-es-mx-preview` | Container installatie kopie met de `es-MX` land instelling. |
| `1.1.2-amd64-fr-ca-preview` | Container installatie kopie met de `fr-CA` land instelling. |
| `1.1.2-amd64-fr-fr-preview` | Container installatie kopie met de `fr-FR` land instelling. |
| `1.1.2-amd64-it-it-preview` | Container installatie kopie met de `it-IT` land instelling. |
| `1.1.2-amd64-ja-jp-preview` | Container installatie kopie met de `ja-JP` land instelling. |
| `1.1.2-amd64-pt-br-preview` | Container installatie kopie met de `pt-BR` land instelling. |
| `1.1.2-amd64-zh-cn-preview` | Container installatie kopie met de `zh-CN` land instelling. |
| `1.1.1-amd64-de-de-preview` | Container installatie kopie met de `de-DE` land instelling. |
| `1.1.1-amd64-en-au-preview` | Container installatie kopie met de `en-AU` land instelling. |
| `1.1.1-amd64-en-ca-preview` | Container installatie kopie met de `en-CA` land instelling. |
| `1.1.1-amd64-en-gb-preview` | Container installatie kopie met de `en-GB` land instelling. |
| `1.1.1-amd64-en-in-preview` | Container installatie kopie met de `en-IN` land instelling. |
| `1.1.1-amd64-en-us-preview` | Container installatie kopie met de `en-US` land instelling. |
| `1.1.1-amd64-es-es-preview` | Container installatie kopie met de `es-ES` land instelling. |
| `1.1.1-amd64-es-mx-preview` | Container installatie kopie met de `es-MX` land instelling. |
| `1.1.1-amd64-fr-ca-preview` | Container installatie kopie met de `fr-CA` land instelling. |
| `1.1.1-amd64-fr-fr-preview` | Container installatie kopie met de `fr-FR` land instelling. |
| `1.1.1-amd64-it-it-preview` | Container installatie kopie met de `it-IT` land instelling. |
| `1.1.1-amd64-ja-jp-preview` | Container installatie kopie met de `ja-JP` land instelling. |
| `1.1.1-amd64-pt-br-preview` | Container installatie kopie met de `pt-BR` land instelling. |
| `1.1.1-amd64-zh-cn-preview` | Container installatie kopie met de `zh-CN` land instelling. |
| `1.1.0-amd64-de-de-preview` | Container installatie kopie met de `de-DE` land instelling. |
| `1.1.0-amd64-en-au-preview` | Container installatie kopie met de `en-AU` land instelling. |
| `1.1.0-amd64-en-ca-preview` | Container installatie kopie met de `en-CA` land instelling. |
| `1.1.0-amd64-en-gb-preview` | Container installatie kopie met de `en-GB` land instelling. |
| `1.1.0-amd64-en-in-preview` | Container installatie kopie met de `en-IN` land instelling. |
| `1.1.0-amd64-en-us-preview` | Container installatie kopie met de `en-US` land instelling. |
| `1.1.0-amd64-es-es-preview` | Container installatie kopie met de `es-ES` land instelling. |
| `1.1.0-amd64-es-mx-preview` | Container installatie kopie met de `es-MX` land instelling. |
| `1.1.0-amd64-fr-ca-preview` | Container installatie kopie met de `fr-CA` land instelling. |
| `1.1.0-amd64-fr-fr-preview` | Container installatie kopie met de `fr-FR` land instelling. |
| `1.1.0-amd64-it-it-preview` | Container installatie kopie met de `it-IT` land instelling. |
| `1.1.0-amd64-ja-jp-preview` | Container installatie kopie met de `ja-JP` land instelling. |
| `1.1.0-amd64-pt-br-preview` | Container installatie kopie met de `pt-BR` land instelling. |
| `1.1.0-amd64-zh-cn-preview` | Container installatie kopie met de `zh-CN` land instelling. |
| `1.0.0-amd64-de-de-preview` | Container installatie kopie met de `de-DE` land instelling. |
| `1.0.0-amd64-en-au-preview` | Container installatie kopie met de `en-AU` land instelling. |
| `1.0.0-amd64-en-ca-preview` | Container installatie kopie met de `en-CA` land instelling. |
| `1.0.0-amd64-en-gb-preview` | Container installatie kopie met de `en-GB` land instelling. |
| `1.0.0-amd64-en-in-preview` | Container installatie kopie met de `en-IN` land instelling. |
| `1.0.0-amd64-en-us-preview` | Container installatie kopie met de `en-US` land instelling. |
| `1.0.0-amd64-es-es-preview` | Container installatie kopie met de `es-ES` land instelling. |
| `1.0.0-amd64-es-mx-preview` | Container installatie kopie met de `es-MX` land instelling. |
| `1.0.0-amd64-fr-ca-preview` | Container installatie kopie met de `fr-CA` land instelling. |
| `1.0.0-amd64-fr-fr-preview` | Container installatie kopie met de `fr-FR` land instelling. |
| `1.0.0-amd64-it-it-preview` | Container installatie kopie met de `it-IT` land instelling. |
| `1.0.0-amd64-ja-jp-preview` | Container installatie kopie met de `ja-JP` land instelling. |
| `1.0.0-amd64-pt-br-preview` | Container installatie kopie met de `pt-BR` land instelling. |
| `1.0.0-amd64-zh-cn-preview` | Container installatie kopie met de `zh-CN` land instelling. |

## <a name="text-to-speech"></a>Tekst naar spraak

De [tekst-naar-spraak][sp-tts] container installatie kopie vindt u in het `containerpreview.azurecr.io` container register. Deze bevindt zich in de `microsoft` opslag plaats en heet `cognitive-services-text-to-speech` . De volledig gekwalificeerde naam van de container installatie kopie is, `containerpreview.azurecr.io/microsoft/cognitive-services-text-to-speech` .

Deze container installatie kopie heeft de volgende tags beschikbaar:

| Afbeeldings Tags                                  | Opmerkingen                                                                      |
|---------------------------------------------|:---------------------------------------------------------------------------|
| `latest`                                    | Container installatie kopie met de `en-US` land instellingen en de `en-US-JessaRUS` stem.        |
| `1.3.0-amd64-ar-eg-hoda-preview`            | Container installatie kopie met de `ar-EG` land instellingen en de `ar-EG-Hoda` stem.            |
| `1.3.0-amd64-ar-sa-naayf-preview`           | Container installatie kopie met de `ar-SA` land instellingen en de `ar-SA-Naayf` stem.           |
| `1.3.0-amd64-bg-bg-ivan-preview`            | Container installatie kopie met de `bg-BG` land instellingen en de `bg-BG-Ivan` stem.            |
| `1.3.0-amd64-ca-es-herenarus-preview`       | Container installatie kopie met de `ca-ES` land instellingen en de `ca-ES-HerenaRUS` stem.       |
| `1.3.0-amd64-cs-cz-jakub-preview`           | Container installatie kopie met de `cs-CZ` land instellingen en de `cs-CZ-Jakub` stem.           |
| `1.3.0-amd64-da-dk-hellerus-preview`        | Container installatie kopie met de `da-DK` land instellingen en de `da-DK-HelleRUS` stem.        |
| `1.3.0-amd64-de-at-michael-preview`         | Container installatie kopie met de `de-AT` land instellingen en de `de-AT-Michael` stem.         |
| `1.3.0-amd64-de-ch-karsten-preview`         | Container installatie kopie met de `de-CH` land instellingen en de `de-CH-Karsten` stem.         |
| `1.3.0-amd64-de-de-hedda-preview`           | Container installatie kopie met de `de-DE` land instellingen en de `de-DE-Hedda` stem.           |
| `1.3.0-amd64-de-de-heddarus-preview`        | Container installatie kopie met de `de-DE` land instellingen en de `de-DE-Hedda` stem.           |
| `1.3.0-amd64-de-de-heddarus-preview`        | Container installatie kopie met de `de-DE` land instellingen en de `de-DE-HeddaRUS` stem.        |
| `1.3.0-amd64-de-de-stefan-apollo-preview`   | Container installatie kopie met de `de-DE` land instellingen en de `de-DE-Stefan-Apollo` stem.   |
| `1.3.0-amd64-el-gr-stefanos-preview`        | Container installatie kopie met de `el-GR` land instellingen en de `el-GR-Stefanos` stem.        |
| `1.3.0-amd64-en-au-catherine-preview`       | Container installatie kopie met de `en-AU` land instellingen en de `en-AU-Catherine` stem.       |
| `1.3.0-amd64-en-au-hayleyrus-preview`       | Container installatie kopie met de `en-AU` land instellingen en de `en-AU-HayleyRUS` stem.       |
| `1.3.0-amd64-en-ca-heatherrus-preview`      | Container installatie kopie met de `en-CA` land instellingen en de `en-CA-HeatherRUS` stem.      |
| `1.3.0-amd64-en-ca-linda-preview`           | Container installatie kopie met de `en-CA` land instellingen en de `en-CA-Linda` stem.           |
| `1.3.0-amd64-en-gb-george-apollo-preview`   | Container installatie kopie met de `en-GB` land instellingen en de `en-GB-George-Apollo` stem.   |
| `1.3.0-amd64-en-gb-hazelrus-preview`        | Container installatie kopie met de `en-GB` land instellingen en de `en-GB-HazelRUS` stem.        |
| `1.3.0-amd64-en-gb-susan-apollo-preview`    | Container installatie kopie met de `en-GB` land instellingen en de `en-GB-Susan-Apollo` stem.    |
| `1.3.0-amd64-en-ie-sean-preview`            | Container installatie kopie met de `en-IE` land instellingen en de `en-IE-Sean` stem.            |
| `1.3.0-amd64-en-in-heera-apollo-preview`    | Container installatie kopie met de `en-IN` land instellingen en de `en-IN-Heera-Apollo` stem.    |
| `1.3.0-amd64-en-in-priyarus-preview`        | Container installatie kopie met de `en-IN` land instellingen en de `en-IN-PriyaRUS` stem.        |
| `1.3.0-amd64-en-in-ravi-apollo-preview`     | Container installatie kopie met de `en-IN` land instellingen en de `en-IN-Ravi-Apollo` stem.     |
| `1.3.0-amd64-en-us-benjaminrus-preview`     | Container installatie kopie met de `en-US` land instellingen en de `en-US-BenjaminRUS` stem.     |
| `1.3.0-amd64-en-us-guy24krus-preview`       | Container installatie kopie met de `en-US` land instellingen en de `en-US-Guy24kRUS` stem.       |
| `1.3.0-amd64-en-us-jessa24krus-preview`     | Container installatie kopie met de `en-US` land instellingen en de `en-US-Jessa24kRUS` stem.     |
| `1.3.0-amd64-en-us-jessarus-preview`        | Container installatie kopie met de `en-US` land instellingen en de `en-US-JessaRUS` stem.        |
| `1.3.0-amd64-en-us-zirarus-preview`         | Container installatie kopie met de `en-US` land instellingen en de `en-US-ZiraRUS` stem.         |
| `1.3.0-amd64-es-es-helenarus-preview`       | Container installatie kopie met de `es-ES` land instellingen en de `es-ES-HelenaRUS` stem.       |
| `1.3.0-amd64-es-es-laura-apollo-preview`    | Container installatie kopie met de `es-ES` land instellingen en de `es-ES-Laura-Apollo` stem.    |
| `1.3.0-amd64-es-es-pablo-apollo-preview`    | Container installatie kopie met de `es-ES` land instellingen en de `es-ES-Pablo-Apollo` stem.    |
| `1.3.0-amd64-es-mx-hildarus-preview`        | Container installatie kopie met de `es-MX` land instellingen en de `es-MX-HildaRUS` stem.        |
| `1.3.0-amd64-es-mx-raul-apollo-preview`     | Container installatie kopie met de `es-MX` land instellingen en de `es-MX-Raul-Apollo` stem.     |
| `1.3.0-amd64-fi-fi-heidirus-preview`        | Container installatie kopie met de `fi-FI` land instellingen en de `fi-FI-HeidiRUS` stem.        |
| `1.3.0-amd64-fr-ca-caroline-preview`        | Container installatie kopie met de `fr-CA` land instellingen en de `fr-CA-Caroline` stem.        |
| `1.3.0-amd64-fr-ca-harmonierus-preview`     | Container installatie kopie met de `fr-CA` land instellingen en de `fr-CA-HarmonieRUS` stem.     |
| `1.3.0-amd64-fr-ch-guillaume-preview`       | Container installatie kopie met de `fr-CH` land instellingen en de `fr-CH-Guillaume` stem.       |
| `1.3.0-amd64-fr-fr-hortenserus-preview`     | Container installatie kopie met de `fr-FR` land instellingen en de `fr-FR-HortenseRUS` stem.     |
| `1.3.0-amd64-fr-fr-julie-apollo-preview`    | Container installatie kopie met de `fr-FR` land instellingen en de `fr-FR-Julie-Apollo` stem.    |
| `1.3.0-amd64-fr-fr-paul-apollo-preview`     | Container installatie kopie met de `fr-FR` land instellingen en de `fr-FR-Paul-Apollo` stem.     |
| `1.3.0-amd64-he-il-asaf-preview`            | Container installatie kopie met de `he-IL` land instellingen en de `he-IL-Asaf` stem.            |
| `1.3.0-amd64-hi-in-hemant-preview`          | Container installatie kopie met de `hi-IN` land instellingen en de `hi-IN-Hemant` stem.          |
| `1.3.0-amd64-hi-in-kalpana-apollo-preview`  | Container installatie kopie met de `hi-IN` land instellingen en de `hi-IN-Kalpana-Apollo` stem.  |
| `1.3.0-amd64-hi-in-kalpana-apollo-preview`  | Container installatie kopie met de `hi-IN` land instellingen en de `hi-IN-Kalpana` stem.         |
| `1.3.0-amd64-hi-in-kalpana-preview`         | Container installatie kopie met de `hi-IN` land instellingen en de `hi-IN-Kalpana` stem.         |
| `1.3.0-amd64-hr-hr-matej-preview`           | Container installatie kopie met de `hr-HR` land instellingen en de `hr-HR-Matej` stem.           |
| `1.3.0-amd64-hu-hu-szabolcs-preview`        | Container installatie kopie met de `hu-HU` land instellingen en de `hu-HU-Szabolcs` stem.        |
| `1.3.0-amd64-id-id-andika-preview`          | Container installatie kopie met de `id-ID` land instellingen en de `id-ID-Andika` stem.          |
| `1.3.0-amd64-it-it-cosimo-apollo-preview`   | Container installatie kopie met de `it-IT` land instellingen en de `it-IT-Cosimo-Apollo` stem.   |
| `1.3.0-amd64-it-it-luciarus-preview`        | Container installatie kopie met de `it-IT` land instellingen en de `it-IT-LuciaRUS` stem.        |
| `1.3.0-amd64-ja-jp-ayumi-apollo-preview`    | Container installatie kopie met de `ja-JP` land instellingen en de `ja-JP-Ayumi-Apollo` stem.    |
| `1.3.0-amd64-ja-jp-harukarus-preview`       | Container installatie kopie met de `ja-JP` land instellingen en de `ja-JP-HarukaRUS` stem.       |
| `1.3.0-amd64-ja-jp-ichiro-apollo-preview`   | Container installatie kopie met de `ja-JP` land instellingen en de `ja-JP-Ichiro-Apollo` stem.   |
| `1.3.0-amd64-ko-kr-heamirus-preview`        | Container installatie kopie met de `ko-KR` land instellingen en de `ko-KR-HeamiRUS` stem.        |
| `1.3.0-amd64-ms-my-rizwan-preview`          | Container installatie kopie met de `ms-MY` land instellingen en de `ms-MY-Rizwan` stem.          |
| `1.3.0-amd64-nb-no-huldarus-preview`        | Container installatie kopie met de `nb-NO` land instellingen en de `nb-NO-HuldaRUS` stem.        |
| `1.3.0-amd64-nl-nl-hannarus-preview`        | Container installatie kopie met de `nl-NL` land instellingen en de `nl-NL-HannaRUS` stem.        |
| `1.3.0-amd64-pl-pl-paulinarus-preview`      | Container installatie kopie met de `pl-PL` land instellingen en de `pl-PL-PaulinaRUS` stem.      |
| `1.3.0-amd64-pt-br-daniel-apollo-preview`   | Container installatie kopie met de `pt-BR` land instellingen en de `pt-BR-Daniel-Apollo` stem.   |
| `1.3.0-amd64-pt-br-heloisarus-preview`      | Container installatie kopie met de `pt-BR` land instellingen en de `pt-BR-HeloisaRUS` stem.      |
| `1.3.0-amd64-pt-pt-heliarus-preview`        | Container installatie kopie met de `pt-PT` land instellingen en de `pt-PT-HeliaRUS` stem.        |
| `1.3.0-amd64-ro-ro-andrei-preview`          | Container installatie kopie met de `ro-RO` land instellingen en de `ro-RO-Andrei` stem.          |
| `1.3.0-amd64-ru-ru-ekaterinarus-preview`    | Container installatie kopie met de `ru-RU` land instellingen en de `ru-RU-EkaterinaRUS` stem.    |
| `1.3.0-amd64-ru-ru-irina-apollo-preview`    | Container installatie kopie met de `ru-RU` land instellingen en de `ru-RU-Irina-Apollo` stem.    |
| `1.3.0-amd64-ru-ru-pavel-apollo-preview`    | Container installatie kopie met de `ru-RU` land instellingen en de `ru-RU-Pavel-Apollo` stem.    |
| `1.3.0-amd64-sk-sk-filip-preview`           | Container installatie kopie met de `sk-SK` land instellingen en de `sk-SK-Filip` stem.           |
| `1.3.0-amd64-sl-si-lado-preview`            | Container installatie kopie met de `sl-SI` land instellingen en de `sl-SI-Lado` stem.            |
| `1.3.0-amd64-sv-se-hedvigrus-preview`       | Container installatie kopie met de `sv-SE` land instellingen en de `sv-SE-HedvigRUS` stem.       |
| `1.3.0-amd64-ta-in-valluvar-preview`        | Container installatie kopie met de `ta-IN` land instellingen en de `ta-IN-Valluvar` stem.        |
| `1.3.0-amd64-te-in-chitra-preview`          | Container installatie kopie met de `te-IN` land instellingen en de `te-IN-Chitra` stem.          |
| `1.3.0-amd64-th-th-pattara-preview`         | Container installatie kopie met de `th-TH` land instellingen en de `th-TH-Pattara` stem.         |
| `1.3.0-amd64-tr-tr-sedarus-preview`         | Container installatie kopie met de `tr-TR` land instellingen en de `tr-TR-SedaRUS` stem.         |
| `1.3.0-amd64-vi-vn-an-preview`              | Container installatie kopie met de `vi-VN` land instellingen en de `vi-VN-An` stem.              |
| `1.3.0-amd64-zh-cn-huihuirus-preview`       | Container installatie kopie met de `zh-CN` land instellingen en de `zh-CN-HuihuiRUS` stem.       |
| `1.3.0-amd64-zh-cn-kangkang-apollo-preview` | Container installatie kopie met de `zh-CN` land instellingen en de `zh-CN-Kangkang-Apollo` stem. |
| `1.3.0-amd64-zh-cn-yaoyao-apollo-preview`   | Container installatie kopie met de `zh-CN` land instellingen en de `zh-CN-Yaoyao-Apollo` stem.   |
| `1.3.0-amd64-zh-hk-danny-apollo-preview`    | Container installatie kopie met de `zh-HK` land instellingen en de `zh-HK-Danny-Apollo` stem.    |
| `1.3.0-amd64-zh-hk-tracy-apollo-preview`    | Container installatie kopie met de `zh-HK` land instellingen en de `zh-HK-Tracy-Apollo` stem.    |
| `1.3.0-amd64-zh-hk-tracyrus-preview`        | Container installatie kopie met de `zh-HK` land instellingen en de `zh-HK-TracyRUS` stem.        |
| `1.3.0-amd64-zh-tw-hanhanrus-preview`       | Container installatie kopie met de `zh-TW` land instellingen en de `zh-TW-HanHanRUS` stem.       |
| `1.3.0-amd64-zh-tw-yating-apollo-preview`   | Container installatie kopie met de `zh-TW` land instellingen en de `zh-TW-Yating-Apollo` stem.   |
| `1.3.0-amd64-zh-tw-zhiwei-apollo-preview`   | Container installatie kopie met de `zh-TW` land instellingen en de `zh-TW-Zhiwei-Apollo` stem.   |
| `1.2.0-amd64-de-de-heddarus-preview`        | Container installatie kopie met de `de-DE` land instellingen en de `de-DE-Hedda` stem.           |
| `1.2.0-amd64-de-de-heddarus-preview`        | Container installatie kopie met de `de-DE` land instellingen en de `de-DE-HeddaRUS` stem.        |
| `1.2.0-amd64-de-de-stefan-apollo-preview`   | Container installatie kopie met de `de-DE` land instellingen en de `de-DE-Stefan-Apollo` stem.   |
| `1.2.0-amd64-en-au-catherine-preview`       | Container installatie kopie met de `en-AU` land instellingen en de `en-AU-Catherine` stem.       |
| `1.2.0-amd64-en-au-hayleyrus-preview`       | Container installatie kopie met de `en-AU` land instellingen en de `en-AU-HayleyRUS` stem.       |
| `1.2.0-amd64-en-gb-george-apollo-preview`   | Container installatie kopie met de `en-GB` land instellingen en de `en-GB-George-Apollo` stem.   |
| `1.2.0-amd64-en-gb-hazelrus-preview`        | Container installatie kopie met de `en-GB` land instellingen en de `en-GB-HazelRUS` stem.        |
| `1.2.0-amd64-en-gb-susan-apollo-preview`    | Container installatie kopie met de `en-GB` land instellingen en de `en-GB-Susan-Apollo` stem.    |
| `1.2.0-amd64-en-in-heera-apollo-preview`    | Container installatie kopie met de `en-IN` land instellingen en de `en-IN-Heera-Apollo` stem.    |
| `1.2.0-amd64-en-in-priyarus-preview`        | Container installatie kopie met de `en-IN` land instellingen en de `en-IN-PriyaRUS` stem.        |
| `1.2.0-amd64-en-in-ravi-apollo-preview`     | Container installatie kopie met de `en-IN` land instellingen en de `en-IN-Ravi-Apollo` stem.     |
| `1.2.0-amd64-en-us-benjaminrus-preview`     | Container installatie kopie met de `en-US` land instellingen en de `en-US-BenjaminRUS` stem.     |
| `1.2.0-amd64-en-us-guy24krus-preview`       | Container installatie kopie met de `en-US` land instellingen en de `en-US-Guy24kRUS` stem.       |
| `1.2.0-amd64-en-us-jessa24krus-preview`     | Container installatie kopie met de `en-US` land instellingen en de `en-US-Jessa24kRUS` stem.     |
| `1.2.0-amd64-en-us-jessarus-preview`        | Container installatie kopie met de `en-US` land instellingen en de `en-US-JessaRUS` stem.        |
| `1.2.0-amd64-en-us-zirarus-preview`         | Container installatie kopie met de `en-US` land instellingen en de `en-US-ZiraRUS` stem.         |
| `1.2.0-amd64-es-es-helenarus-preview`       | Container installatie kopie met de `es-ES` land instellingen en de `es-ES-HelenaRUS` stem.       |
| `1.2.0-amd64-es-es-laura-apollo-preview`    | Container installatie kopie met de `es-ES` land instellingen en de `es-ES-Laura-Apollo` stem.    |
| `1.2.0-amd64-es-es-pablo-apollo-preview`    | Container installatie kopie met de `es-ES` land instellingen en de `es-ES-Pablo-Apollo` stem.    |
| `1.2.0-amd64-es-mx-hildarus-preview`        | Container installatie kopie met de `es-MX` land instellingen en de `es-MX-HildaRUS` stem.        |
| `1.2.0-amd64-es-mx-raul-apollo-preview`     | Container installatie kopie met de `es-MX` land instellingen en de `es-MX-Raul-Apollo` stem.     |
| `1.2.0-amd64-fr-ca-caroline-preview`        | Container installatie kopie met de `fr-CA` land instellingen en de `fr-CA-Caroline` stem.        |
| `1.2.0-amd64-fr-ca-harmonierus-preview`     | Container installatie kopie met de `fr-CA` land instellingen en de `fr-CA-HarmonieRUS` stem.     |
| `1.2.0-amd64-fr-fr-hortenserus-preview`     | Container installatie kopie met de `fr-FR` land instellingen en de `fr-FR-HortenseRUS` stem.     |
| `1.2.0-amd64-fr-fr-julie-apollo-preview`    | Container installatie kopie met de `fr-FR` land instellingen en de `fr-FR-Julie-Apollo` stem.    |
| `1.2.0-amd64-fr-fr-paul-apollo-preview`     | Container installatie kopie met de `fr-FR` land instellingen en de `fr-FR-Paul-Apollo` stem.     |
| `1.2.0-amd64-it-it-cosimo-apollo-preview`   | Container installatie kopie met de `it-IT` land instellingen en de `it-IT-Cosimo-Apollo` stem.   |
| `1.2.0-amd64-it-it-luciarus-preview`        | Container installatie kopie met de `it-IT` land instellingen en de `it-IT-LuciaRUS` stem.        |
| `1.2.0-amd64-ja-jp-ayumi-apollo-preview`    | Container installatie kopie met de `ja-JP` land instellingen en de `ja-JP-Ayumi-Apollo` stem.    |
| `1.2.0-amd64-ja-jp-harukarus-preview`       | Container installatie kopie met de `ja-JP` land instellingen en de `ja-JP-HarukaRUS` stem.       |
| `1.2.0-amd64-ja-jp-ichiro-apollo-preview`   | Container installatie kopie met de `ja-JP` land instellingen en de `ja-JP-Ichiro-Apollo` stem.   |
| `1.2.0-amd64-ko-kr-heamirus-preview`        | Container installatie kopie met de `ko-KR` land instellingen en de `ko-KR-HeamiRUS` stem.        |
| `1.2.0-amd64-pt-br-daniel-apollo-preview`   | Container installatie kopie met de `pt-BR` land instellingen en de `pt-BR-Daniel-Apollo` stem.   |
| `1.2.0-amd64-pt-br-heloisarus-preview`      | Container installatie kopie met de `pt-BR` land instellingen en de `pt-BR-HeloisaRUS` stem.      |
| `1.2.0-amd64-zh-cn-huihuirus-preview`       | Container installatie kopie met de `zh-CN` land instellingen en de `zh-CN-HuihuiRUS` stem.       |
| `1.2.0-amd64-zh-cn-kangkang-apollo-preview` | Container installatie kopie met de `zh-CN` land instellingen en de `zh-CN-Kangkang-Apollo` stem. |
| `1.2.0-amd64-zh-cn-yaoyao-apollo-preview`   | Container installatie kopie met de `zh-CN` land instellingen en de `zh-CN-Yaoyao-Apollo` stem.   |
| `1.1.0-amd64-de-de-hedda-preview`           | Container installatie kopie met de `de-DE` land instellingen en de `de-DE-Hedda` stem.           |
| `1.1.0-amd64-de-de-heddarus-preview`        | Container installatie kopie met de `de-DE` land instellingen en de `de-DE-Hedda` stem.           |
| `1.1.0-amd64-de-de-heddarus-preview`        | Container installatie kopie met de `de-DE` land instellingen en de `de-DE-HeddaRUS` stem.        |
| `1.1.0-amd64-de-de-stefan-apollo-preview`   | Container installatie kopie met de `de-DE` land instellingen en de `de-DE-Stefan-Apollo` stem.   |
| `1.1.0-amd64-en-au-catherine-preview`       | Container installatie kopie met de `en-AU` land instellingen en de `en-AU-Catherine` stem.       |
| `1.1.0-amd64-en-au-hayleyrus-preview`       | Container installatie kopie met de `en-AU` land instellingen en de `en-AU-HayleyRUS` stem.       |
| `1.1.0-amd64-en-gb-george-apollo-preview`   | Container installatie kopie met de `en-GB` land instellingen en de `en-GB-George-Apollo` stem.   |
| `1.1.0-amd64-en-gb-hazelrus-preview`        | Container installatie kopie met de `en-GB` land instellingen en de `en-GB-HazelRUS` stem.        |
| `1.1.0-amd64-en-gb-susan-apollo-preview`    | Container installatie kopie met de `en-GB` land instellingen en de `en-GB-Susan-Apollo` stem.    |
| `1.1.0-amd64-en-in-heera-apollo-preview`    | Container installatie kopie met de `en-IN` land instellingen en de `en-IN-Heera-Apollo` stem.    |
| `1.1.0-amd64-en-in-priyarus-preview`        | Container installatie kopie met de `en-IN` land instellingen en de `en-IN-PriyaRUS` stem.        |
| `1.1.0-amd64-en-in-ravi-apollo-preview`     | Container installatie kopie met de `en-IN` land instellingen en de `en-IN-Ravi-Apollo` stem.     |
| `1.1.0-amd64-en-us-benjaminrus-preview`     | Container installatie kopie met de `en-US` land instellingen en de `en-US-BenjaminRUS` stem.     |
| `1.1.0-amd64-en-us-guy24krus-preview`       | Container installatie kopie met de `en-US` land instellingen en de `en-US-Guy24kRUS` stem.       |
| `1.1.0-amd64-en-us-jessa24krus-preview`     | Container installatie kopie met de `en-US` land instellingen en de `en-US-Jessa24kRUS` stem.     |
| `1.1.0-amd64-en-us-jessarus-preview`        | Container installatie kopie met de `en-US` land instellingen en de `en-US-JessaRUS` stem.        |
| `1.1.0-amd64-en-us-zirarus-preview`         | Container installatie kopie met de `en-US` land instellingen en de `en-US-ZiraRUS` stem.         |
| `1.1.0-amd64-es-es-helenarus-preview`       | Container installatie kopie met de `es-ES` land instellingen en de `es-ES-HelenaRUS` stem.       |
| `1.1.0-amd64-es-es-laura-apollo-preview`    | Container installatie kopie met de `es-ES` land instellingen en de `es-ES-Laura-Apollo` stem.    |
| `1.1.0-amd64-es-es-pablo-apollo-preview`    | Container installatie kopie met de `es-ES` land instellingen en de `es-ES-Pablo-Apollo` stem.    |
| `1.1.0-amd64-es-mx-hildarus-preview`        | Container installatie kopie met de `es-MX` land instellingen en de `es-MX-HildaRUS` stem.        |
| `1.1.0-amd64-es-mx-raul-apollo-preview`     | Container installatie kopie met de `es-MX` land instellingen en de `es-MX-Raul-Apollo` stem.     |
| `1.1.0-amd64-fr-ca-caroline-preview`        | Container installatie kopie met de `fr-CA` land instellingen en de `fr-CA-Caroline` stem.        |
| `1.1.0-amd64-fr-ca-harmonierus-preview`     | Container installatie kopie met de `fr-CA` land instellingen en de `fr-CA-HarmonieRUS` stem.     |
| `1.1.0-amd64-fr-fr-hortenserus-preview`     | Container installatie kopie met de `fr-FR` land instellingen en de `fr-FR-HortenseRUS` stem.     |
| `1.1.0-amd64-fr-fr-julie-apollo-preview`    | Container installatie kopie met de `fr-FR` land instellingen en de `fr-FR-Julie-Apollo` stem.    |
| `1.1.0-amd64-fr-fr-paul-apollo-preview`     | Container installatie kopie met de `fr-FR` land instellingen en de `fr-FR-Paul-Apollo` stem.     |
| `1.1.0-amd64-it-it-cosimo-apollo-preview`   | Container installatie kopie met de `it-IT` land instellingen en de `it-IT-Cosimo-Apollo` stem.   |
| `1.1.0-amd64-it-it-luciarus-preview`        | Container installatie kopie met de `it-IT` land instellingen en de `it-IT-LuciaRUS` stem.        |
| `1.1.0-amd64-ja-jp-ayumi-apollo-preview`    | Container installatie kopie met de `ja-JP` land instellingen en de `ja-JP-Ayumi-Apollo` stem.    |
| `1.1.0-amd64-ja-jp-harukarus-preview`       | Container installatie kopie met de `ja-JP` land instellingen en de `ja-JP-HarukaRUS` stem.       |
| `1.1.0-amd64-ja-jp-ichiro-apollo-preview`   | Container installatie kopie met de `ja-JP` land instellingen en de `ja-JP-Ichiro-Apollo` stem.   |
| `1.1.0-amd64-ko-kr-heamirus-preview`        | Container installatie kopie met de `ko-KR` land instellingen en de `ko-KR-HeamiRUS` stem.        |
| `1.1.0-amd64-pt-br-daniel-apollo-preview`   | Container installatie kopie met de `pt-BR` land instellingen en de `pt-BR-Daniel-Apollo` stem.   |
| `1.1.0-amd64-pt-br-heloisarus-preview`      | Container installatie kopie met de `pt-BR` land instellingen en de `pt-BR-HeloisaRUS` stem.      |
| `1.1.0-amd64-zh-cn-huihuirus-preview`       | Container installatie kopie met de `zh-CN` land instellingen en de `zh-CN-HuihuiRUS` stem.       |
| `1.1.0-amd64-zh-cn-kangkang-apollo-preview` | Container installatie kopie met de `zh-CN` land instellingen en de `zh-CN-Kangkang-Apollo` stem. |
| `1.1.0-amd64-zh-cn-yaoyao-apollo-preview`   | Container installatie kopie met de `zh-CN` land instellingen en de `zh-CN-Yaoyao-Apollo` stem.   |
| `1.0.0-amd64-en-us-benjaminrus-preview`     | Container installatie kopie met de `en-US` land instellingen en de `en-US-BenjaminRUS` stem.     |
| `1.0.0-amd64-en-us-guy24krus-preview`       | Container installatie kopie met de `en-US` land instellingen en de `en-US-Guy24kRUS` stem.       |
| `1.0.0-amd64-en-us-jessa24krus-preview`     | Container installatie kopie met de `en-US` land instellingen en de `en-US-Jessa24kRUS` stem.     |
| `1.0.0-amd64-en-us-jessarus-preview`        | Container installatie kopie met de `en-US` land instellingen en de `en-US-JessaRUS` stem.        |
| `1.0.0-amd64-en-us-zirarus-preview`         | Container installatie kopie met de `en-US` land instellingen en de `en-US-ZiraRUS` stem.         |
| `1.0.0-amd64-zh-cn-huihuirus-preview`       | Container installatie kopie met de `zh-CN` land instellingen en de `zh-CN-HuihuiRUS` stem.       |
| `1.0.0-amd64-zh-cn-kangkang-apollo-preview` | Container installatie kopie met de `zh-CN` land instellingen en de `zh-CN-Kangkang-Apollo` stem. |
| `1.0.0-amd64-zh-cn-yaoyao-apollo-preview`   | Container installatie kopie met de `zh-CN` land instellingen en de `zh-CN-Yaoyao-Apollo` stem.   |

## <a name="key-phrase-extraction"></a>Sleuteltermextractie

De [Sleuteltermextractie][ta-kp] container installatie kopie vindt u in het `mcr.microsoft.com` container register Syndicate. Deze bevindt zich in de `azure-cognitive-services` opslag plaats en heet `keyphrase` . De volledig gekwalificeerde naam van de container installatie kopie is, `mcr.microsoft.com/azure-cognitive-services/keyphrase` .

Deze container installatie kopie heeft de volgende tags beschikbaar:

| Afbeeldings Tags                    | Opmerkingen |
|-------------------------------|:------|
| `latest`                      |       |
| `1.1.009301-amd64-preview`    |       |
| `1.1.008510001-amd64-preview` |       |
| `1.1.007750002-amd64-preview` |       |
| `1.1.007360001-amd64-preview` |       |
| `1.1.006770001-amd64-preview` |       |

## <a name="language-detection"></a>Taaldetectie

De [taaldetectie][ta-la] container installatie kopie vindt u in het `mcr.microsoft.com` container register Syndicate. Deze bevindt zich in de `azure-cognitive-services` opslag plaats en heet `language` . De volledig gekwalificeerde naam van de container installatie kopie is, `mcr.microsoft.com/azure-cognitive-services/language` .

Deze container installatie kopie heeft de volgende tags beschikbaar:

| Afbeeldings Tags                    | Opmerkingen |
|-------------------------------|:------|
| `latest`                      |       |
| `1.1.009301-amd64-preview`    |       |
| `1.1.008510001-amd64-preview` |       |
| `1.1.007750002-amd64-preview` |       |
| `1.1.007360001-amd64-preview` |       |
| `1.1.006770001-amd64-preview` |       |

## <a name="sentiment-analysis"></a>Sentimentanalyse

De [sentimentanalyse][ta-se] container installatie kopie vindt u in het `mcr.microsoft.com` container register Syndicate. Deze bevindt zich in de `azure-cognitive-services` opslag plaats en heet `sentiment` . De volledig gekwalificeerde naam van de container installatie kopie is, `mcr.microsoft.com/azure-cognitive-services/sentiment` .

Deze container installatie kopie heeft de volgende tags beschikbaar:

| Afbeeldings Tags | Opmerkingen                                         |
|------------|:----------------------------------------------|
| `latest`   |                                               |
| `3.0-en`   | Sentimentanalyse v3 (Engels)               |
| `3.0-es`   | Sentimentanalyse v3 (Spaans)               |
| `3.0-fr`   | Sentimentanalyse v3 (Frans)                |
| `3.0-it`   | Sentimentanalyse v3 (Italiaans)               |
| `3.0-de`   | Sentimentanalyse v3 (Duits)                |
| `3.0-zh`   | Sentimentanalyse v3 (Chinees-vereenvoudigd)  |
| `3.0-zht`  | Sentimentanalyse v3 (Chinees-traditioneel) |
| `3.0-ja`   | Sentimentanalyse v3 (Japans)              |
| `3.0-pt`   | Sentimentanalyse v3 (Portugees)            |
| `3.0-nl`   | Sentimentanalyse v3 (Nederlands)                 |
| `1.1.009301-amd64-preview`    | Sentimentanalyse v2      |
| `1.1.008510001-amd64-preview` |       |
| `1.1.007750002-amd64-preview` |       |
| `1.1.007360001-amd64-preview` |       |
| `1.1.006770001-amd64-preview` |       |

[ad-containers]: ../anomaly-Detector/anomaly-detector-container-howto.md
[cv-containers]: ../computer-vision/computer-vision-how-to-install-containers.md
[fa-containers]: ../face/face-how-to-install-containers.md
[fr-containers]: ../form-recognizer/form-recognizer-container-howto.md
[lu-containers]: ../luis/luis-container-howto.md
[sp-stt]: ../speech-service/speech-container-howto.md?tabs=stt
[sp-cstt]: ../speech-service/speech-container-howto.md?tabs=cstt
[sp-tts]: ../speech-service/speech-container-howto.md?tabs=tts
[sp-ctts]: ../speech-service/speech-container-howto.md?tabs=ctts
[ta-kp]: ../text-analytics/how-tos/text-analytics-how-to-install-containers.md?tabs=keyphrase
[ta-la]: ../text-analytics/how-tos/text-analytics-how-to-install-containers.md?tabs=language
[ta-se]: ../text-analytics/how-tos/text-analytics-how-to-install-containers.md?tabs=sentiment

---
title: Gegevens voorbereiden voor de aangepaste Voice-Speech-Service
titleSuffix: Azure Cognitive Services
description: Maak een aangepaste stem voor uw merk met de spraak service. U geeft studio-opnames en de bijbehorende scripts, de service genereert een uniek spraak model dat is afgestemd op de opgenomen spraak. Gebruik deze stem om spraak te gebruiken in uw producten, hulpprogram ma's en toepassingen.
services: cognitive-services
author: erhopf
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 11/04/2019
ms.author: erhopf
ms.openlocfilehash: 5427e9f996fb77d455aa8064fc7cb1c65e1fcf7e
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/29/2020
ms.locfileid: "74805974"
---
# <a name="prepare-data-to-create-a-custom-voice"></a>Gegevens voorbereiden voor het maken van een aangepaste spraak

Wanneer u klaar bent voor het maken van een aangepaste tekst-naar-spraak-stem voor uw toepassing, is de eerste stap het verzamelen van audio-opnames en bijbehorende scripts om te beginnen met het trainen van het spraak model. De speech-service gebruikt deze gegevens om een unieke stem te maken die is afgestemd op de stem in de opnamen. Nadat u de stem hebt getraind, kunt u de spraak functie voor uw toepassingen starten.

U kunt beginnen met een kleine hoeveelheid gegevens om een proef concept te maken. Hoe meer gegevens u ook opgeeft, hoe natuurlijk uw aangepaste stem klinkt. Voordat u uw eigen stem model voor tekst naar spraak kunt trainen, hebt u audio-opnames en de bijbehorende tekst transcripties nodig. Op deze pagina controleren we gegevens typen, hoe ze worden gebruikt en hoe u deze kunt beheren.

## <a name="data-types"></a>Gegevenstypen

Een gegevensset voor spraak training bevat audio-opnames en een tekst bestand met de gekoppelde transcripties. Elk audio bestand moet één utterance bevatten (één zin of één keer voor een dialoog systeem), en moet minder dan 15 seconden lang zijn.

In sommige gevallen hebt u de juiste gegevensset mogelijk niet gereed en wilt u de aangepaste spraak training testen met beschik bare audio bestanden, korte of lange, met of zonder transcripten. We bieden hulpprogram ma's (bèta) die u helpen bij het segmenteren van uw audio in uitingen en het voorbereiden van transcripten met behulp van de [batch transcriptie-API](batch-transcription.md).

Deze tabel geeft een lijst van gegevens typen en hoe deze worden gebruikt voor het maken van een aangepast spraak model voor tekst naar spraak.

| Gegevenstype | Beschrijving | Wanneer gebruikt u dit? | Aanvullende service vereist | Aantal voor het trainen van een model | Land instelling (en) |
| --------- | ----------- | ----------- | --------------------------- | ----------------------------- | --------- |
| **Afzonderlijke uitingen + overeenkomende transcript** | Een verzameling (. zip) van audio bestanden (. wav) als afzonderlijke uitingen. Elk audio bestand moet 15 seconden of korter zijn, gekoppeld aan een opgemaakt transcript (. txt). | Professionele opnamen met overeenkomende transcripten | Klaar voor training. | Geen harde vereiste voor en-US en zh-CN. Meer dan 2000 + afzonderlijke uitingen voor andere landen. | [Alle aangepaste spraak instellingen](language-support.md#customization) |
| **Lange audio en Transcripten (bèta)** | Een verzameling (. zip) van lange, niet-gesegmenteerde audio bestanden (langer dan 20 seconden), gekoppeld aan een transcript (. txt) dat alle gesp roken woorden bevat. | U hebt audio bestanden en overeenkomende transcripten, maar deze worden niet gesegmenteerd in uitingen. | Segmentatie (met behulp van batch transcriptie).<br>Trans formatie van audio-indeling waar nodig. | Geen harde vereiste  | [Alle aangepaste spraak instellingen](language-support.md#customization) |
| **Alleen audio (bèta)** | Een verzameling (. zip) van audio bestanden zonder transcripten. | Er zijn alleen audio bestanden beschikbaar, zonder transcripten. | Segmentatie en Transcripten genereren (met behulp van batch transcriptie).<br>Trans formatie van audio-indeling waar nodig.| Geen harde vereiste | [Alle aangepaste spraak instellingen](language-support.md#customization) |

Bestanden moeten worden gegroepeerd door te typen in een gegevensset en worden geüpload als een zip-bestand. Elke gegevensset kan slechts één gegevens type bevatten.

> [!NOTE]
> Het maximum aantal gegevens sets dat per abonnement mag worden geïmporteerd is 10. zip-bestanden voor gratis abonnement (F0) gebruikers en 500 voor Standard Subscription (S0)-gebruikers.

## <a name="individual-utterances--matching-transcript"></a>Afzonderlijke uitingen + overeenkomende transcript

U kunt de opnamen van afzonderlijke uitingen en de overeenkomende transcripten op twee manieren voorbereiden. Schrijf een script en laat het door een stem-talen Schrift lezen of gebruik openbaar beschik bare audio en transcribeer het naar tekst. Als u dit wel doet, bewerkt u disfluencies van de audio bestanden, zoals "um" en andere opvul geluiden, stutters, mumbled woorden of mispronunciations.

Als u een goed gesp roken letter type wilt maken, maakt u de opnamen in een stille ruimte met een microfoon van hoge kwaliteit. Het consistente volume, de spreek snelheid, de spreek hoogte en de expres mannerisms van spraak zijn essentieel.

> [!TIP]
> Als u een spraak voor productie gebruik wilt maken, raden we u aan om een professionele opname studio en Voice-talen te gebruiken. Zie voor meer informatie [spraak voorbeelden vastleggen voor een aangepaste stem](record-custom-voice-samples.md).

### <a name="audio-files"></a>Audio bestanden

Elk audio bestand moet één utterance bevatten (één zin of één draai van een dialoog systeem), kleiner dan 15 seconden lang. Alle bestanden moeten zich in dezelfde gesp roken taal behoeven. Aangepaste tekst-naar-spraak-stemmen met meerdere talen worden niet ondersteund, met uitzonde ring van het Chinese Engelse bi-taal. Elk audio bestand moet een unieke numerieke bestands naam hebben met de bestandsnaam extensie. WAV.

Volg deze richt lijnen bij het voorbereiden van audio.

| Eigenschap | Waarde |
| -------- | ----- |
| Bestands indeling | RIFF (. wav), gegroepeerd in een zip-bestand |
| Sampling frequentie | Ten minste 16.000 Hz |
| Voorbeeld indeling | PCM, 16-bits |
| Bestandsnaam | Numeric, met de extensie. WAV. Er zijn geen dubbele bestands namen toegestaan. |
| Audio lengte | Korter dan 15 seconden |
| Archief indeling | .zip |
| Maximale archief grootte | 2048 MB |

> [!NOTE]
> WAV-bestanden met een lagere sampling frequentie dan 16.000 Hz worden geweigerd. Als een zip-bestand. WAV-bestanden met verschillende sample frequenties bevat, worden alleen die waarden geïmporteerd die gelijk zijn aan of hoger zijn dan 16.000 Hz. De portal importeert momenteel een zip-archief van Maxi maal 200 MB. Er kunnen echter meerdere archieven worden geüpload.

### <a name="transcripts"></a>Transcripten

Het transcriptie-bestand is een bestand met tekst zonder opmaak. Gebruik deze richt lijnen om uw transcripties voor te bereiden.

| Eigenschap | Waarde |
| -------- | ----- |
| Bestands indeling | Tekst zonder opmaak (. txt) |
| Coderings indeling | ANSI/ASCII, UTF-8, UTF-8-BOM, UTF-16-LE of UTF-16-to. Voor zh-CN, ANSI/ASCII en UTF-8-code ringen worden niet ondersteund. |
| Aantal utterances per regel | **Eén** -elke regel van het transcriptie-bestand moet de naam bevatten van een van de audio bestanden, gevolgd door de bijbehorende transcriptie. De bestandsnaam en transcriptie moeten worden gescheiden door een tab (\t). |
| Maximale bestandsgrootte | 2048 MB |

Hieronder ziet u een voor beeld van hoe de transcripten worden georganiseerd utterance door utterance in één txt-bestand:

```
0000000001[tab] This is the waistline, and it's falling.
0000000002[tab] We have trouble scoring.
0000000003[tab] It was Janet Maslin.
```
Het is belang rijk dat de transcripten 100% nauw keurige transcripties van de bijbehorende audio zijn. Fouten in de transcripten leiden tot kwaliteits verlies tijdens de training.

> [!TIP]
> Bij het bouwen van stemmen voor tekst-naar-spraak, selecteert u uitingen (of scripts schrijven) die rekening houden met zowel een fonetische dekking als efficiëntie. Hebt u problemen met het verkrijgen van de gewenste resultaten? [Neem contact op met het aangepaste spraak](mailto:speechsupport@microsoft.com) team om meer te weten te komen over hoe u ons kunt raadplegen.

## <a name="long-audio--transcript-beta"></a>Lange audio en Transcripten (bèta)

In sommige gevallen hebt u mogelijk geen gesegmenteerde audio beschikbaar. We bieden een service (bèta) via de aangepaste Voice Portal om u te helpen lange audio bestanden te segmenteren en transcripties te maken. Houd er rekening mee dat deze service wordt gefactureerd voor het gebruik van spraak-naar-tekst-abonnementen.

> [!NOTE]
> De Long-audio segmentation-service maakt gebruik van de batch transcriptie-functie van spraak naar tekst, die alleen ondersteuning biedt voor standaard-abonnements gebruikers (S0). Tijdens de verwerking van de segmentering worden uw audio bestanden en de transcripten ook verzonden naar de Custom Speech-Service om het herkennings model te verfijnen zodat de nauw keurigheid van uw gegevens kan worden verbeterd. Er worden geen gegevens bewaard tijdens dit proces. Nadat de segmentatie is voltooid, worden alleen de transcripten voor uitingen en de bijbehorende toewijzingen opgeslagen voor uw down loads en trainingen.

### <a name="audio-files"></a>Audio bestanden

Volg deze richt lijnen bij het voorbereiden van de audio voor segmentering.

| Eigenschap | Waarde |
| -------- | ----- |
| Bestands indeling | RIFF (. wav) met een sampling frequentie van ten minste 16 kHz-16-bits in PCM of. mp3 met een bitsnelheid van ten minste 256 KBps, gegroepeerd in een zip-bestand |
| Bestandsnaam | ASCII-en Unicode-tekens worden ondersteund. Er zijn geen dubbele namen toegestaan. |
| Audio lengte | Langer dan 20 seconden |
| Archief indeling | .zip |
| Maximale archief grootte | 2048 MB |

Alle audio bestanden moeten worden gegroepeerd in een zip-bestand. U kunt WAV-bestanden en MP3-bestanden opslaan in één audio-zip. U kunt bijvoorbeeld een zip-bestand uploaden met een audio bestand met de naam ' kingstory. wav ', 45-Second-Long en een ander audio met de naam ' queenstory. mp3 ', 200-Second-Long. Alle MP3-bestanden worden na verwerking getransformeerd in de. WAV-indeling.

### <a name="transcripts"></a>Transcripten

Transcripten moeten worden voor bereid op de specificaties die in deze tabel worden vermeld. Elk audio bestand moet overeenkomen met een transcript.

| Eigenschap | Waarde |
| -------- | ----- |
| Bestands indeling | Tekst zonder opmaak (. txt), gegroepeerd in een. zip |
| Bestandsnaam | Dezelfde naam gebruiken als het overeenkomende audio bestand |
| Coderings indeling | Alleen UTF-8-stuk lijst |
| Aantal utterances per regel | Geen limiet |
| Maximale bestandsgrootte | 2048 MB |

Alle transcripten-bestanden in dit gegevens type moeten worden gegroepeerd in een zip-bestand. U hebt bijvoorbeeld een zip-bestand met een audio bestand met de naam ' kingstory. wav ', 45 seconden lang, en een andere met de naam ' queenstory. mp3 ', 200 seconden lang. U moet een ander zip-bestand uploaden dat twee transcripten bevat: een met de naam ' kingstory. txt ', de andere ' queenstory. txt '. In elk bestand met tekst zonder opmaak geeft u de volledige juiste transcriptie op voor de overeenkomende audio.

Nadat uw gegevensset is geüpload, kunt u het audio bestand in uitingen segmenteren op basis van het gegeven transcript. U kunt de gesegmenteerde uitingen en de overeenkomende transcripten controleren door de gegevensset te downloaden. Unieke Id's worden automatisch toegewezen aan de gesegmenteerde uitingen. Het is belang rijk dat u ervoor zorgt dat de transcripten die u opgeeft, 100% nauw keurig zijn. Fouten in de transcripten kunnen de nauw keurigheid van de audio segmentering verminderen en het kwaliteits verlies in de trainings fase verder verbeteren.

## <a name="audio-only-beta"></a>Alleen audio (bèta)

Als u geen transcripties voor uw audio-opnames hebt, gebruikt u de optie **alleen audio** om uw gegevens te uploaden. Ons systeem kan u helpen bij het segmenteren en transcriberen van uw audio bestanden. Houd er rekening mee dat deze service wordt meegeteld bij het gebruik van spraak-naar-tekst-abonnementen.

Volg deze richt lijnen bij het voorbereiden van audio.

> [!NOTE]
> De Long-audio segmentation-service maakt gebruik van de batch transcriptie-functie van spraak naar tekst, die alleen ondersteuning biedt voor standaard-abonnements gebruikers (S0).

| Eigenschap | Waarde |
| -------- | ----- |
| Bestands indeling | RIFF (. wav) met een sampling frequentie van ten minste 16 kHz-16-bits in PCM of. mp3 met een bitsnelheid van ten minste 256 KBps, gegroepeerd in een zip-bestand |
| Bestandsnaam | ASCII-en Unicode-tekens worden ondersteund. Er is geen dubbele naam toegestaan. |
| Audio lengte | Langer dan 20 seconden |
| Archief indeling | .zip |
| Maximale archief grootte | 2048 MB |

Alle audio bestanden moeten worden gegroepeerd in een zip-bestand. Zodra uw gegevensset is geüpload, kunt u het audio bestand in uitingen segmenteren op basis van de transcriptie-service voor spraak batches. Unieke Id's worden automatisch toegewezen aan de gesegmenteerde uitingen. Overeenkomende transcripten worden gegenereerd via spraak herkenning. Alle MP3-bestanden worden na verwerking getransformeerd in de. WAV-indeling. U kunt de gesegmenteerde uitingen en de overeenkomende transcripten controleren door de gegevensset te downloaden.

## <a name="next-steps"></a>Volgende stappen

- [Een aangepaste stem maken](how-to-custom-voice-create-voice.md)
- [Hand leiding: uw stem voorbeelden vastleggen](record-custom-voice-samples.md)

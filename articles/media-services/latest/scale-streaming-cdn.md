---
title: Inhoud streamen met CDN-integratie
titleSuffix: Azure Media Services
description: Meer informatie over het streamen van inhoud met CDN-integratie en het vooraf ophalen en verkrijgen van CDN-prefetch.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: ''
ms.topic: article
ms.date: 02/13/2020
ms.author: juliako
ms.openlocfilehash: 4ed8ada306720b7a8b44ddd59cefe399238c906a
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/28/2020
ms.locfileid: "80128056"
---
# <a name="stream-content-with-cdn-integration"></a>Inhoud streamen met CDN-integratie

Azure Content Delivery Network (CDN) biedt ontwikkelaars een globale oplossing voor het snel leveren van inhoud met hoge bandbreedte door de inhoud op strategische, fysieke knooppunten in de hele wereld in de cache op te slaan.  

Met CDN wordt inhoud gestreamd van een Media Services [streaming-eind punt (oorsprong)](streaming-endpoint-concept.md) per codec, per streaming-protocol, per bitrate, per container indeling en per VERSLEUTELING/DRM. Voor elke combi natie van codec-streaming protocol-container indeling-bitrate-Encryption is er een afzonderlijke CDN-cache.

De populaire inhoud wordt rechtstreeks vanuit de CDN-cache verwerkt zolang het video fragment in de cache wordt opgeslagen. Live-inhoud wordt waarschijnlijk in de cache opgeslagen, omdat er meestal veel mensen precies hetzelfde zijn. Inhoud op aanvraag kan een beetje trickier, omdat u een aantal inhoud zou kunnen hebben die populair is en niet. Als u miljoenen video-assets hebt waar geen van de elementen populair zijn (slechts één of twee kijkers per week), maar duizenden personen die alle verschillende Video's volgen, wordt het CDN veel minder effectief.

U moet ook nadenken over de werking van adaptieve streaming. Elk afzonderlijk video fragment wordt in de cache opgeslagen als een eigen entiteit. Stel bijvoorbeeld dat de eerste keer dat een bepaalde video wordt bekeken. Als de Viewer niet meer dan een paar seconden ziet, worden er alleen video fragmenten weer gegeven die zijn gekoppeld aan wat de persoon bekijkt in het cache geheugen in CDN. Met adaptieve streaming hebt u doorgaans 5 tot 7 verschillende bitrates voor video. Als één persoon een bitrate bekijkt en een andere persoon een andere bitsnelheid bekijkt, worden ze afzonderlijk in het CDN opgeslagen. Zelfs als twee mensen dezelfde bitsnelheid volgen, kunnen ze worden gestreamd via verschillende protocollen. Elk protocol (HLS, MPEG-DASH, Smooth Streaming) wordt afzonderlijk in de cache opgeslagen. Elke bitsnelheid en elk protocol worden afzonderlijk in de cache opgeslagen en alleen de video fragmenten die zijn aangevraagd, worden in de cache opgeslagen.

Als u wilt bepalen of CDN moet worden ingeschakeld op het Media Services [streaming-eind punt](streaming-endpoint-concept.md), moet u rekening houden met het aantal verwachte viewers. CDN helpt alleen als u veel kijkers voor uw inhoud verwacht. Als de maximale gelijktijdigheid van viewers lager is dan 500, is het raadzaam CDN uit te scha kelen omdat CDN het beste met gelijktijdigheid kan worden geschaald.

In dit onderwerp vindt u informatie over het inschakelen van [CDN-integratie](#enable-azure-cdn-integration). Ook wordt uitgelegd hoe u vooraf haalt (actieve cache) en het concept van [CDN-prefetch](#origin-assist-cdn-prefetch) .

## <a name="considerations"></a>Overwegingen

* Het [streaming-eind punt](streaming-endpoint-concept.md) `hostname` en de streaming-URL blijven hetzelfde, ongeacht of u CDN inschakelt.
* Als u de mogelijkheid wilt bieden om uw inhoud te testen met of zonder CDN, maakt u een ander streaming-eind punt dat niet is ingeschakeld voor CDN.

## <a name="enable-azure-cdn-integration"></a>Integratie van Azure CDN inschakelen

> [!IMPORTANT]
> U kunt CDN niet inschakelen voor Azure-accounts voor proef abonnementen of studenten.
>
> CDN-integratie is ingeschakeld in alle Azure-data centers, met uitzonde ring van regio's van de federale overheid en China.

Nadat een streaming-eind punt is ingericht met CDN ingeschakeld, is er een gedefinieerde wacht tijd op Media Services voordat de DNS-update wordt uitgevoerd om het streaming-eind punt toe te wijzen aan het CDN-eind punt.

Als u het CDN later wilt uitschakelen/inschakelen, moet het streaming-eind punt de status **gestopt** hebben. Het kan tot twee uur duren voordat de Azure CDN integratie is ingeschakeld en de wijzigingen voor alle CDN-Pop's actief zijn. U kunt echter het streaming-eind punt starten en streamen zonder onderbrekingen van het streaming-eind punt. Zodra de integratie is voltooid, wordt de stroom geleverd vanuit het CDN. Tijdens de inrichtings periode bevinden uw streaming-eind punt zich in de **Start** status en kunt u gedegradeerde prestaties waarnemen.

Wanneer het standaard streaming-eind punt wordt gemaakt, wordt het standaard geconfigureerd met standaard Verizon. U kunt Premium Verizon-of Standard Akamai-providers configureren met behulp van REST Api's.

Azure Media Services integratie met Azure CDN is geïmplementeerd op **Azure CDN van Verizon** voor Standard streaming-eind punten. Premium streaming-eind punten kunnen worden geconfigureerd met alle **Azure CDN prijs categorieën en providers**.

> [!NOTE]
> Zie het [CDN-overzicht](../../cdn/cdn-overview.md)voor meer informatie over Azure CDN.

## <a name="determine-if-a-dns-change-was-made"></a>Bepalen of een DNS-wijziging is doorgevoerd

U kunt bepalen of de DNS-wijziging is doorgevoerd in een streaming-eind punt (het verkeer wordt omgeleid naar <https://www.digwebinterface.com>de Azure CDN) met behulp van. Als u azureedge.net-domein namen in de resultaten ziet, wordt het verkeer nu naar het CDN gewijsd.

## <a name="origin-assist-cdn-prefetch"></a>Oorsprong-door CDN-prefetch

CDN-caching is een reactief proces. Als CDN kan voors pellen wat het volgende object zal worden aangevraagd, kan CDN het volgende object proactief aanvragen en in de cache opslaan. Met dit proces kunt u een cache-treffer voor alle (of de meeste) objecten bereiken, waardoor de prestaties verbeteren.

Het concept van het vooraf ophalen streeft naar het plaatsen van objecten aan de rand van het Internet, in afwachting dat deze door de speler onmiddellijk worden aangevraagd, waardoor de tijd voor het leveren van dat object aan de speler wordt verminderd.

Om dit doel te verzorgen, moet een streaming-eind punt (oorsprong) en CDN op een paar manieren aan de hand worden gebracht:

- De Media Services oorsprong moet de ' intelligentie ' (oorsprong-assistent) hebben om CDN het volgende object te informeren over prefetch.
- CDN voert de prefetch-en caching-functie (CDN-prefetch) uit. CDN moet ook beschikken over de "intelligentie" om de oorsprong te informeren of het een prefetch of een regel matige ophaal bewerking is, de antwoorden van 404 afhandelen en een manier om oneindige prefetch-lus te voor komen.

### <a name="benefits"></a>Voordelen

De voor delen van de functie voor *CDN-prefetch van oorsprong-assistentie zijn* onder andere:

- Met Prefetch wordt de video weergave kwaliteit verbeterd door verwachte video segmenten aan de rand te plaatsen tijdens het afspelen, het verminderen van de latentie tot de viewer en het verbeteren van download tijden voor video segmenten. Dit leidt tot snellere video-start tijd en kleinere rebuffering van exemplaren.
- Dit concept is van toepassing op het algemene CDN-Origin-scenario en is niet beperkt tot media.
- Akamai heeft deze functie toegevoegd aan [Akamai Cloud Embed (ACE)](https://learn.akamai.com/en-us/products/media_delivery/cloud_embed.html).

> [!NOTE]
> Deze functie is nog niet van toepassing op het Akamai CDN dat is geïntegreerd met Media Services streaming-eind punt. Het is echter wel beschikbaar voor Media Services klanten die een reeds bestaand Akamai-contract hebben en aangepaste integratie vereisen tussen Akamai CDN en de Media Services oorsprong.

### <a name="how-it-works"></a>Hoe werkt het?

CDN-ondersteuning voor `Origin-Assist CDN-Prefetch` de headers (voor Live en video on-demand streaming) is beschikbaar voor klanten die een direct-contract met Akamai CDN hebben. De functie omvat de volgende HTTP-header-uitwisselingen tussen Akamai CDN en de Media Services Origin:

|HTTP-header|Waarden|Afzender|Ontvanger|Doel|
| ---- | ---- | ---- | ---- | ----- |
|`CDN-Origin-Assist-Prefetch-Enabled` | 1 (standaard) of 0 |CDN|Oorsprong|Om aan te geven dat CDN is ingeschakeld.|
|`CDN-Origin-Assist-Prefetch-Path`| Voorbeeld: <br/>Fragmenten (video = 1400000000, Format = mpd-time-CMAF)|Oorsprong|CDN|Om het prefetch-pad naar CDN op te geven.|
|`CDN-Origin-Assist-Prefetch-Request`|1 (prefetch-aanvraag) of 0 (normale aanvraag)|CDN|Oorsprong|Om aan te geven dat de aanvraag van CDN een prefetch is.|

Als u een deel van de koptekst uitwisseling in actie wilt zien, kunt u de volgende stappen uitvoeren:

1. Gebruik postman of krul om een aanvraag uit te geven aan de Media Services oorsprong van een audio-of video segment of fragment. Zorg ervoor dat u de header `CDN-Origin-Assist-Prefetch-Enabled: 1` toevoegt in de aanvraag.
2. In het antwoord ziet u de koptekst `CDN-Origin-Assist-Prefetch-Path` met een relatief pad als waarde.

### <a name="supported-streaming-protocols"></a>Ondersteunde streaming protocollen

De `Origin-Assist CDN-Prefetch` functie ondersteunt de volgende streaming-protocollen voor Live en on-demand streaming:

* HLS v3
* HLS v4
* HLS CMAF
* STREEPJE (KVP)
* STREEPJE (CMAF)
* Vloeiend streamen

### <a name="faqs"></a>Veelgestelde vragen

* Wat gebeurt er als de URL van een prefetch-pad ongeldig is zodat CDN prefetch een 404 krijgt?

    CDN zal alleen gedurende 10 seconden (of een andere geconfigureerde waarde) een 404-antwoord in de cache opslaan.

* Stel dat u een video op aanvraag hebt. Als CDN-prefetch is ingeschakeld, impliceert deze functie dat wanneer een client het eerste video segment opvraagt, wordt door prefetch een lus gestart om alle volgende video segmenten met dezelfde bitrate te prefetch?

    Nee, CDN-prefetch wordt pas uitgevoerd na een aanvraag/antwoord dat door de client is gestart. CDN-prefetch wordt nooit geactiveerd door een prefetch, om een prefetch-lus te voor komen.

* Is oorsprong: de functie voor het vooraf gebruiken van CDN-prefetch is altijd ingeschakeld? Hoe kan deze worden in-of uitgeschakeld?

    Deze functie is standaard uitgeschakeld. Klanten moeten deze inschakelen via Akamai API.

* Wat gebeurt er met live streamen als het volgende segment of fragment nog niet beschikbaar is?

    In dit geval bieden `CDN-Origin-Assist-Prefetch-Path` de Media Services oorsprong geen header en kan CDN-prefetch niet worden uitgevoerd.

* Hoe `Origin-Assist CDN-Prefetch` werkt het met dynamische manifest filters?

    Deze functie werkt onafhankelijk van het manifest filter. Wanneer het volgende fragment zich buiten een filter venster bevindt, wordt de bijbehorende URL nog steeds gevonden door naar het onbewerkte client manifest te kijken en vervolgens te retour neren als de header van het prefetch-verzoek van de CDN. Daarom krijgt CDN de URL van een fragment dat wordt uitgefilterd op basis van een DASH-HLS/glad-manifest. De speler maakt echter nooit een GET-aanvraag naar CDN om het fragment op te halen, omdat dat fragment niet is opgenomen in het DASH-HLS/Smooth-manifest dat door de speler wordt bewaard (de speler weet niet dat het aanwezige fragment is).

* Kan streepje MPD/HLS-afspeel lijst/glad manifest vooraf worden opgehaald?

    Nee, streepje MPD, HLS, HLS variant List of Smooth manifest URL is niet toegevoegd aan de prefetch-header.

* Zijn prefetch-Url's relatief of absoluut?

    Hoewel Akamai CDN beide toestaat, biedt de Media Services oorsprong alleen relatieve Url's voor het prefetch-pad, omdat er geen voor deel is van het gebruik van absolute Url's.

* Werkt deze functie met inhoud die is beveiligd met DRM?

    Ja, omdat deze functie werkt op het HTTP-niveau, wordt geen segment/fragment gedecodeerd of geparseerd. Het maakt niet uit of de inhoud is versleuteld of niet.

* Werkt deze functie met AD-invoeging (SSAI) aan de server zijde?
    
    Dit geldt voor oorspronkelijke/hoofd inhoud (de oorspronkelijke video-inhoud vóór het invoegen van AD), omdat SSAI de tijds tempel van de bron inhoud niet wijzigt van de Media Services oorsprong. Of deze functie werkt met AD-inhoud, is afhankelijk van of AD Origin ondersteuning biedt voor oorsprong-assistentie. Als bijvoorbeeld de inhoud van de advertentie ook wordt gehost in Azure Media Services (zelfde of afzonderlijke oorsprong), wordt de inhoud van AD ook vooraf opgehaald.

* Werkt deze functie met de inhoud van UHD/HEVC?

    Ja.

## <a name="ask-questions-give-feedback-get-updates"></a>Vragen stellen, feedback geven, updates ophalen

Bekijk het [Azure Media Services Community](media-services-community.md) -artikel voor verschillende manieren om vragen te stellen, feedback te geven en updates te ontvangen over Media Services.

## <a name="next-steps"></a>Volgende stappen

* Zorg ervoor dat u het [streaming-eind punt document (origin)](streaming-endpoint-concept.md) bekijkt.
* Het voor beeld [in deze opslag plaats](https://github.com/Azure-Samples/media-services-v3-dotnet-quickstarts/blob/master/AMSV3Quickstarts/EncodeAndStreamFiles/Program.cs) laat zien hoe u het standaard streaming-eind punt met .net kunt starten.

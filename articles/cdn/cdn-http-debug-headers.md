---
title: X-EC-fouten opsporen in HTTP-headers voor Azure CDN-regel engine | Microsoft Docs
description: De header X-EC-debug debug cache-aanvraag bevat aanvullende informatie over het cache beleid dat wordt toegepast op de aangevraagde Asset. Deze headers zijn specifiek voor Verizon.
services: cdn
documentationcenter: ''
author: asudbring
manager: danielgi
editor: ''
ms.assetid: ''
ms.service: azure-cdn
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/12/2018
ms.author: allensu
ms.openlocfilehash: 52aae3bdd2fe82eea6cbd500723192c88c293a1e
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/28/2020
ms.locfileid: "81260493"
---
# <a name="x-ec-debug-http-headers-for-azure-cdn-rules-engine"></a>X-EC-fouten opsporen in HTTP-headers voor de engine van Azure CDN regels
De header debug cache-aanvraag `X-EC-Debug`, bevat aanvullende informatie over het cache beleid dat wordt toegepast op de aangevraagde Asset. Deze headers zijn specifiek voor **Azure CDN Premium van Verizon** -producten.

## <a name="usage"></a>Gebruik
Het antwoord van de POP-servers naar een gebruiker bevat alleen `X-EC-Debug` de header wanneer aan de volgende voor waarden wordt voldaan:

- De [functie debug cache response headers](cdn-verizon-premium-rules-engine-reference-features.md#debug-cache-response-headers) is ingeschakeld op de regel engine voor de opgegeven aanvraag.
- Met de opgegeven aanvraag wordt de set debug cache response headers gedefinieerd die worden opgenomen in het antwoord.

## <a name="requesting-debug-cache-information"></a>Cache gegevens voor fout opsporing aanvragen
Gebruik de volgende richt lijnen in de opgegeven aanvraag om de cache gegevens voor fout opsporing te definiëren die in het antwoord worden opgenomen:

Aanvraagheader | Beschrijving |
---------------|-------------|
X-EC-debug: x-EC-cache | [Cache status code](#cache-status-code-information)
X-EC-debug: x-EC-cache-Remote | [Cache status code](#cache-status-code-information)
X-EC-debug: x-EC-check-cache | [Gecachet kan worden](#cacheable-response-header)
X-EC-debug: x-EU-cache-sleutel | [Cache sleutel](#cache-key-response-header)
X-EC-debug: x-EC-cache-status | [Cache status](#cache-state-response-header)

### <a name="syntax"></a>Syntaxis

Fouten in de cache reactie van debug kunnen worden aangevraagd door de volgende header en de opgegeven instructies in de aanvraag op te nemen:

`X-EC-Debug: Directive1,Directive2,DirectiveN`

### <a name="sample-x-ec-debug-header"></a>Voor beeld X-EC-debug-header

`X-EC-Debug: x-ec-cache,x-ec-check-cacheable,x-ec-cache-key,x-ec-cache-state`

## <a name="cache-status-code-information"></a>Informatie over cache status code
De reactie header X-EC-debug kan een server identificeren en hoe deze het antwoord verwerkt via de volgende instructies:

Header | Beschrijving
-------|------------
X-EC-debug: x-EC-cache | Deze header wordt gerapporteerd wanneer de inhoud via het CDN wordt doorgestuurd. Hiermee wordt de POP-server geïdentificeerd die de aanvraag heeft voltooid.
X-EC-debug: x-EC-cache-Remote | Deze header wordt alleen gerapporteerd wanneer de aangevraagde inhoud in de cache is opgeslagen op een originele schild server of een ADN-Gateway server.

### <a name="response-header-format"></a>Indeling van de antwoord header

De header X-EC-debug rapporteert cache status code gegevens in de volgende indeling:

- `X-EC-Debug: x-ec-cache: <StatusCode from Platform (POP/ID)>`

- `X-EC-Debug: x-ec-cache-remote: <StatusCode from Platform (POP/ID)>`

De termen die in de bovenstaande syntaxis van de reactie header worden gebruikt, zijn als volgt gedefinieerd:
- Status code: Hiermee wordt aangegeven hoe de aangevraagde inhoud is verwerkt door het CDN, die wordt vertegenwoordigd door een cache statuscode.
    
    De TCP_DENIED status code kan in plaats van geen worden gerapporteerd wanneer een niet-geautoriseerde aanvraag wordt geweigerd vanwege verificatie op basis van tokens. De status code geen kan echter nog steeds worden gebruikt bij het weer geven van de cache status rapporten of onbewerkte logboek gegevens.

- Platform: geeft het platform aan waarop de inhoud is aangevraagd. De volgende codes zijn geldig voor dit veld:

    Code  | Platform
    ------| --------
    ECAcc | HTTP-groot
    ECS   | HTTP-klein
    ECD   | Application Delivery Network (ADN)

- POP: Hiermee wordt de [pop](cdn-pop-abbreviations.md) aangegeven waarmee de aanvraag is verwerkt. 

### <a name="sample-response-headers"></a>Voor beeld van antwoord headers

De volgende voorbeeld headers bieden informatie over de cache status code voor een aanvraag:

- `X-EC-Debug: x-ec-cache: TCP_HIT from ECD (lga/0FE8)`

- `X-EC-Debug: x-ec-cache-remote: TCP_HIT from ECD (dca/EF00)`

## <a name="cacheable-response-header"></a>Cacheable reactie header
De `X-EC-Debug: x-ec-check-cacheable` reactie header geeft aan of de aangevraagde inhoud in de cache kan worden opgeslagen.

Deze antwoord header geeft niet aan of het cache gebruik heeft plaatsgevonden. In plaats daarvan wordt aangegeven of de aanvraag in aanmerking komt voor caching.

### <a name="response-header-format"></a>Indeling van de antwoord header

De `X-EC-Debug` antwoord header rapporteert of een aanvraag in de cache is opgeslagen, heeft de volgende indeling:

`X-EC-Debug: x-ec-check-cacheable: <cacheable status>`

De term die wordt gebruikt in de bovenstaande syntaxis van de reactie header wordt als volgt gedefinieerd:

Waarde  | Beschrijving
-------| --------
JA    | Geeft aan dat de aangevraagde inhoud in aanmerking komt voor caching.
NO     | Geeft aan dat de aangevraagde inhoud niet in aanmerking komt voor caching. Deze status kan een van de volgende oorzaken hebben: <br /> -Klantspecifieke configuratie: een configuratie die specifiek is voor uw account kan verhinderen dat de POP-servers een asset in de cache opslaan. Regels engine kan bijvoorbeeld voor komen dat een asset in de cache wordt opgeslagen door de functie voor het overs laan van de cache in te scha kelen voor het kwalificeren van aanvragen.<br /> -Cache-antwoord headers: de aangevraagde kopteksten Cache-Control en Expires van het activum kunnen verhinderen dat de POP-servers deze in de cache opslaan.
UNKNOWN | Geeft aan dat de servers niet kunnen beoordelen of het aangevraagde activum in de cache kan worden opgeslagen. Deze status treedt meestal op wanneer de aanvraag wordt geweigerd vanwege verificatie op basis van tokens.

### <a name="sample-response-header"></a>Voor beeld van reactie header

De volgende voor beeld van een antwoord header geeft aan of de aangevraagde inhoud in de cache is opgeslagen:

`X-EC-Debug: x-ec-check-cacheable: YES`

## <a name="cache-key-response-header"></a>Cache-sleutel antwoord header
De `X-EC-Debug: x-ec-cache-key` antwoord header geeft de fysieke cache sleutel aan die is gekoppeld aan de aangevraagde inhoud. Een fysieke cache sleutel bestaat uit een pad dat een Asset identificeert voor het opslaan in de cache. Met andere woorden, de servers controleren op een versie van een asset in de cache op basis van het pad dat is gedefinieerd door de cache sleutel.

Deze fysieke cache sleutel begint met een dubbele slash (/), gevolgd door het protocol dat wordt gebruikt om de inhoud aan te vragen (HTTP of HTTPS). Dit protocol wordt gevolgd door het relatieve pad naar het aangevraagde activum, dat begint met het toegangs punt voor inhoud (bijvoorbeeld _/000001/_).

HTTP-platforms zijn standaard geconfigureerd voor het gebruik van *standaard-cache*, wat betekent dat query reeksen worden genegeerd door het mechanisme voor caching. Met dit type configuratie kan de cache sleutel geen query teken reeks gegevens opnemen.

Als een query reeks wordt vastgelegd in de cache sleutel, wordt deze geconverteerd naar het bijbehorende hash-equivalent en vervolgens ingevoegd tussen de naam van het aangevraagde activum en de bijbehorende bestands extensie (&lt;bijvoorbeeld activum&gt;hashwaarde. html).

### <a name="response-header-format"></a>Indeling van de antwoord header

De `X-EC-Debug` antwoord header rapporteert fysieke cache-sleutel gegevens in de volgende indeling:

`X-EC-Debug: x-ec-cache-key: CacheKey`

### <a name="sample-response-header"></a>Voor beeld van reactie header

De volgende voorbeeld reactie header geeft de fysieke cache sleutel voor de aangevraagde inhoud aan:

`X-EC-Debug: x-ec-cache-key: //http/800001/origin/images/foo.jpg`

## <a name="cache-state-response-header"></a>Cache status reactie header
De `X-EC-Debug: x-ec-cache-state` reactie header geeft de cache status van de aangevraagde inhoud op het tijdstip aan waarop deze is aangevraagd.

### <a name="response-header-format"></a>Indeling van de antwoord header

De `X-EC-Debug` antwoord header rapporteert cache status gegevens in de volgende indeling:

`X-EC-Debug: x-ec-cache-state: max-age=MASeconds (MATimePeriod); cache-ts=UnixTime (ddd, dd MMM yyyy HH:mm:ss GMT); cache-age=CASeconds (CATimePeriod); remaining-ttl=RTSeconds (RTTimePeriod); expires-delta=ExpiresSeconds`

De termen die in de bovenstaande syntaxis van de reactie header worden gebruikt, zijn als volgt gedefinieerd:

- MASeconds: geeft de maximale leeftijd (in seconden) aan, zoals gedefinieerd door de cache-control-headers van de aangevraagde inhoud.

- MATimePeriod: converteert de waarde voor de maximale leeftijd (MASeconds) naar het geschatte equivalent van een grotere eenheid (bijvoorbeeld dagen). 

- UnixTime: geeft de tijds tempel van de cache van de aangevraagde inhoud in de Unix-tijd aan (ook wel bekend als POSIX-tijd of UNIX-epoche). De time stamp van de cache geeft de begin datum/tijd aan van waaruit de TTL van een activum wordt berekend. 

    Als de oorspronkelijke server geen HTTP-cache server van derden gebruikt of als die server niet de header van de leeftijds reactie retourneert, is de cache-tijds tempel altijd de datum/tijd waarop de Asset is opgehaald of opnieuw gevalideerd. Als dat niet het geval is, gebruiken de POP-servers het veld leeftijd om de TTL van het activum als volgt te berekenen: ophalen/RevalidateDateTime-Age.

- ddd, DD MMM JJJJ uu: mm: SS GMT: Hiermee wordt de tijds tempel van de cache van de aangevraagde inhoud aangegeven. Zie voor meer informatie de bovenstaande UnixTime-term.

- CASeconds: geeft het aantal seconden aan dat is verstreken sinds de tijds tempel van de cache.

- RTSeconds: geeft het aantal seconden aan dat de in de cache geplaatste inhoud als vers wordt beschouwd. Deze waarde wordt als volgt berekend: RTSeconds = Max-Age-cache leeftijd.

- RTTimePeriod: converteert de resterende TTL-waarde (dat wil zeggen, RTSeconds) naar het geschatte equivalent van een grotere eenheid (bijvoorbeeld dagen).

- ExpiresSeconds: geeft het aantal seconden aan dat moet zijn vóór de datum/tijd die `Expires` in de antwoord header is opgegeven. Als de `Expires` antwoord header niet is opgenomen in het antwoord, is de waarde van deze term *geen*.

### <a name="sample-response-header"></a>Voor beeld van reactie header

De volgende voorbeeld reactie header geeft de cache status van de aangevraagde inhoud op het moment dat deze werd aangevraagd:

```X-EC-Debug: x-ec-cache-state: max-age=604800 (7d); cache-ts=1341802519 (Mon, 09 Jul 2012 02:55:19 GMT); cache-age=0 (0s); remaining-ttl=604800 (7d); expires-delta=none```


---
title: Verkeer op een kaart weer geven | Microsoft Azure kaarten
description: In dit artikel leert u hoe u verkeers gegevens op een kaart kunt weer geven met behulp van de Microsoft Azure Maps Web SDK.
author: Philmea
ms.author: philmea
ms.date: 07/29/2019
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: ''
ms.custom: codepen
ms.openlocfilehash: 9c17c3cc22d478d81ed3c2b2ae9f61c173aad6cd
ms.sourcegitcommit: a8ee9717531050115916dfe427f84bd531a92341
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/12/2020
ms.locfileid: "83123918"
---
# <a name="show-traffic-on-the-map"></a>Verkeer op de kaart weer geven

Er zijn twee soorten verkeers gegevens beschikbaar in Azure Maps:

- Incident gegevens: bestaat uit gegevens op basis van een punt en lijn voor dingen, zoals bouw, wegsluitingen en ongel ukken.
- Stroom gegevens: voorziet in metrische informatie over de stroom van verkeer op de wegen. De verkeers stroom gegevens worden vaak gebruikt om de wegen te kleuren. De kleuren zijn gebaseerd op de hoeveelheid verkeer die de stroom vertraagt, ten opzichte van de snelheids limiet of een andere metriek. De gegevens van de verkeers stroom in Azure Maps hebben drie verschillende meet waarden:
    - `relative`-is relatief ten opzichte van de vrije stroom snelheid van de weg.
    - `absolute`-is de absolute snelheid van alle Voer tuigen op de weg.
    - `relative-delay`-gebieden weer geven die langzamer zijn dan de gemiddelde verwachte vertraging.

De volgende code laat zien hoe verkeers gegevens op de kaart worden weer gegeven.

```javascript
//Show traffic on the map using the traffic options.
map.setTraffic({
    incidents: true,
    flow: 'relative'
});
```

Hieronder ziet u het volledige programma voor het uitvoeren van code van de bovenstaande functionaliteit.

<br/>

<iframe height='500' scrolling='no' title='Verkeer op een kaart weer geven' src='//codepen.io/azuremaps/embed/WMLRPw/?height=500&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Zie het <a href='https://codepen.io/azuremaps/pen/WMLRPw/'>netwerk verkeer weer geven op een kaart</a> door Azure Maps ( <a href='https://codepen.io/azuremaps'>@azuremaps</a> ) op <a href='https://codepen.io'>CodePen</a>.
</iframe>

## <a name="traffic-overlay-options"></a>Opties voor Traffic overlay

Met het volgende hulp programma kunt u scha kelen tussen de verschillende instellingen van Traffic overlay om te zien hoe de rendering verandert. 

<br/>

<iframe height="700" style="width: 100%;" scrolling="no" title="Opties voor Traffic overlay" src="//codepen.io/azuremaps/embed/RwbPqRY/?height=700&theme-id=0&default-tab=result" frameborder="no" allowtransparency="true" allowfullscreen="true">
Zie de <a href='https://codepen.io/azuremaps/pen/RwbPqRY/'>bedekkings opties</a> voor pen-verkeer per Azure Maps ( <a href='https://codepen.io/azuremaps'>@azuremaps</a> ) op <a href='https://codepen.io'>CodePen</a>.
</iframe>

## <a name="next-steps"></a>Volgende stappen

Meer informatie over de klassen en methoden die in dit artikel worden gebruikt:

> [!div class="nextstepaction"]
> [Kaart](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map)

> [!div class="nextstepaction"]
> [TrafficOptions](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.trafficoptions)

Verbeter uw gebruikers ervaring:

> [!div class="nextstepaction"]
> [Interactie met muis gebeurtenissen koppelen](map-events.md)

> [!div class="nextstepaction"]
> [Een toegankelijke kaart bouwen](map-accessibility.md)

> [!div class="nextstepaction"]
> [Voorbeeld pagina voor code](https://aka.ms/AzureMapsSamples)

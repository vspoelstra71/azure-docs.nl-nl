---
title: Informatie over een coördinaat op een kaart weer geven | Microsoft Azure kaarten
description: Informatie over het weer geven van gegevens over een adres op de kaart wanneer een gebruiker een coördinaat selecteert.
author: Philmea
ms.author: philmea
ms.date: 07/29/2019
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: ''
ms.custom: codepen
ms.openlocfilehash: 274d1102649dade1b3295bd02feba03f64a26ad5
ms.sourcegitcommit: a8ee9717531050115916dfe427f84bd531a92341
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/12/2020
ms.locfileid: "83123986"
---
# <a name="get-information-from-a-coordinate"></a>Informatie ophalen uit een coördinaat

In dit artikel wordt beschreven hoe u een omgekeerde adres zoekactie kunt maken met het adres van een klik op de pop-upbesturingselementnaam.

Er zijn twee manieren om een omgekeerde adres zoekactie te maken. Een manier is om de [Azure Maps reverse lookup-API](https://docs.microsoft.com/rest/api/maps/search/getsearchaddressreverse) van het adres te doorzoeken via een service module. De andere manier is om met behulp van de [ophaal-API](https://fetch.spec.whatwg.org/) een aanvraag in te stellen voor de [Azure Maps reverse lookup-API](https://docs.microsoft.com/rest/api/maps/search/getsearchaddressreverse) om een adres te vinden. Beide manieren worden hieronder weer gegeven.

## <a name="make-a-reverse-search-request-via-service-module"></a>Een Reverse Search-aanvraag maken via de service module

<iframe height='500' scrolling='no' title='Informatie ophalen uit een coördinaat (Service module)' src='//codepen.io/azuremaps/embed/ejEYMZ/?height=265&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Zie de pen <a href='https://codepen.io/azuremaps/pen/ejEYMZ/'>informatie ophalen uit een coördinaat (Service module)</a> door Azure Maps ( <a href='https://codepen.io/azuremaps'>@azuremaps</a> ) op <a href='https://codepen.io'>CodePen</a>.
</iframe>

In de bovenstaande code maakt het eerste blok een kaart object en stelt het verificatie mechanisme in voor het gebruik van het toegangs token. U kunt [een overzicht maken](./map-create.md) voor instructies.

Het tweede code blok maakt een `TokenCredential` voor de verificatie van HTTP-aanvragen aan Azure Maps met het toegangs token. Vervolgens wordt door gegeven `TokenCredential` aan `atlas.service.MapsURL.newPipeline()` en wordt een [pijplijn](https://docs.microsoft.com/javascript/api/azure-maps-rest/atlas.service.pipeline?view=azure-maps-typescript-latest) instantie gemaakt. De `searchURL` vertegenwoordigt een URL voor het Azure Maps van [Zoek](https://docs.microsoft.com/rest/api/maps/search) bewerkingen.

Met het derde code blok wordt de stijl van de muis aanwijzer naar een wijzer bijgewerkt en wordt een [pop](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.popup?view=azure-iot-typescript-latest#open) -upobject gemaakt. U kunt [een pop-upvenster toevoegen op de kaart](./map-add-popup.md) voor instructies.

Het vierde code blok voegt een gebeurtenislistener toe aan een [muis klik.](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest#events) Als deze wordt geactiveerd, wordt er een zoek query gemaakt met de coördinaten van het geklikte punt. Vervolgens wordt de methode [getSearchAddressReverse](https://docs.microsoft.com/javascript/api/azure-maps-rest/atlas.service.searchurl?view=azure-iot-typescript-latest#searchaddressreverse-aborter--geojson-position--searchaddressreverseoptions-)gebruikt om een query uit te zoeken naar de [Reverse-API Get-adres](https://docs.microsoft.com/rest/api/maps/search/getsearchaddressreverse) voor het adres van de coördinaten. Er wordt vervolgens een verzameling geojson-onderdelen geëxtraheerd met behulp `geojson.getFeatures()` van de methode van het antwoord.

Met het vijfde code blok wordt de inhoud van het HTML-pop-upvenster ingesteld om het antwoord adres voor de geklikte coördinaat positie weer te geven.

De wijziging van de cursor, het pop-upobject en de Click-gebeurtenis worden allemaal gemaakt in de [Load event-listener](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest#events)van de kaart. Deze code structuur zorgt voor volledige belasting van de kaart voordat de coördinaten gegevens worden opgehaald.

## <a name="make-a-reverse-search-request-via-fetch-api"></a>Een Reverse Search-aanvraag indienen via de API voor ophalen

Klik op de kaart om een reverse Geocode-aanvraag voor die locatie te maken met behulp van ophalen.

<iframe height='500' scrolling='no' title='Informatie ophalen uit een coördinaat' src='//codepen.io/azuremaps/embed/ddXzoB/?height=516&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Zie de pen <a href='https://codepen.io/azuremaps/pen/ddXzoB/'>informatie ophalen uit een coördinaat</a> door Azure Maps ( <a href='https://codepen.io/azuremaps'>@azuremaps</a> ) op <a href='https://codepen.io'>CodePen</a>.
</iframe>

In de bovenstaande code bouwt het eerste code blok een kaart object in en stelt het verificatie mechanisme in voor het gebruik van het toegangs token. U kunt [een overzicht maken](./map-create.md) voor instructies.

Met het tweede code blok wordt de stijl van de muis aanwijzer bijgewerkt naar een wijzer. Er wordt een [pop](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.popup?view=azure-iot-typescript-latest#open) -upobject gemaakt. U kunt [een pop-upvenster toevoegen op de kaart](./map-add-popup.md) voor instructies.

Het derde code blok voegt een gebeurtenislistener toe voor muis klikken. Wanneer u op een muis klik klikt, wordt de [ophaal-API](https://fetch.spec.whatwg.org/) gebruikt om een query uit te [zoeken naar Azure Maps reverse lookup-API](https://docs.microsoft.com/rest/api/maps/search/getsearchaddressreverse) voor het geklikte coördinaten adres. Voor een geslaagde respons wordt het adres voor de geklikte locatie verzameld. Hiermee worden de pop-upinhoud en-positie gedefinieerd met behulp van de functie [SetOption](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.popup?view=azure-iot-typescript-latest#setoptions-popupoptions-) van de klasse pop-up.

De wijziging van de cursor, het pop-upobject en de Click-gebeurtenis worden allemaal gemaakt in de [Load event-listener](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest#events)van de kaart. Deze code structuur zorgt ervoor dat de kaart volledig wordt geladen voordat de coördinaten gegevens worden opgehaald.

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Aanbevolen procedures voor het gebruik van de zoek service](how-to-use-best-practices-for-search.md)

Meer informatie over de klassen en methoden die in dit artikel worden gebruikt:

> [!div class="nextstepaction"]
> [Kaart](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest)

> [!div class="nextstepaction"]
> [Popup](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.popup?view=azure-iot-typescript-latest)

Raadpleeg de volgende artikelen voor voor beelden van volledige code:

> [!div class="nextstepaction"]
> [Routebeschrijving van A naar B](./map-route.md)

> [!div class="nextstepaction"]
> [Verkeer weergeven](./map-show-traffic.md)

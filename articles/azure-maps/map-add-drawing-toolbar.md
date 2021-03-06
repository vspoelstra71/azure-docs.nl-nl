---
title: Een werk balk tekenen toevoegen aan een kaart | Microsoft Azure kaarten
description: Een werk balk voor tekenen toevoegen aan een kaart met Azure Maps Web SDK
author: philmea
ms.author: philmea
ms.date: 09/04/2019
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: philmea
ms.openlocfilehash: bebf1ddfbca3aec5a551193609381cf3510bc3ac
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/28/2020
ms.locfileid: "80334488"
---
# <a name="add-a-drawing-tools-toolbar-to-a-map"></a>Een werk balk voor teken hulpprogramma's toevoegen aan een kaart

In dit artikel wordt beschreven hoe u de module teken Hulpprogramma's gebruikt en hoe u de werk balk tekenen op de kaart weergeeft. Met het besturings element [DrawingToolbar](https://docs.microsoft.com/javascript/api/azure-maps-drawing-tools/atlas.control.drawingtoolbar?view=azure-node-latest) wordt de werk balk tekenen op de kaart toegevoegd. U leert hoe u kaarten met slechts één en alle teken hulpmiddelen maakt en hoe u de rendering van de teken vormen in de tekening Manager aanpast.

## <a name="add-drawing-toolbar"></a>Tekenwerkbalk toevoegen

Met de volgende code wordt een exemplaar van de tekening Manager gemaakt en wordt de werk balk op de kaart weer gegeven.

```javascript
//Create an instance of the drawing manager and display the drawing toolbar.
drawingManager = new atlas.drawing.DrawingManager(map, {
        toolbar: new atlas.control.DrawingToolbar({
            position: 'top-right',
            style: 'dark'
        })
    });
```

Hieronder ziet u het volledige uitvoerings voorbeeld code van de bovenstaande functies:

<br/>

<iframe height="500" style="width: 100%;" scrolling="no" title="Tekenwerkbalk toevoegen" src="//codepen.io/azuremaps/embed/ZEzLeRg/?height=265&theme-id=0&default-tab=js,result&editable=true" frameborder="no" allowtransparency="true" allowfullscreen="true">
Ga naar de pen <a href='https://codepen.io/azuremaps/pen/ZEzLeRg/'>werk balk tekening toevoegen</a> op<a href='https://codepen.io/azuremaps'>@azuremaps</a>Azure Maps () op <a href='https://codepen.io'>CodePen</a>.
</iframe>


## <a name="limit-displayed-toolbar-options"></a>Opties voor weer gave van werk balken beperken

Met de volgende code wordt een exemplaar van het teken beheer gemaakt en wordt de werk balk met een veelhoek tekening op de kaart weer gegeven. 

```javascript
//Create an instance of the drawing manager and display the drawing toolbar with polygon drawing tool.
drawingManager = new atlas.drawing.DrawingManager(map, {
        toolbar: new atlas.control.DrawingToolbar({
            position: 'top-right',
            style: 'light',
            buttons: ["draw-polygon"]
        })
    });
```

Hieronder ziet u het volledige uitvoerings voorbeeld code van de bovenstaande functies:

<br/>

<iframe height="500" style="width: 100%;" scrolling="no" title="Een hulp programma voor veelhoek tekenen toevoegen" src="//codepen.io/azuremaps/embed/OJLWWMy/?height=265&theme-id=0&default-tab=js,result&editable=true" frameborder="no" allowtransparency="true" allowfullscreen="true">
Zie de pen <a href='https://codepen.io/azuremaps/pen/OJLWWMy/'>een hulp programma voor veelhoek tekenen toevoegen</a> door<a href='https://codepen.io/azuremaps'>@azuremaps</a>Azure Maps () op <a href='https://codepen.io'>CodePen</a>.
</iframe>


## <a name="change-drawing-rendering-style"></a>Stijl van tekening weergave wijzigen

De stijl van de getekende vormen kan worden aangepast door de onderliggende lagen van de tekening Manager op te halen met behulp van de `drawingManager.getLayers()` functie en vervolgens opties in te stellen voor de afzonderlijke lagen. De Sleep grepen die voor coördinaten worden weer gegeven bij het bewerken van een vorm, zijn HTML-markeringen. De stijl van de Sleep grepen kan worden aangepast door HTML-markerings opties door `dragHandleStyle` te `secondaryDragHandleStyle` geven in de opties en van de teken beheer.  

Met de volgende code worden de weergave lagen opgehaald uit de tekening Manager en worden de opties voor het wijzigen van de stijl van de tekening gewijzigd. In dit geval worden punten weer gegeven met een pictogram met een blauwe markering. Lijnen worden rood en vier pixels breed. Veelhoeken hebben een groene opvul kleur en een oranje overzicht. Vervolgens worden de stijlen van de Sleep grepen gewijzigd in vier Kante pictogrammen. 

```javascript
//Get rendering layers of drawing manager.
var layers = drawingManager.getLayers();

//Change the icon rendered for points.
layers.pointLayer.setOptions({
    iconOptions: {
        image: 'marker-blue'
    }
});

//Change the color and width of lines.
layers.lineLayer.setOptions({
    strokeColor: 'red',
    strokeWidth: 4
});

//Change fill color of polygons.
layers.polygonLayer.setOptions({
    fillColor: 'green'
});

//Change the color of polygon outlines.
layers.polygonOutlineLayer.setOptions({
    strokeColor: 'orange'
});

//Update the style of the drag handles that appear when editting.
drawingManager.setOptions({
    //Primary drag handle that represents coordinates in the shape.
    dragHandleStyle: {
        anchor: 'center',
        htmlContent: '<svg width="15" height="15" viewBox="0 0 15 15" xmlns="http://www.w3.org/2000/svg" style="cursor:pointer"><rect x="0" y="0" width="15" height="15" style="stroke:black;fill:white;stroke-width:4px;"/></svg>',
        draggable: true
    },

    //Secondary drag hanle that represents mid-point coordinates that users can grab to add new cooridnates in the middle of segments.
    secondaryDragHandleStyle: {
        anchor: 'center',
        htmlContent: '<svg width="10" height="10" viewBox="0 0 10 10" xmlns="http://www.w3.org/2000/svg" style="cursor:pointer"><rect x="0" y="0" width="10" height="10" style="stroke:white;fill:black;stroke-width:4px;"/></svg>',
        draggable: true
    }
});  
```

Hieronder ziet u het volledige uitvoerings voorbeeld code van de bovenstaande functies:

<br/>

<iframe height="500" style="width: 100%;" scrolling="no" title="Stijl van tekening weergave wijzigen" src="//codepen.io/azuremaps/embed/OJLWpyj/?height=265&theme-id=0&default-tab=js,result&editable=true" frameborder="no" allowtransparency="true" allowfullscreen="true">
Bekijk de stijl voor het <a href='https://codepen.io/azuremaps/pen/OJLWpyj/'>weer geven van wijzigingen</a> in<a href='https://codepen.io/azuremaps'>@azuremaps</a>de pen op Azure Maps () op <a href='https://codepen.io'>CodePen</a>.
</iframe>


## <a name="next-steps"></a>Volgende stappen

Meer informatie over het gebruik van aanvullende functies van de module teken hulpprogramma's:

> [!div class="nextstepaction"]
> [Vormgegevens ophalen](map-get-shape-data.md)

> [!div class="nextstepaction"]
> [Reageren op tekengebeurtenissen](drawing-tools-events.md)

> [!div class="nextstepaction"]
> [Interactietypen en sneltoetsen](drawing-tools-interactions-keyboard-shortcuts.md)

Meer informatie over de klassen en methoden die in dit artikel worden gebruikt:

> [!div class="nextstepaction"]
> [Kaart](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest)

> [!div class="nextstepaction"]
> [Werk balk tekenen](https://docs.microsoft.com/javascript/api/azure-maps-drawing-tools/atlas.control.drawingtoolbar?view=azure-node-latest)

> [!div class="nextstepaction"]
> [Drawing Manager](https://docs.microsoft.com/javascript/api/azure-maps-drawing-tools/atlas.drawing.drawingmanager?view=azure-node-latest)

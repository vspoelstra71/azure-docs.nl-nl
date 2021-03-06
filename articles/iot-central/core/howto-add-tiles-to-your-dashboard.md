---
title: Tegels toevoegen aan uw Azure IoT Central-dash board | Microsoft Docs
description: Als ontwerper leert u hoe u de standaard Azure IoT Central Application dash board kunt configureren met tegels.
author: Haley-Rowland
ms.author: harowl
ms.date: 05/27/2020
ms.topic: how-to
ms.service: iot-central
services: iot-central
ms.openlocfilehash: 892bdcc08bd19b92c8b3d32d2954583f80005e87
ms.sourcegitcommit: 053e5e7103ab666454faf26ed51b0dfcd7661996
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/27/2020
ms.locfileid: "84022861"
---
# <a name="configure-the-application-dashboard"></a>Het toepassings dashboard configureren

Het **dash board** is de eerste pagina die u ziet wanneer u verbinding maakt met een IOT Central-toepassing. Als u uw toepassing maakt vanuit een van de [toepassingen](./concepts-app-templates.md)die in de praktijk zijn gericht, heeft uw toepassing een vooraf gedefinieerd dash board om te starten. Als u uw toepassing maakt op basis van een aangepaste [toepassings sjabloon](./concepts-app-templates.md), bevat uw dash board enkele tips om aan de slag te gaan.

> [!TIP]
> Gebruikers kunnen naast het standaard toepassings dashboard [meerdere Dash boards maken](howto-create-personal-dashboards.md) . Deze Dash boards kunnen alleen persoonlijk zijn voor de gebruiker of worden gedeeld met alle gebruikers van de toepassing.  

## <a name="add-tiles"></a>Tegels toevoegen

In de volgende scherm afbeelding ziet u het dash board in een toepassing die is gemaakt op basis van de sjabloon voor **aangepaste toepassingen** . Als u het huidige dash board wilt aanpassen, selecteert u **bewerken**, selecteert u **Nieuw**persoonlijk of gedeeld dash board toevoegen.

:::image type="content" source="media/howto-add-tiles-to-your-dashboard/dashboard-sample-contoso.png" alt-text="Dash board voor toepassingen op basis van de sjabloon voor aangepaste toepassingen":::

Nadat u **bewerken** of **Nieuw**hebt geselecteerd, bevindt het dash board zich in de *bewerkings* modus. U kunt de hulpprogram ma's in het deel venster **dash board bewerken** gebruiken om tegels aan het dash board toe te voegen en tegels op het dash board zelf aan te passen en te verwijderen. Als u bijvoorbeeld een **telemetrie** -tegel wilt toevoegen om de huidige Tempe ratuur weer te geven die wordt gerapporteerd door een of meer apparaten:

1. Selecteer een **apparaatgroep**in het deel venster **dash board bewerken** .
1. Selecteer een of meer apparaten in de vervolg keuzelijst **apparaten** om weer te geven op de tegel. U ziet nu de beschik bare telemetrie, eigenschappen en opdrachten van de apparaten.
1. Selecteer **Tempe ratuur** in het gedeelte telemetrie en selecteer vervolgens **tegel toevoegen**. De tegel wordt nu weer gegeven op het dash board, waar u de visualisatie kunt wijzigen, het formaat van de tegel wijzigt en deze configureert:

:::image type="content" source="media/howto-add-tiles-to-your-dashboard/device-details.png" alt-text="Een tweemetrische temperatuur tegel toevoegen aan het dash board":::

Wanneer u klaar bent met het toevoegen en aanpassen van tegels op het dash board, selecteert u **Opslaan**.

## <a name="customize-tiles"></a>Tegels aanpassen

Als u een tegel op het dash board wilt aanpassen, moet het dash board zich in de bewerkings modus bevindt. De beschik bare aanpassings opties zijn afhankelijk van het [type tegel](#tile-types):

* Met het pictogram liniaal op een tegel kunt u de visualisatie wijzigen. Visualisaties zijn onder andere lijn diagrammen, laatste bekende waarden en heatmap.

* Met het pictogram met het vier kant kunt u het formaat van de tegel wijzigen.

* Met het tandwiel pictogram kunt u de visualisatie configureren. Voor een lijn diagram visualisatie kunt u bijvoorbeeld kiezen om de legenda en assen weer te geven en het tijds bereik te kiezen dat u wilt uitzetten.

## <a name="tile-types"></a>Tegel typen

In de volgende tabel worden de verschillende typen tegels beschreven die u aan een dash board kunt toevoegen:

| Tegel             | Beschrijving |
| ---------------- | ----------- |
| Markdown         | Geprijsde tegels kunnen worden weer gegeven op tegels met een kop en beschrijving tekst die is opgemaakt met prijs verlaging. De URL kan een relatieve koppeling zijn naar een andere pagina in de toepassing of een absolute koppeling naar een externe site.|
| Installatiekopie            | Afbeeldings tegels geven een aangepaste afbeelding weer en kunnen worden geklikt. De URL kan een relatieve koppeling zijn naar een andere pagina in de toepassing of een absolute koppeling naar een externe site.|
| Label            | Met label tegels wordt aangepaste tekst op een dash board weer gegeven. U kunt de grootte van de tekst kiezen. Gebruik een label tegel om relevante informatie toe te voegen aan het dash board, zoals beschrijvingen, contact gegevens of Help.|
| Count            | Met de tegels tellen wordt het aantal apparaten in een apparaatgroep weer gegeven.|
| Kaart              | Kaart tegels geven de locatie weer van een of meer apparaten op een kaart. U kunt ook Maxi maal 100 punten van de locatie geschiedenis van een apparaat weer geven. U kunt bijvoorbeeld een bemonsterde route weer geven van waar een apparaat zich in de afgelopen week bevindt.|
| KPI              |  KPI-tegels geven geaggregeerde telemetrie-waarden weer voor een of meer apparaten gedurende een bepaalde periode. U kunt dit bijvoorbeeld gebruiken om de maximale Tempe ratuur en druk voor een of meer apparaten in het afgelopen uur weer te geven.|
| Lijndiagram       | Met tegels in lijn diagrammen worden een of meer geaggregeerde telemetriegegevens voor een of meer apparaten voor een bepaalde periode getekend. U kunt bijvoorbeeld een lijn diagram weer geven om de gemiddelde Tempe ratuur en druk van een of meer apparaten in het afgelopen uur af te zetten.|
| Staafdiagram        | In staafdiagram tegels worden een of meer geaggregeerde telemetriegegevens voor een of meer apparaten voor een bepaalde periode getekend. U kunt bijvoorbeeld een staaf diagram weer geven om de gemiddelde Tempe ratuur en druk van een of meer apparaten in het afgelopen uur weer te geven.|
| Cirkeldiagram        | Cirkel diagram tegels geven een of meer geaggregeerde telemetriegegevens voor een of meer apparaten voor een bepaalde periode weer.|
| Heatmap         | Heatmap tegels geven informatie weer over een of meer apparaten, weer gegeven als kleuren.|
| Laatst bekende waarde | De laatste bekende waarde-tegels geven de laatste telemetriegegevens voor een of meer apparaten weer. U kunt deze tegel bijvoorbeeld gebruiken om de meest recente waarden voor de Tempe ratuur, druk en lucht vochtigheid voor een of meer apparaten weer te geven. |
| Gebeurtenis geschiedenis    | Met de tegels van gebeurtenis geschiedenis worden de gebeurtenissen voor een apparaat gedurende een bepaalde periode weer gegeven. U kunt dit bijvoorbeeld gebruiken om alle kleppen open-en Close-gebeurtenissen weer te geven voor een of meer apparaten tijdens het laatste uur.|
| Eigenschap         |  Eigenschappen tegels geven de huidige waarde weer voor eigenschappen en Cloud eigenschappen van een of meer apparaten. U kunt deze tegel bijvoorbeeld gebruiken om apparaateigenschappen weer te geven, zoals de fabrikant of firmware versie voor een apparaat. |

Op dit moment kunt u Maxi maal 10 apparaten toevoegen aan tegels die ondersteuning bieden voor meerdere apparaten.

### <a name="customizing-visualizations"></a>Visualisaties aanpassen

Voor tegels die statistische waarden weer geven, selecteert u het tandwiel pictogram naast het type telemetrie in het deel venster **grafiek configureren** om de aggregatie te kiezen. U kunt kiezen uit gemiddelde, som, maximum, minimum en aantal.

Voor lijn diagrammen, staaf diagrammen en cirkel diagrammen kunt u de kleur van de verschillende telemetrie-waarden aanpassen. Selecteer het palet pictogram naast de telemetrie die u wilt aanpassen:

:::image type="content" source="media/howto-add-tiles-to-your-dashboard/color-customization.png" alt-text="De kleur van een telemetrie-waarde wijzigen":::

Voor tegels die teken reeks eigenschappen of telemetrie-waarden weer geven, kunt u kiezen hoe u de tekst wilt weer geven. Als het apparaat bijvoorbeeld een URL in een teken reeks eigenschap opslaat, kunt u deze weer geven als een klikbare koppeling. Als de URL verwijst naar een afbeelding, kunt u de installatie kopie weer geven in een laatste bekende waarde of eigenschaps tegel. Als u wilt wijzigen hoe een teken reeks wordt weer gegeven, selecteert u in de tegel configuratie het tandwiel pictogram naast het type telemetrie of de eigenschap:

:::image type="content" source="media/howto-add-tiles-to-your-dashboard/string-customization.png" alt-text="Bepalen hoe een teken reeks op een tegel wordt weer gegeven":::

## <a name="next-steps"></a>Volgende stappen

Nu u hebt geleerd hoe u uw Azure IoT Central standaard toepassings dashboard configureert, kunt u [meer informatie over het maken van een persoonlijk dash board](howto-create-personal-dashboards.md).

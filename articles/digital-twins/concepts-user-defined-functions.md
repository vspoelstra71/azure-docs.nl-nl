---
title: Gegevens verwerking en door de gebruiker gedefinieerde functies-Azure Digital Apparaatdubbels | Microsoft Docs
description: Overzicht van gegevens verwerking,-overeenkomsten en door de gebruiker gedefinieerde functies met Azure Digital Apparaatdubbels.
ms.author: alinast
author: alinamstanciu
manager: bertvanhoof
ms.service: digital-twins
services: digital-twins
ms.topic: conceptual
ms.date: 01/03/2020
ms.openlocfilehash: 75ed2029582438ede43687addfd54c0a187e0120
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/28/2020
ms.locfileid: "79265179"
---
# <a name="data-processing-and-user-defined-functions"></a>Gegevensverwerking en door gebruikers gedefinieerde functies

Azure Digital Apparaatdubbels biedt geavanceerde reken mogelijkheden. Ontwikkel aars kunnen aangepaste functies definiëren en uitvoeren voor inkomende telemetriegegevens voor het verzenden van gebeurtenissen naar vooraf gedefinieerde eind punten.

## <a name="data-processing-flow"></a>Gegevens verwerkings stroom

Nadat apparaten telemetriegegevens naar Azure Digital Apparaatdubbels hebben verzonden, kunnen ontwikkel aars gegevens in vier fasen verwerken: *Validate*, *match*, *Compute*en *Dispatch*.

[![Gegevens verwerkings stroom Azure Digital Apparaatdubbels](media/concepts/digital-twins-data-processing-flow.png)](media/concepts/digital-twins-data-processing-flow.png#lightbox)

1. Met de validate-fase wordt het inkomende telemetriegegevens getransformeerd naar een veelgebruikte indeling voor [gegevens overdracht](https://docs.microsoft.com/aspnet/web-api/overview/data/using-web-api-with-entity-framework/part-5) . Deze fase voert ook de validatie van het apparaat en de sensor uit.
1. De match-fase vindt de juiste door de gebruiker gedefinieerde functies die moeten worden uitgevoerd. Vooraf gedefinieerde overeenkomsten zoeken de door de gebruiker gedefinieerde functies op basis van het apparaat, de sensor en de ruimte-informatie uit het binnenkomende telemetrie-bericht.
1. De reken fase voert de door de gebruiker gedefinieerde functies uit die in de vorige fase overeenkomen. Deze functies kunnen berekende waarden op knoop punten van ruimtelijke grafieken lezen en bijwerken en kunnen aangepaste meldingen verzenden.
1. In de verzend fase worden alle aangepaste meldingen van de reken fase gerouteerd naar eind punten die in de grafiek zijn gedefinieerd.

## <a name="data-processing-objects"></a>Gegevens verwerkings objecten

Gegevens verwerking in azure Digital Apparaatdubbels bestaat uit het definiëren van drie objecten: *matchers*, door de *gebruiker gedefinieerde functies*en *roltoewijzingen*.

[![Azure Digital Apparaatdubbels data processing Objects](media/concepts/digital-twins-user-defined-functions.png)](media/concepts/digital-twins-user-defined-functions.png#lightbox)

### <a name="matchers"></a>Overeenkomst

Met matchers definieert u een set voor waarden die evalueren welke acties worden uitgevoerd op basis van de inkomende sensor-telemetrie. Voor waarden om de overeenkomst te bepalen kan eigenschappen van de sensor, het bovenliggende apparaat van de sensor en de bovenliggende ruimte van de sensor bevatten. De voor waarden worden uitgedrukt als vergelijkingen voor een [JSON-pad](https://jsonpath.com/) zoals beschreven in dit voor beeld:

- Alle Sens oren van gegevens type **temperatuur** vertegenwoordigd door de teken reeks waarde escaped`\"Temperature\"`
- Hun `01` poort hebben
- Die deel uitmaken van apparaten met de uitgebreide eigenschap sleutel **fabrikant** ingesteld op de teken reeks waarde escaped`\"Contoso\"`
- Die horen bij spaties van het type dat is opgegeven door de escape teken reeks`\"Venue\"`
- Wat zijn descendanten van bovenliggende **SpaceId**`DE8F06CA-1138-4AD7-89F4-F782CC6F69FD`

```JSON
{
  "id": "23535afafd-f39b-46c0-9b0c-0dd3892a1c30",
  "name": "My custom matcher",
  "spaceId": "DE8F06CA-1138-4AD7-89F4-F782CC6F69FD",
  "description": "All sensors of datatype Temperature with 01 in their port that belong to devices with the extended property key Manufacturer set to the value Contoso and that belong to spaces of type Venue that are somewhere below space Id DE8F06CA-1138-4AD7-89F4-F782CC6F69FD",
  "conditions": [
    {
      "id": "43898sg43-e15a-4e9c-abb8-2gw464364",
      "target": "Sensor",
      "path": "$.dataType",
      "value": "\"Temperature\"",
      "comparison": "Equals"
    },
    {
      "id": "wt3th44-e15a-35sg-seg3-235wf3ga463",
      "target": "Sensor",
      "path": "$.port",
      "value": "01",
      "comparison": "Contains"
    },
    {
      "id": "735hs33-e15a-37jj-23532-db901d550af5",
      "target": "SensorDevice",
      "path": "$.properties[?(@.name == 'Manufacturer')].value",
      "value": "\"Contoso\"",
      "comparison": "Equals"
    },
    {
      "id": "222325-e15a-49fg-5744-463643644",
      "target": "SensorSpace",
      "path": "$.type",
      "value": "\"Venue\"",
      "comparison": "Equals"
    }
  ]
}
```

> [!IMPORTANT]
> - JSON-paden zijn hoofdletter gevoelig.
> - De JSON-nettolading is hetzelfde als de nettolading die wordt geretourneerd door:
>   - `/sensors/{id}?includes=properties,types`voor de sensor.
>   - `/devices/{id}?includes=properties,types,sensors,sensorsproperties,sensorstypes`voor het bovenliggende apparaat van de sensor.
>   - `/spaces/{id}?includes=properties,types,location,timezone`voor de bovenliggende ruimte van de sensor.
> - De vergelijkingen zijn niet hoofdletter gevoelig.

### <a name="user-defined-functions"></a>Door de gebruiker gedefinieerde functies

Een door de gebruiker gedefinieerde functie is een aangepaste functie die wordt uitgevoerd in een geïsoleerde omgeving met een digitale Apparaatdubbels van Azure. Door de gebruiker gedefinieerde functies hebben toegang tot een bericht van de onbewerkte sensor-telemetrie, zodra het wordt ontvangen. Door de gebruiker gedefinieerde functies hebben ook toegang tot de spatiale grafiek en de dispatcher-service. Nadat de door de gebruiker gedefinieerde functie is geregistreerd binnen een grafiek, moet er een overeenkomende functie (beschreven [hierboven](#matchers)) worden gemaakt om op te geven wanneer de functies worden uitgevoerd. Als bijvoorbeeld Azure Digital Apparaatdubbels nieuwe telemetrie van een bepaalde sensor ontvangt, kan de overeenkomende door de gebruiker gedefinieerde functie een zwevend gemiddelde berekenen van de laatste lees bewerkingen van de sensor.

Door de gebruiker gedefinieerde functies kunnen worden geschreven in Java script. Hulp methoden communiceren met de grafiek in de door de gebruiker gedefinieerde uitvoerings omgeving. Ontwikkel aars kunnen aangepaste code fragmenten uitvoeren op Sens-metrie-berichten. Voorbeelden zijn:

- Stel de sensor direct in op het sensor object in de grafiek.
- Een actie uitvoeren op basis van verschillende sensor aflezingen binnen een ruimte in de grafiek.
- Een melding maken wanneer aan bepaalde voor waarden wordt voldaan voor een binnenkomende sensor Lees bewerking.
- Voeg de meta gegevens van de grafiek toe aan de lees van de sensor voordat u een melding verzendt.

Lees voor meer informatie [over het gebruik van door de gebruiker gedefinieerde functies](./how-to-user-defined-functions.md).

#### <a name="examples"></a>Voorbeelden

De [github-opslag plaats voor het digitale Apparaatdubbels C#-voor beeld](https://github.com/Azure-Samples/digital-twins-samples-csharp/) bevat enkele voor beelden van de door de gebruiker gedefinieerde functies:
- [Deze functie](https://github.com/Azure-Samples/digital-twins-samples-csharp/blob/master/occupancy-quickstart/src/actions/userDefinedFunctions/availabilityForTutorial.js) zoekt naar kool dioxide-, bewegings-en temperatuur waarden om te bepalen of een ruimte beschikbaar is met deze waarden in het bereik. De [zelf studies voor Digital apparaatdubbels](tutorial-facilities-udf.md) verkennen deze functie in meer details. 
- [Deze functie](https://github.com/Azure-Samples/digital-twins-samples-csharp/blob/master/occupancy-quickstart/src/actions/userDefinedFunctions/multiplemotionsensors.js) zoekt naar gegevens uit meerdere bewegings Sens oren en bepaalt of de ruimte beschikbaar is als geen enkele beweging kan worden gedetecteerd. U kunt de door de gebruiker gedefinieerde functie die in de [Snelstartgids](quickstart-view-occupancy-dotnet.md)of de [zelf studies](tutorial-facilities-setup.md)wordt gebruikt, gemakkelijk vervangen door de wijzigingen aan te brengen die worden vermeld in de sectie opmerkingen van het bestand. 

### <a name="role-assignment"></a>Nieuwe roltoewijzing

De acties van een door de gebruiker gedefinieerde functie zijn onderhevig aan op [rollen gebaseerd toegangs beheer van](./security-role-based-access-control.md) Azure Digital apparaatdubbels voor het beveiligen van gegevens in de service. Met roltoewijzingen definieert u welke door de gebruiker gedefinieerde functies de juiste machtigingen hebben om te communiceren met de ruimtelijke grafiek en de bijbehorende entiteiten. Een door de gebruiker gedefinieerde functie kan bijvoorbeeld de mogelijkheid hebben om grafiek gegevens te *maken*, *lezen*, *bijwerken*of *verwijderen* onder een bepaalde ruimte. Het toegangs niveau van een door de gebruiker gedefinieerde functie wordt gecontroleerd wanneer de door de gebruiker gedefinieerde functie de grafiek voor gegevens aanvraagt of een actie probeert uit te proberen. Lees op [rollen gebaseerd toegangs beheer](./security-create-manage-role-assignments.md)voor meer informatie.

Het is mogelijk dat een Matcher een door de gebruiker gedefinieerde functie moet activeren die geen roltoewijzingen heeft. In dit geval kan de door de gebruiker gedefinieerde functie geen gegevens uit de grafiek lezen.

## <a name="next-steps"></a>Volgende stappen

- Lees [route gebeurtenissen en berichten](./concepts-events-routing.md)voor meer informatie over het door sturen van gebeurtenissen en telemetrie-berichten naar andere Azure-Services.

- Meer informatie over het maken van matchers, door de gebruiker gedefinieerde functies en roltoewijzingen vindt u in de [hand leiding voor het gebruik van door de gebruiker gedefinieerde functies](./how-to-user-defined-functions.md).

- Bekijk de referentie documentatie voor de door de [gebruiker gedefinieerde functie-client bibliotheek](./reference-user-defined-functions-client-library.md).

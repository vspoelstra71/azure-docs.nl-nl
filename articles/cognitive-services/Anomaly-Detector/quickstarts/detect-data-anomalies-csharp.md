---
title: 'Snelstartgids: afwijkingen in uw tijdreeks gegevens detecteren met behulp van de anomalie detectie REST API en C #'
titleSuffix: Azure Cognitive Services
description: Meer informatie over hoe u de Anomaliey detector API kunt gebruiken om afwijkingen in uw gegevens reeksen op te sporen als een batch of gegevens stromen.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: anomaly-detector
ms.topic: quickstart
ms.date: 03/24/2020
ms.author: aahi
ms.openlocfilehash: b53fecad3655048a7b9d799134926b2730b16dae
ms.sourcegitcommit: 34a6fa5fc66b1cfdfbf8178ef5cdb151c97c721c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/28/2020
ms.locfileid: "80239113"
---
# <a name="quickstart-detect-anomalies-in-your-time-series-data-using-the-anomaly-detector-rest-api-and-c"></a>Snelstartgids: afwijkingen in uw tijdreeks gegevens detecteren met behulp van de anomalie detectie REST API en C # 

Gebruik deze Quick Start om de twee detectie modi van de anomalie detectie-API te gebruiken voor het detecteren van afwijkingen in uw time series-gegevens. Deze C#-toepassing verzendt twee API-aanvragen met tijdreeks gegevens in JSON-indeling en ontvangt de antwoorden.

| API-aanvraag                                        | Toepassingsuitvoer                                                                                                                                         |
|----------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Afwijkingen als een batch detecteren                        | Het JSON-antwoord met de afwijkings status (en andere gegevens) voor elk gegevens punt in de tijdreeks gegevens en de posities van gedetecteerde afwijkingen. |
| De afwijkings status van het laatste gegevens punt detecteren | Het JSON-antwoord met de afwijkings status (en andere gegevens) voor het laatste gegevens punt in de time series-gegevens.                                        |

 Hoewel deze toepassing in C# is geschreven, is de API een RESTful-webservice die compatibel is met vrijwel elke programmeertaal. U vindt de bron code voor deze Quick Start op [github](https://github.com/Azure-Samples/AnomalyDetector/blob/master/quickstarts/csharp-detect-anomalies.cs).

## <a name="prerequisites"></a>Vereisten

- Elke versie van [Visual Studio 2017 of hoger](https://visualstudio.microsoft.com/downloads/),
- Een anomalie detectie sleutel en eind punt
- Het [JSON.net](https://www.newtonsoft.com/json) -Framework, beschikbaar als een NuGet-pakket. Newton soft. json installeren als een NuGet-pakket in Visual Studio:
    
    1. Klik met de rechter muisknop op uw project in **Solution Explorer**.
    2. Selecteer **NuGet-pakketten beheren**.
    3. Zoek naar *Newton soft. json* en installeer het pakket.

- Als u Linux/MacOS gebruikt, kan deze toepassing worden uitgevoerd met behulp van [mono](https://www.mono-project.com/).

- Een JSON-bestand met gegevens punten van de tijd reeks. De voorbeeld gegevens voor deze Quick Start vindt u op [github](https://github.com/Azure-Samples/anomalydetector/blob/master/example-data/request-data.json).

### <a name="create-an-anomaly-detector-resource"></a>Een afwijkende detector-resource maken

[!INCLUDE [anomaly-detector-resource-creation](../../../../includes/cognitive-services-anomaly-detector-resource-cli.md)]

## <a name="create-a-new-application"></a>Een nieuwe toepassing maken

1. Maak in Visual Studio een nieuwe console oplossing en voeg de volgende pakketten toe. 

    [!code-csharp[using statements](~/samples-anomaly-detector/quickstarts/csharp-detect-anomalies.cs?name=usingStatements)]


2. Maak variabelen voor uw abonnements sleutel en uw eind punt. Hieronder vindt u de Uri's die u voor anomalie detectie kunt gebruiken. Deze worden later toegevoegd aan uw service-eind punt om de API-aanvraag-Url's te maken.

    | Detectiemethode                   | URI                                              |
    |------------------------------------|--------------------------------------------------|
    | Batch detectie                    | `/anomalydetector/v1.0/timeseries/entire/detect` |
    | Detectie op het laatste gegevens punt | `/anomalydetector/v1.0/timeseries/last/detect`   |
        
    [!code-csharp[initial variables for endpoint, key and data file](~/samples-anomaly-detector/quickstarts/csharp-detect-anomalies.cs?name=vars)]

## <a name="create-a-function-to-send-requests"></a>Een functie maken om aanvragen te verzenden

1. Maak een nieuwe async-functie `Request` met de naam die de hierboven gemaakte variabelen accepteert.

2. Het beveiligings protocol en de header gegevens van de client instellen `HttpClient` met behulp van een-object. Zorg ervoor dat u uw abonnements sleutel aan de `Ocp-Apim-Subscription-Key` koptekst toevoegt. Maak vervolgens een `StringContent` object voor de aanvraag.

3. Verzend de aanvraag met `PostAsync()`en retour neer het antwoord.

    [!code-csharp[Request method](~/samples-anomaly-detector/quickstarts/csharp-detect-anomalies.cs?name=requestMethod)]

## <a name="detect-anomalies-as-a-batch"></a>Afwijkingen als een batch detecteren

1. Maak een nieuwe functie met `detectAnomaliesBatch()`de naam. Maak de aanvraag en verzend deze door de `Request()` functie aan te roepen met het eind punt, de abonnements sleutel, de URL voor de batch afwijkings detectie en de tijdreeks gegevens.

2. Deserialiseren van het JSON-object en schrijf het naar de-console.

3. Als het antwoord veld `code` bevat, drukt u de fout code en het fout bericht af. 

4. Als dat niet het geval is, kunt u de positie van afwijkingen vinden in de gegevensset. Het `isAnomaly` veld van de reactie bevat een matrix met Boole-waarden, die elk aangeeft of een gegevens punt een afwijkend is. Converteer deze naar een teken reeks matrix met de functie van `ToObject<bool[]>()` het Response-object. Herhaal de matrix en druk de index van alle `true` waarden af. Deze waarden komen overeen met de index van afwijkende gegevens punten, als deze zijn gevonden.

    [!code-csharp[Detect anomalies batch](~/samples-anomaly-detector/quickstarts/csharp-detect-anomalies.cs?name=detectAnomaliesBatch)]


## <a name="detect-the-anomaly-status-of-the-latest-data-point"></a>De afwijkings status van het laatste gegevens punt detecteren

1. Maak een nieuwe functie met `detectAnomaliesLatest()`de naam. Maak de aanvraag en verzend deze door de `Request()` functie aan te roepen met uw eind punt, abonnements sleutel, de URL voor de laatste detectie van punt afwijkingen en de tijdreeks gegevens.

2. Deserialiseren van het JSON-object en schrijf het naar de-console.

    [!code-csharp[Detect anomalies latest](~/samples-anomaly-detector/quickstarts/csharp-detect-anomalies.cs?name=detectAnomaliesLatest)]
 
## <a name="load-your-time-series-data-and-send-the-request"></a>Laad uw time series-gegevens en verzend de aanvraag

1. In de hoofd methode van uw toepassing laadt u uw JSON Time Series- `File.ReadAllText()`gegevens met. 

2. Roep de hierboven gemaakte anomalie detectie functies aan. Gebruiken `System.Console.ReadKey()` om te voor komen dat het console venster wordt geopend nadat de toepassing is uitgevoerd.

    [!code-csharp[Main method](~/samples-anomaly-detector/quickstarts/csharp-detect-anomalies.cs?name=main)]

### <a name="example-response"></a>Voorbeeld van een antwoord

Een geslaagde reactie wordt geretourneerd in JSON-indeling. Klik op de onderstaande koppelingen om het JSON-antwoord op GitHub weer te geven:
* [Voor beeld van een antwoord op een batch detectie](https://github.com/Azure-Samples/anomalydetector/blob/master/example-data/batch-response.json)
* [Voor beeld van laatste punt detectie respons](https://github.com/Azure-Samples/anomalydetector/blob/master/example-data/latest-point-response.json)

[!INCLUDE [anomaly-detector-next-steps](../includes/quickstart-cleanup-next-steps.md)]

---
title: bestand opnemen
description: bestand opnemen
services: cognitive-services
author: diberry
manager: nitinme
ms.service: cognitive-services
ms.subservice: language-understanding
ms.date: 05/28/2020
ms.topic: include
ms.custom: include file
ms.author: diberry
ms.openlocfilehash: 8aeb6b964ab38a68a6d8681a4e5c93e1650c6a69
ms.sourcegitcommit: 1692e86772217fcd36d34914e4fb4868d145687b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/29/2020
ms.locfileid: "84171266"
---
Gebruik de voor Spellings-client bibliotheek van Language Understanding (LUIS) voor python voor het volgende:

* Voor spelling per sleuf ophalen
* Voor spellingen ophalen op basis van versie

[Referentie documentatie](https://docs.microsoft.com/python/api/azure-cognitiveservices-language-luis/index?view=azure-python)  |  [Bron code](https://github.com/Azure/azure-sdk-for-python/tree/master/sdk/cognitiveservices/azure-cognitiveservices-language-luis/azure/cognitiveservices/language/luis)  |  van bibliotheek [Voorspellings runtime pakket (PyPi)](https://pypi.org/project/azure-cognitiveservices-language-luis/)  |  Voor [beelden](https://github.com/Azure-Samples/cognitive-services-quickstart-code/tree/master/python/LUIS)

## <a name="prerequisites"></a>Vereisten

* Language Understanding-Portal account (LUIS): [Maak er gratis een](https://www.luis.ai) .
* [Python 3. x](https://www.python.org/)
* Een LUIS-App-ID: gebruik de open bare IoT-App-ID van `df67dcdb-c37d-46af-88e1-8b97951ca1c2` . De gebruikers query die in de Quick Start code wordt gebruikt, is specifiek voor die app.

## <a name="setting-up"></a>Instellen

### <a name="get-your-language-understanding-luis-runtime-key"></a>De runtime sleutel van uw Language Understanding (LUIS) ophalen

Down load uw [runtime sleutel](../luis-how-to-azure-subscription.md) door een Luis-runtime bron te maken. Behoud uw sleutel en het eind punt van de sleutel voor de volgende stap.

[!INCLUDE [Set up environment variables for prediction quickstart](sdk-prediction-environment-variables.md)]

### <a name="create-a-new-python-file"></a>Een nieuw python-bestand maken

Maak een nieuw python-bestand in uw voorkeurs editor of IDE, met de naam `prediction_quickstart.py` .

### <a name="install-the-sdk"></a>De SDK installeren

Installeer in de toepassingsmap de Language Understanding (LUIS) prediction runtime-client bibliotheek voor python met de volgende opdracht:

```python
python -m pip install azure-cognitiveservices-language-luis
```

## <a name="object-model"></a>Object model

De Language Understanding (LUIS) prediction runtime-client is een [LUISRuntimeClient](https://docs.microsoft.com/python/api/azure-cognitiveservices-language-luis/azure.cognitiveservices.language.luis.runtime.luisruntimeclient?view=azure-python) -object dat wordt geverifieerd bij Azure, dat de bron sleutel bevat.

Nadat de client is gemaakt, gebruikt u deze client voor toegang tot de functionaliteit, waaronder:

* Voor spelling door [staging of productie sleuf](https://docs.microsoft.com/python/api/azure-cognitiveservices-language-luis/azure.cognitiveservices.language.luis.runtime.operations.predictionoperations?view=azure-python#get-slot-prediction-app-id--slot-name--prediction-request--verbose-none--show-all-intents-none--log-none--custom-headers-none--raw-false----operation-config-)
* Voor spelling per [versie](https://docs.microsoft.com/python/api/azure-cognitiveservices-language-luis/azure.cognitiveservices.language.luis.runtime.operations.predictionoperations?view=azure-python#get-version-prediction-app-id--version-id--prediction-request--verbose-none--show-all-intents-none--log-none--custom-headers-none--raw-false----operation-config-)

## <a name="code-examples"></a>Codevoorbeelden

Deze code fragmenten laten zien hoe u het volgende kunt doen met de Language Understanding (LUIS) prediction runtime-client bibliotheek voor python:

* [Voor spelling per sleuf](#get-prediction-from-runtime)

## <a name="add-the-dependencies"></a>De afhankelijkheden toevoegen

Open het bestand in de map van het project `prediction_quickstart.py` in uw voorkeurs editor of IDE. Voeg de volgende afhankelijkheden toe:

[!code-python[Dependency statements](~/cognitive-services-quickstart-code/python/LUIS/python-sdk-authoring-prediction/prediction_quickstart.py?name=Dependencies)]

## <a name="authenticate-the-client"></a>De client verifiëren

1. Maak variabelen voor uw eigen vereiste LUIS-informatie:

    Voeg variabelen toe om de Voorspellings sleutel te beheren die wordt opgehaald uit een omgevings variabele met de naam `LUIS_RUNTIME_KEY` . Als u de omgevings variabele hebt gemaakt nadat de toepassing is gestart, moet de editor, IDE of shell die deze uitvoert, worden gesloten en opnieuw worden geladen om toegang te krijgen tot de variabele. De methoden worden later gemaakt.

    Maak een variabele voor de naam van uw resource `LUIS_RUNTIME_ENDPOINT` .

    [!code-python[Dependency statements](~/cognitive-services-quickstart-code/python/LUIS/python-sdk-authoring-prediction/prediction_quickstart.py?name=AuthorizationVariables)]

1. Maak een variabele voor de App-ID als een omgevings variabele met de naam `LUIS_APP_ID` . Stel de omgevings variabele in op de open bare IoT-app **`df67dcdb-c37d-46af-88e1-8b97951ca1c2`** . Maak een variabele om de gepubliceerde sleuf in te stellen `production` .

    [!code-python[Dependency statements](~/cognitive-services-quickstart-code/python/LUIS/python-sdk-authoring-prediction/prediction_quickstart.py?name=OtherVariables)]


1. Maak een referenties-object met uw sleutel en gebruik dit met uw eind punt om een [LUISRuntimeClientConfiguration]-object te maken https://docs.microsoft.com/python/api/azure-cognitiveservices-language-luis/azure.cognitiveservices.language.luis.runtime.luisruntimeclientconfiguration?view=azure-python() .

    [!code-python[Dependency statements](~/cognitive-services-quickstart-code/python/LUIS/python-sdk-authoring-prediction/prediction_quickstart.py?name=Client)]

## <a name="get-prediction-from-runtime"></a>Voor spelling van runtime ophalen

Voeg de volgende methode toe om de aanvraag te maken voor de Voorspellings runtime.

De gebruiker utterance maakt deel uit van het [prediction_request](https://docs.microsoft.com/python/api/azure-cognitiveservices-language-luis/azure.cognitiveservices.language.luis.runtime.models.predictionrequest?view=azure-python) -object.

De **[get_slot_prediction](https://docs.microsoft.com/python/api/azure-cognitiveservices-language-luis/azure.cognitiveservices.language.luis.runtime.operations.predictionoperations?view=azure-python#get-slot-prediction-app-id--slot-name--prediction-request--verbose-none--show-all-intents-none--log-none--custom-headers-none--raw-false----operation-config-)** methode heeft verschillende para meters nodig, zoals de App-ID, de naam van de sleuf en het aanvraag object voor de voor spelling om te voldoen aan de aanvraag. De andere opties, zoals uitgebreid, alle intenten weer geven en logboeken zijn optioneel. De aanvraag retourneert een [PredictionResponse](https://docs.microsoft.com/python/api/azure-cognitiveservices-language-luis/azure.cognitiveservices.language.luis.runtime.models.predictionresponse?view=azure-python) -object.

[!code-python[Dependency statements](~/cognitive-services-quickstart-code/python/LUIS/python-sdk-authoring-prediction/prediction_quickstart.py?name=predict)]

## <a name="main-code-for-the-prediction"></a>Hoofd code voor de voor spelling

Gebruik de volgende methode om de variabelen en methoden samen te koppelen om de voor spelling te verkrijgen.

```python
predict(luisAppID, luisSlotName)
```
## <a name="run-the-application"></a>De toepassing uitvoeren

Voer de toepassing uit met de `python prediction_quickstart.py` opdracht uit de toepassingsmap.

```console
python prediction_quickstart.py
```

In de Quick Start-console wordt de uitvoer weer gegeven:

```console
Top intent: HomeAutomation.TurnOn
Sentiment: None
Intents:
        "HomeAutomation.TurnOn"
Entities: {'HomeAutomation.Operation': ['on']}
```

## <a name="clean-up-resources"></a>Resources opschonen

Wanneer u klaar bent met de voor spelling, kunt u het werk van deze Quick Start opschonen door het bestand en de submappen ervan te verwijderen.

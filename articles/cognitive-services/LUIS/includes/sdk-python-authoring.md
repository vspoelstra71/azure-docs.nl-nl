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
ms.openlocfilehash: 96a2268da0e0a01f5b4c0cdba094ccb78da979a2
ms.sourcegitcommit: 1692e86772217fcd36d34914e4fb4868d145687b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/29/2020
ms.locfileid: "84171332"
---
Gebruik de Language Understanding (LUIS)-ontwerp-client bibliotheek voor python voor het volgende:

* Een app maken.
* Voeg intenties, entiteiten en voor beeld-uitingen toe.
* Voeg onderdelen, zoals een woordgroepen lijst, toe.
* Train en publiceer een app.

[Referentie documentatie](https://docs.microsoft.com/python/api/azure-cognitiveservices-language-luis/index?view=azure-python)  |  [Bron code](https://github.com/Azure/azure-sdk-for-python/tree/master/sdk/cognitiveservices/azure-cognitiveservices-language-luis/azure/cognitiveservices/language/luis)  |  van bibliotheek [Ontwerp pakket (pypi)](https://pypi.org/project/azure-cognitiveservices-language-luis/)  |  Voor [beelden](https://github.com/Azure-Samples/cognitive-services-quickstart-code/blob/master/python/LUIS/application_quickstart.py)

## <a name="prerequisites"></a>Vereisten

* Azure-abonnement: [Maak er gratis een](https://azure.microsoft.com/free/)
* De huidige versie van [python 3. x](https://www.python.org/).
* Wanneer u uw Azure-abonnement hebt, [maakt u een language Understanding-ontwerp bron](https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesLUISAllInOne) in de Azure Portal om uw sleutel en eind punt op te halen. Wacht tot de implementatie is geïmplementeerd en klik op de knop **naar resource** .
    * U hebt de sleutel en het eind punt nodig van de resource die u [maakt](../luis-how-to-azure-subscription.md#create-luis-resources-in-azure-portal) om uw toepassing te koppelen aan language Understanding-ontwerp. U plakt uw sleutel en het eind punt in de onderstaande code verderop in de Quick Start. U kunt de gratis prijs categorie ( `F0` ) gebruiken om de service te proberen.

## <a name="setting-up"></a>Instellen

### <a name="install-the-python-library-for-luis"></a>Installeer de python-bibliotheek voor LUIS

Installeer in de toepassingsmap de Language Understanding (LUIS) ontwerp-client bibliotheek voor python met de volgende opdracht:

```console
pip install azure-cognitiveservices-language-luis
```

## <a name="object-model"></a>Object model

De ontwerp-client voor Language Understanding (LUIS) is een [LUISAuthoringClient](https://docs.microsoft.com/python/api/azure-cognitiveservices-language-luis/azure.cognitiveservices.language.luis.authoring.luisauthoringclient?view=azure-python) -object dat wordt geverifieerd bij Azure, dat uw ontwerp sleutel bevat.

Nadat de client is gemaakt, gebruikt u deze client voor toegang tot de functionaliteit, waaronder:

* Apps: [maken](https://docs.microsoft.com/python/api/azure-cognitiveservices-language-luis/azure.cognitiveservices.language.luis.authoring.operations.appsoperations?view=azure-python#add-application-create-object--custom-headers-none--raw-false----operation-config-), [verwijderen](https://docs.microsoft.com/python/api/azure-cognitiveservices-language-luis/azure.cognitiveservices.language.luis.authoring.operations.appsoperations?view=azure-python#delete-app-id--force-false--custom-headers-none--raw-false----operation-config-), [publiceren](https://docs.microsoft.com/python/api/azure-cognitiveservices-language-luis/azure.cognitiveservices.language.luis.authoring.operations.appsoperations?view=azure-python#publish-app-id--version-id-none--is-staging-false--custom-headers-none--raw-false----operation-config-)
* Voor beeld van uitingen- [toevoegen per batch](https://docs.microsoft.com/python/api/azure-cognitiveservices-language-luis/azure.cognitiveservices.language.luis.authoring.operations.examplesoperations?view=azure-python#batch-app-id--version-id--example-label-object-array--custom-headers-none--raw-false----operation-config-), [verwijderen op id](https://docs.microsoft.com/python/api/azure-cognitiveservices-language-luis/azure.cognitiveservices.language.luis.authoring.operations.examplesoperations?view=azure-python#delete-app-id--version-id--example-id--custom-headers-none--raw-false----operation-config-)
* Functies: [woordgroepen lijsten](https://docs.microsoft.com/python/api/azure-cognitiveservices-language-luis/azure.cognitiveservices.language.luis.authoring.operations.featuresoperations?view=azure-python#add-phrase-list-app-id--version-id--phraselist-create-object--custom-headers-none--raw-false----operation-config-) beheren
* Model- [intents](https://docs.microsoft.com/python/api/azure-cognitiveservices-language-luis/azure.cognitiveservices.language.luis.authoring.operations.modeloperations?view=azure-python#add-intent-app-id--version-id--name-none--custom-headers-none--raw-false----operation-config-) en entiteiten beheren
* Patroon: [patronen](https://docs.microsoft.com/python/api/azure-cognitiveservices-language-luis/azure.cognitiveservices.language.luis.authoring.operations.patternoperations?view=azure-python#add-pattern-app-id--version-id--pattern-none--intent-none--custom-headers-none--raw-false----operation-config-) beheren
* [Train de app](https://docs.microsoft.com/python/api/azure-cognitiveservices-language-luis/azure.cognitiveservices.language.luis.authoring.operations.trainoperations?view=azure-python#train-version-app-id--version-id--custom-headers-none--raw-false----operation-config-) en vraag naar [trainings status](https://docs.microsoft.com/python/api/azure-cognitiveservices-language-luis/azure.cognitiveservices.language.luis.authoring.operations.trainoperations?view=azure-python#get-status-app-id--version-id--custom-headers-none--raw-false----operation-config-)
* [Versies](https://docs.microsoft.com/python/api/azure-cognitiveservices-language-luis/azure.cognitiveservices.language.luis.authoring.operations.versionsoperations?view=azure-python) : beheren met klonen, exporteren en verwijderen


## <a name="code-examples"></a>Codevoorbeelden

Deze code fragmenten laten zien hoe u het volgende kunt doen met de Language Understanding (LUIS) ontwerp-client bibliotheek voor python:

* [Een app maken](#create-a-luis-app)
* [Entiteiten toevoegen](#create-entities-for-the-app)
* [Intents toevoegen](#create-intent-for-the-app)
* [Voorbeelden van utterances toevoegen](#add-example-utterance-to-intent)
* [De app trainen](#train-the-app)
* [De app publiceren](#publish-a-language-understanding-app)

## <a name="create-a-new-python-application"></a>Een nieuwe python-toepassing maken

Maak een nieuwe python-toepassing in uw voorkeurs editor of IDE. Importeer vervolgens de volgende bibliotheken.

[!code-python[Create a new Python application in your preferred editor or IDE.](~/cognitive-services-quickstart-code/python/LUIS/python-sdk-authoring-prediction/application_quickstart.py?name=Dependencies)]

Maak variabelen voor het Azure-eind punt en de sleutel van uw resource. Als u de omgevings variabele hebt gemaakt nadat u de toepassing hebt gestart, moet u de editor, IDE of shell waarmee deze wordt uitgevoerd, sluiten en opnieuw openen om toegang te krijgen tot de variabele.

[!code-python[Create variables for your resource's Azure endpoint and key.](~/cognitive-services-quickstart-code/python/LUIS/python-sdk-authoring-prediction/application_quickstart.py?name=AuthorizationVariables)]

## <a name="authenticate-the-client"></a>De client verifiëren

Maak een [CognitiveServicesCredentials](https://docs.microsoft.com/python/api/msrest/msrest.authentication.cognitiveservicescredentials?view=azure-python) -object met uw sleutel en gebruik het met uw eind punt om een [LUISAuthoringClient](https://docs.microsoft.com/python/api/azure-cognitiveservices-language-luis/azure.cognitiveservices.language.luis.authoring.luisauthoringclient?view=azure-python) -object te maken.

[!code-python[Create LUIS client object](~/cognitive-services-quickstart-code/python/LUIS/python-sdk-authoring-prediction/application_quickstart.py?name=Client)]

## <a name="create-a-luis-app"></a>Een LUIS-app maken

1. Maak een LUIS-app die de intenties, entiteiten en voorbeeld uitingen van het model voor natuurlijke taal verwerking (NLP) bevat.

1. De [add](https://docs.microsoft.com/python/api/azure-cognitiveservices-language-luis/azure.cognitiveservices.language.luis.authoring.operations.appsoperations?view=azure-python#add-application-create-object--custom-headers-none--raw-false----operation-config-) -methode van een [AppsOperation](https://docs.microsoft.com/python/api/azure-cognitiveservices-language-luis/azure.cognitiveservices.language.luis.authoring.operations.appsoperations?view=azure-python) -object maken om de app te maken. De naam en taal cultuur zijn vereiste eigenschappen.

    [!code-python[Create LUIS client app](~/cognitive-services-quickstart-code/python/LUIS/python-sdk-authoring-prediction/application_quickstart.py?name=createApp)]


## <a name="create-intent-for-the-app"></a>Intentie maken voor de app
Het primaire object in het model van een LUIS-app is het doel. De intentie Lijnt uit met een groepering van gebruikers utterance _voornemens_. Een gebruiker kan een vraag stellen of een overzicht maken van een bepaalde _gewenste_ reactie van een bot (of een andere client toepassing). Voor beelden van bedoelingen zijn het reserveren van een vlucht, waarin wordt gevraagd om de weer gave van een bestemmings plaats en om contact op te nemen met de klanten service.

Gebruik de methode [model. add_intent](https://docs.microsoft.com/python/api/azure-cognitiveservices-language-luis/azure.cognitiveservices.language.luis.authoring.operations.modeloperations?view=azure-python#add-intent-app-id--version-id--name-none--custom-headers-none--raw-false----operation-config-) met de naam van de unieke intentie en geef vervolgens de App-ID, de versie-id en de naam van de nieuwe intentie door.

[!code-python[Create LUIS client app](~/cognitive-services-quickstart-code/python/LUIS/python-sdk-authoring-prediction/application_quickstart.py?name=addIntents)]

## <a name="create-entities-for-the-app"></a>Entiteiten voor de app maken

Hoewel entiteiten niet vereist zijn, worden ze in de meeste apps gevonden. De entiteit extraheert informatie uit de utterance van de gebruiker, die nodig is om de bedoeling van de gebruiker te fullfilen. Er zijn verschillende typen [vooraf ontwikkelde](https://docs.microsoft.com/python/api/azure-cognitiveservices-language-luis/azure.cognitiveservices.language.luis.authoring.operations.modeloperations?view=azure-python#add-prebuilt-app-id--version-id--prebuilt-extractor-names--custom-headers-none--raw-false----operation-config-) en aangepaste entiteiten met hun eigen DTO-modellen (Data Transformation object).  Algemene vooraf gemaakte entiteiten die u wilt toevoegen aan uw app, zijn [Number](../luis-reference-prebuilt-number.md), [datetimeV2](../luis-reference-prebuilt-datetimev2.md), [geographyV2](../luis-reference-prebuilt-geographyv2.md), [Ordinal](../luis-reference-prebuilt-ordinal.md).

Met deze **add_entities** methode `Location` is een eenvoudige entiteit gemaakt met twee rollen, een `Class` eenvoudige entiteit, een `Flight` samengestelde entiteit en worden verschillende vooraf gemaakte entiteiten toegevoegd.

Het is belang rijk te weten dat entiteiten niet zijn gemarkeerd met een bedoeling. Ze kunnen en meestal worden toegepast op veel intenties. Alleen voor beelden van gebruikers uitingen zijn gemarkeerd voor een specifiek, enkelvoudig doel.

De methode voor het maken van entiteiten maakt deel uit van de [ModelOperations](https://docs.microsoft.com/python/api/azure-cognitiveservices-language-luis/azure.cognitiveservices.language.luis.authoring.operations.modeloperations?view=azure-python) -klasse. Elk entiteits type heeft een eigen DTO-model (Data Transformation object).

[!code-python[Create LUIS client app](~/cognitive-services-quickstart-code/python/LUIS/python-sdk-authoring-prediction/application_quickstart.py?name=addEntities)]

## <a name="add-example-utterance-to-intent"></a>Bijvoorbeeld utterance toevoegen aan intentie

De app heeft voor beelden van uitingen nodig om de voor delen en het ophalen van entiteiten van een utterance te bepalen. De voor beelden moeten gericht zijn op een specifieke, afzonderlijke intentie en alle aangepaste entiteiten moeten worden gemarkeerd. Vooraf gemaakte entiteiten hoeven niet te worden gemarkeerd.

Een voor beeld van een uitingen toevoegen door een lijst met [ExampleLabelObject](https://docs.microsoft.com/python/api/azure-cognitiveservices-language-luis/azure.cognitiveservices.language.luis.authoring.models.examplelabelobject?view=azure-python) -objecten te maken, één object voor elk voor beeld utterance. Elk voor beeld moet alle entiteiten markeren met een woorden lijst met naam/waarde-paren van de naam van de entiteit en de waarde van de entiteit. De waarde van de entiteit moet exact zo zijn als deze wordt weer gegeven in de tekst van het voor beeld utterance.

Call- [voor beelden. batch](https://docs.microsoft.com/python/api/azure-cognitiveservices-language-luis/azure.cognitiveservices.language.luis.authoring.operations.examplesoperations?view=azure-python#batch-app-id--version-id--example-label-object-array--custom-headers-none--raw-false----operation-config-) met de App-ID, versie-id en de lijst met voor beelden. De aanroep reageert met een lijst met resultaten. U moet het resultaat van elk voor beeld controleren om er zeker van te zijn dat het is toegevoegd aan het model.

[!code-python[Add example utterances via a batch](~/cognitive-services-quickstart-code/python/LUIS/python-sdk-authoring-prediction/application_quickstart.py?name=addUtterances)]

## <a name="train-the-app"></a>De app trainen

Zodra het model is gemaakt, moet de LUIS-app worden getraind voor deze versie van het model. Een getraind model kan worden gebruikt in een [container](../luis-container-howto.md)of naar de staging-of product sleuven worden [gepubliceerd](../luis-how-to-publish-app.md) .

De methode [Train. train_version](https://docs.microsoft.com/python/api/azure-cognitiveservices-language-luis/azure.cognitiveservices.language.luis.authoring.operations.trainoperations?view=azure-python#train-version-app-id--version-id--custom-headers-none--raw-false----operation-config-) moet de App-ID en de versie-id hebben.

Een zeer klein model, zoals deze Quick Start laat zien, wordt zeer snel getraind. Voor toepassingen op productie niveau moet de training van de app een polling aanroep naar de [get_status](https://docs.microsoft.com/python/api/azure-cognitiveservices-language-luis/azure.cognitiveservices.language.luis.authoring.operations.trainoperations?view=azure-python#get-status-app-id--version-id--custom-headers-none--raw-false----operation-config-) methode bevatten om te bepalen wanneer de training is geslaagd. Het antwoord is een lijst met [ModelTrainingInfo](https://docs.microsoft.com/python/api/azure-cognitiveservices-language-luis/azure.cognitiveservices.language.luis.authoring.models.modeltraininginfo?view=azure-python) -objecten met een afzonderlijke status voor elk object. De training kan alleen volledig worden beschouwd als alle objecten zijn voltooid.

[!code-python[Train LUIS client app](~/cognitive-services-quickstart-code/python/LUIS/python-sdk-authoring-prediction/application_quickstart.py?name=train)]

## <a name="publish-a-language-understanding-app"></a>Een Language Understanding-app publiceren

Publiceer de LUIS-app met de methode [app. publish](https://docs.microsoft.com/python/api/azure-cognitiveservices-language-luis/azure.cognitiveservices.language.luis.authoring.operations.appsoperations?view=azure-python#publish-app-id--version-id-none--is-staging-false--custom-headers-none--raw-false----operation-config-) . Hiermee wordt de huidige getrainde versie gepubliceerd naar de opgegeven sleuf op het eind punt. Uw client toepassing gebruikt dit eind punt om gebruikers uitingen te verzenden voor het voors pellen van intentie en het uitpakken van entiteiten.

[!code-python[Publish LUIS client app](~/cognitive-services-quickstart-code/python/LUIS/python-sdk-authoring-prediction/application_quickstart.py?name=publish)]

## <a name="run-the-application"></a>De toepassing uitvoeren

Voer de toepassing uit met de `python` opdracht in uw Quick Start-bestand.

```console
python quickstart-file.py
```

## <a name="clean-up-resources"></a>Resources opschonen

Wanneer u klaar bent met de voor spelling, kunt u het werk van deze Quick Start opschonen door het bestand en de submappen ervan te verwijderen.
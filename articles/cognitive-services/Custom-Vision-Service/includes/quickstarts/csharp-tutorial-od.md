---
author: PatrickFarley
ms.author: pafarley
ms.service: cognitive-services
ms.date: 04/14/2020
ms.openlocfilehash: 67f8293329b8b091cdc4b0cbe534ced49ba69c7c
ms.sourcegitcommit: 34a6fa5fc66b1cfdfbf8178ef5cdb151c97c721c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/28/2020
ms.locfileid: "82134073"
---
In dit artikel wordt beschreven hoe u met behulp van de Custom Vision C# SDK aan de slag gaat met het maken van een object detectie model. Nadat u deze hebt gemaakt, kunt u gelabelde regio's toevoegen, afbeeldingen uploaden, het project trainen, de standaard eind punt-URL van het project ophalen en het eind punt gebruiken om een afbeelding programmatisch te testen. Gebruik dit voor beeld als een sjabloon voor het maken van uw eigen .NET-toepassing. 

## <a name="prerequisites"></a>Vereisten

- Een versie van [Visual Studio 2015 of 2017](https://www.visualstudio.com/downloads/)
- [!INCLUDE [create-resources](../../includes/create-resources.md)]

## <a name="get-the-custom-vision-sdk-and-sample-code"></a>De Custom Vision SDK en voorbeeld code ophalen

Als u een .NET-app wilt schrijven die gebruikmaakt van Custom Vision, hebt u de Custom Vision NuGet-pakketten nodig. Deze pakketten zijn opgenomen in het voorbeeld project dat u wilt downloaden, maar u kunt deze afzonderlijk openen.

- [Microsoft.Azure.CognitiveServices.Vision.CustomVision.Training](https://www.nuget.org/packages/Microsoft.Azure.CognitiveServices.Vision.CustomVision.Training/)
- [Microsoft.Azure.CognitiveServices.Vision.CustomVision.Prediction](https://www.nuget.org/packages/Microsoft.Azure.CognitiveServices.Vision.CustomVision.Prediction/)

Kloon of download het project [.NET-voorbeelden van Cognitive Services](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples). Ga naar de map **CustomVision/ObjectDetection** en open _ObjectDetection. csproj_ in Visual Studio.

Met dit Visual Studio-project wordt een nieuw Custom Vision-project gemaakt met de naam __My New Project__. Dit kan worden geopend via de [Custom Vision-website](https://customvision.ai/). Vervolgens worden afbeeldingen geüpload om een objectdetectiemodel te trainen en te testen. In dit project wordt het model getraind om vorken en scharen in afbeeldingen te detecteren.

[!INCLUDE [get-keys](../../includes/get-keys.md)]

## <a name="understand-the-code"></a>De code begrijpen

Open het bestand _Program.cs_ en inspecteer de code. [Omgevings variabelen maken](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account#configure-an-environment-variable-for-authentication) voor uw trainings-en Voorspellings `CUSTOM_VISION_TRAINING_KEY` sleutels `CUSTOM_VISION_PREDICTION_KEY`met de naam en. Het script zoekt naar deze variabelen.

[!code-csharp[](~/cognitive-services-dotnet-sdk-samples/CustomVision/ObjectDetection/Program.cs?name=snippet_keys)]

U kunt ook uw eind punt-URL ophalen via de pagina instellingen van de website van Custom Vision. Sla het bestand op in een omgevings variabele met de naam `CUSTOM_VISION_ENDPOINT`. Er wordt een verwijzing naar het script opgeslagen in de hoofdmap van uw klasse.

[!code-csharp[](~/cognitive-services-dotnet-sdk-samples/CustomVision/ObjectDetection/Program.cs?name=snippet_endpoint)]

### <a name="create-a-new-custom-vision-service-project"></a>Een nieuw Custom Vision Service project maken

Met het volgende deel van de code maakt u een objectdetectieproject. Het project wordt weergegeven op de [Custom Vision-website](https://customvision.ai/), die u eerder hebt bezocht. Raadpleeg de [CreateProject](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.vision.customvision.training.customvisiontrainingclientextensions.createproject?view=azure-dotnet#Microsoft_Azure_CognitiveServices_Vision_CustomVision_Training_CustomVisionTrainingClientExtensions_CreateProject_Microsoft_Azure_CognitiveServices_Vision_CustomVision_Training_ICustomVisionTrainingClient_System_String_System_String_System_Nullable_System_Guid__System_String_System_Collections_Generic_IList_System_String__) -methode om andere opties op te geven wanneer u uw project maakt (Zie de hand leiding voor het [bouwen van een detector](../../get-started-build-detector.md) -webportal).  

[!code-csharp[](~/cognitive-services-dotnet-sdk-samples/CustomVision/ObjectDetection/Program.cs?name=snippet_create)]


### <a name="add-tags-to-the-project"></a>Tags toevoegen aan het project

[!code-csharp[](~/cognitive-services-dotnet-sdk-samples/CustomVision/ObjectDetection/Program.cs?name=snippet_tags)]

### <a name="upload-and-tag-images"></a>Afbeeldingen uploaden en coderen

Wanneer u afbeeldingen in object detectie projecten labelt, moet u de regio van elk gelabeld object opgeven met behulp van genormaliseerde coördinaten. Met de volgende code wordt elk van de voorbeeld afbeeldingen gekoppeld aan de gelabelde regio.

> [!NOTE]
> Als u geen hulp programma voor klikken en slepen hebt om de coördinaten van regio's te markeren, kunt u de Web-UI gebruiken op [Customvision.ai](https://www.customvision.ai/). In dit voor beeld zijn de coördinaten al opgenomen.

[!code-csharp[](~/cognitive-services-dotnet-sdk-samples/CustomVision/ObjectDetection/Program.cs?name=snippet_upload_regions)]

Vervolgens wordt deze kaart met koppelingen gebruikt om elke voorbeeldafbeelding met de bijbehorende regiocoördinaten te uploaden. U kunt Maxi maal 64 installatie kopieën uploaden in één batch.

[!code-csharp[](~/cognitive-services-dotnet-sdk-samples/CustomVision/ObjectDetection/Program.cs?name=snippet_upload)]

U hebt nu alle voor beelden van afbeeldingen geüpload en elke afbeelding (**Fork** of **schaar**) voorzien van een gekoppelde pixel rechthoek.

### <a name="train-the-project"></a>Het project trainen

Deze code maakt de eerste trainings herhaling in het project.

[!code-csharp[](~/cognitive-services-dotnet-sdk-samples/CustomVision/ObjectDetection/Program.cs?name=snippet_train)]

### <a name="publish-the-current-iteration"></a>De huidige herhaling publiceren

De naam die is opgegeven voor de gepubliceerde herhaling kan worden gebruikt voor het verzenden van voorspellings aanvragen. Er is geen iteratie beschikbaar in het Voorspellings eindpunt totdat het is gepubliceerd.

[!code-csharp[](~/cognitive-services-dotnet-sdk-samples/CustomVision/ObjectDetection/Program.cs?name=snippet_publish)]

### <a name="create-a-prediction-endpoint"></a>Een Voorspellings eindpunt maken

[!code-csharp[](~/cognitive-services-dotnet-sdk-samples/CustomVision/ObjectDetection/Program.cs?name=snippet_prediction_endpoint)]

### <a name="use-the-prediction-endpoint"></a>Het Voorspellings eindpunt gebruiken

Dit deel van het script laadt de test installatie kopie, voert een query uit op het eind punt van het model en voert Voorspellings gegevens uit naar de-console.

[!code-csharp[](~/cognitive-services-dotnet-sdk-samples/CustomVision/ObjectDetection/Program.cs?name=snippet_prediction)]

## <a name="run-the-application"></a>De toepassing uitvoeren

Wanneer de toepassing wordt uitgevoerd, moet deze een console venster openen en de volgende uitvoer schrijven:

```console
Creating new project:
        Training
Done!

Making a prediction:
        fork: 98.2% [ 0.111609578, 0.184719115, 0.6607002, 0.6637112 ]
        scissors: 1.2% [ 0.112389535, 0.119195729, 0.658031344, 0.7023591 ]
```

U kunt vervolgens controleren of de test installatie kopie (gevonden in **installatie kopieën/test/** ) juist is gelabeld en of de detectie regio correct is. Op dit moment kunt u op een wille keurige toets drukken om de toepassing af te sluiten.

[!INCLUDE [clean-od-project](../../includes/clean-od-project.md)]

## <a name="next-steps"></a>Volgende stappen

U hebt nu gezien hoe u elke stap van het object detectie proces in code kunt uitvoeren. In dit voor beeld wordt één trainings iteratie uitgevoerd, maar vaak moet u het model meerdere keren trainen en testen om het nauw keuriger te maken. In de volgende handleiding wordt classificatie van afbeeldingen behandeld. De principes zijn soortgelijk aan die van objectdetectie.

> [!div class="nextstepaction"]
> [Een model testen en opnieuw trainen](../../test-your-model.md)
